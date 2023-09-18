---
title: querydsl에서 fetch join
created: 2023-09-18 15:03
type: trouble shooting
cssclass: cornell-left cornell-border
---
>[!summary] 
>- QueryDsl에서 on절 사용 시 fetchJoin이 작동하지 않는 이슈
>- on절은 join시점에 필터링과정을 거치이게 연관된 모든 엔티티를 불러와야 하느 fetchJoin이념과 맞지않다

# querydsl에서 fetch join

>[!cue] Issue

## 문제
- queryDsl에서 fetchJoin이 적용되지 않는 이슈
>[!cue] on절

```kotlin title:"Not FetchJoin" hl:3
fun findOneByIdJoin(id: Long): LeftEntity? {
    return queryFactory.selectFrom(leftEntity)
	    .leftJoin(rightEntity).on(rightEntity.left.id.eq(leftEntity.id))
	    .fetchJoin()           
	    .fetchOne()
}
```
- on절을 사용하게 되면 fetchJoin이 되지 않는다.

---
## 해결 방안
>[!cue] 원인

앞서 fetchJoin에 대해 의미 부터 파악하면 두 엔티티간에 연관관계가 형성되어있을 때 이를 조인시 연관된 모든 엔티티를 불러와 매핑하는 작업이다.

이 부분을 상기하고 on절을 확인해보자 기본적으로 on절은 join이 이루어질 때 필터링이 되는 작업이다.
즉 on절의 성격과 fetchJoin의 성격이 맞지 않다는걸 알 수 있다.

따라서 하이버네이트에서는 이를 사용할 시 fetchJoin이 아니 일반 join이 되도록 구현되어있다.


```kotlin title:FetchJoin hl:3
fun findOneById(id: Long): LeftEntity? {
	return queryFactory.selectFrom(leftEntity)
		.leftJoin(leftEntity.rights, rightEntity).fetchJoin()
		.where(leftEntity.id.eq(id))
		.fetchOne()
}
```
- join절 좌변에 EntityGrpah로 표시하고 우변에 alias를 지정해주면 된다.
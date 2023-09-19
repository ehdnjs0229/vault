---
title: querydsl에서 fetch join
created: 2023-09-18 15:03
type: trouble shooting
cssclass: cornell-left cornell-border
---
>[!summary] 
>- JPQL에서 fetchJoin과 on절은 함께 사용할 수 없다.
>- JPQL에서 on절은 join과정에서 필터링을 하기에 fetchJoin이념에 맞지않다.


# querydsl에서 fetch join사용시 주의사항

>[!cue] on절에 의한 fetchJoin미 적용 이슈

## 문제

queryDsl에서 fetchJoin시 on절을 사용하게 되면 fetchJoin이 되지 않는다.

```kotlin title:"Not FetchJoin" hl:3
fun findOneByIdJoin(id: Long): LeftEntity? {
    return queryFactory.selectFrom(leftEntity)
	    .leftJoin(rightEntity).on(rightEntity.left.id.eq(leftEntity.id))
	    .fetchJoin()           
	    .fetchOne()
}
```

---
## 해결 방안

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
join절 좌변에 EntityGrpah로 표시하고 우변에 alias를 지정해주면 된다.

---

>[!cue] fetchJoin선언 순서에 따른 이슈

## 문제

```kotlin title:"Not FetchJoin" hl:3
fun findOneByIdJoin(id: Long): LeftEntity? {
    return queryFactory.selectFrom(leftEntity)
	    .fetchJoin().leftJoin(leftEntity.rights, rightEntity)           
	    .fetchOne()
}
```
join앞에 fetchJoin을 사용할 시 `unexpected token: fetch` 발생

## 해결

fetchJoin은 join절 뒤에 붙여 사용해야 한다.

---

>[!cue] on절과 fetchJoin함께 사용 이슈슈

## 문제

```kotlin title:"on + fetchJoin" hl:2-3
leftJoin(leftEntity.rights, rightEntity)
.fetchJoin()
.on(rightEntity.left.id.eq(leftEntity.id))
```

```kotlin title:"on + fetchJoin" hl:2-3
leftJoin(leftEntity.rights, rightEntity)
.on(rightEntity.left.id.eq(leftEntity.id))
.fetchJoin()
```
on과 fetchJoin을 함께 사용할 시 `with-clause not allowed on fetched associations; use filters`에러가 발생한다.

> [!info] 첫 번째 문제와는 약간 다른게 leftJoin에 EntityGraph가 참조되냐, Entity가 직접 호출되냐 차이이다.

---

## 해결 방안

on절을 where절로 바꿔 사용한다.
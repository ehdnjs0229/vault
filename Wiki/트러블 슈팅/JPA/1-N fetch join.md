---
title: 1-N fetch join
created: 2023-09-18 10:44
type: trouble shooting
cssclass: cornell-left cornell-border
---
>[!summary] 
>- 1:N 관계에서 fetchJoin 사용시 반환타입에 따른 결과차이

# 1-N fetch join

>[!cue] Issue

## 문제
- 1:N 관계에서 fetchJoin시 반환형식에 따라 결과값이 달라짐
	- Entity
	- List\<Entity\>

---
## 해결 방안
>[!cue] 단건

- Entity : 단건 조회로 엔티티로 패키징되어 반환 됨

```kotlin title:"단건 반환" hl:2
@Query("select l from LeftEntity l left join fetch l.rights where l.id = :id")  
fun findOneById(id: Long): LeftEntity?
```

>[!cue] 다건

- List\<Entity\> : 다건 조회로 row수만큼 데이터가 늘어나서 반환 됨
	- distinct를 통해 중복건 제거하도록 수정

```kotlin title:"다건 반환" hl:2
@Query("select distinct l from LeftEntity l left join fetch l.rights where l.id = :id")  
fun findOneById(id: Long): List<LeftEntity>?
```

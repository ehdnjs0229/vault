---
title: Cache-Control
created: 2023-09-21 14:20
type: term
cssclass: cornell-left cornell-border
---
>[!summary] 
>- `Shared Cache에는 개인화된 콘텐츠`를 제공하면 안된다.
>- 원본 서버에서의 캐시 유효성 체크를 위해 must-revalidate을 사용해야 한다.
>- 캐시를 사용하고 싶지 않다면 `no-cache no-store must-revalidate`를 사용하자

# Cache-Control

캐시 만료기간, 활성 여부, 공유 혹은 개인화 등 캐시관리를 위해 설정할 수 있는 방법이다.

---

# 특징

##### 어휘

>[!cue] 어휘(Vocabulary)

1. **Shared Cache** : 원본서버와 클라이언트 사이에 존재하는 캐시(`CDN, 프록시`)로 `단일 응답을 저장`한 뒤 여러 `사용자에게 동일한 응답`을 보여준다. 따라서 `개인화된 콘텐츠를 캐싱하면 안된다.`
2. **Private Cache** : `로컬, 브라우저 캐시`로 `개인화된 콘텐츠를 저장`할 때 사용한다.
3. **Store Response** : `응답을 캐시할 수 있는경우 캐시`하나, 항상 재사용 되는것은 아니다.
4. **Revalidate Response** : `저장된 응답이 최신상태`인지 `원본 서버`에 물어본다.
5. **Fresh Response** : 응답이 fresh(최신) 상태임을 나타낸다.
6. **Stable Response**. : 응답이 indicate(부실) 상태로 다시 사용할 수 없음을 나타낸다.

---

##### 지시문

>[!cue] 지시문(Directives)

1. **max-age** : N초까지 응답이 최신 상태로 유지됨을 나타낸다.
	- 응답이 수신된 이후가 아닌, 서버에서 생성한 이후부터 시간이 흐른다.
	- 아래는 3600초 동안 캐싱
```http
Cache-Control: max-age=3600
```
2. **no-cache** : 캐싱은 하되, 매 번 원본서버한테 요청에 대한 유효성 검사를 요구한다.
	- 이름과 달리 캐싱을 하지않는 것이아니다.
	- 만약 원본서버와 연결이 되지않는다면, 프록시 서버의 캐싱 응답을 내려준다.
3. **must-revalidate** : 반드시 응답에 대한 유효성 검사를 해야하며 이 때 원본서버에 연결이 되지않는다면 504에러를 응답한다.
4. **no-store** : 모든 캐시가 응답을 저장하면 안된다.
5. **private** : 개인 캐시로 저장될 수 있음을 나타낸다.
6. **public** : 공유 캐시에 저장될 수 있음을 나타낸다.

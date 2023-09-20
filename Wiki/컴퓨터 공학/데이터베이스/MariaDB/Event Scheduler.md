---
title: Event Scheduler
created: 2023-09-12 14:41
type: term
cssclass: cornell-left cornell-border
---
>[!summary] 
>- 1번 혹은 그이상의 Task를 특정 시각에 맞추거나 주기적으로 실행있게 한다
>- 만약 이벤트가 실행 후 삭제되지 않길 원한다면 **on completion preserve**를 사용

# Event Scheduler

특정 Task를 `반복적`으로 또는 `특정 시각`에 맞춰 실행할 수 있다.

---
# 특징

event_scheduler가 정상적으로 동작하는지 확인하기 위해선 변수 설정을 해야 한다.
- **set global event_scheduler = ON** : 글로벌 변수 설정
- **show variables where variable_name = 'event_scheduler'** : 변수 확인
- **show events** : 이벤트 생성 확인

>[!cue] Syntax

```sql
CREATE [OR REPLACE]
	EVENT [IF NOT EXISTS] event_name
	ON SCHEDULE schedule [ON COMPLETION [NOT] PRESERVE]
	[ENABLE | DISABLE | DISABLE ON SLAVE]
	[COMMENT 'comment']
	DO sql_statement;

schedule:
	AT teimstamp [+ INTERVAL interval]
	| EVRY interval
	[STARTS timestamp [+ INTERVAL interval]]
	[ENDS timestamp [+ INTERVAL interval]]

interval:
	YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
	WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
	DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND
```
- **or replace** : 기존 이벤트를 삭제하고 대체
- **at** : 이벤트를 한번만 실행하려면 at 키워드 뒤 timestamp를 사용
	- `CURRENT_TIMESTAMP`시 생성즉시 호출
- **on completion [not] preserve** : 완료시 이벤트 보존여부
- **enable | disable** : 단순 이벤트 실행, 중지 기본값은 enable

>[!cue] Example

##### Example 1:

```sql
create event if not exists my_delete_event
on scheule
	every '1' day starts '2020-01-01'
do
	call proceduer
```
2020-01-01시점 이후 부터 매일 한 번씩 실행

##### Example 2:

```sql
create event if not exists my_delete_event
on scheule
	at '2020-01-01'
do
	call proceduer
```
최초 한번 실행

##### Example 3:

```sql
create event if not exists my_delete_event
on schedule
	starts '2020-01-01 12:00' interval 1 miniute
	ends '2020-01-01 12:10:00'
on completion preserve
enable
do
	call proceduer
```
2020-01-01 정오부터 10분까지 1분마다 실행하며, 이벤트가 종료될 시 삭제하지 않는다.
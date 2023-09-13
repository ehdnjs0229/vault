---
title: SoC
created: 2023-09-13 13:32
type: term
cssclass: cornell-left cornell-border
---
>[!summary] 
>- 여러 기능이 하나의 단일 칩에 구현되어 있다.
>- 각 모듈들을 직접 설계해야 하므로 비용이 많이 들어간다.

# SoC

System on Chip으로 `하나의 칩`안에 `여러 기능의 모듈`을 포함시킨다.

>[!cue] Apple Slicon

##### Example 1: Apple Slicon
하나의 칩에 CPU, GPU, RAM, SSD, Neural Engine 등의 기능을 하는 IC칩이 포함되어 있다.

>[!cue] [[SiP]]

##### Different 1: [[SiP]]
```txt
SiP는 칩을 패키징하는 것 이라면 SoC는 각 모듈들의 기능을 하나에 칩에 탑재시키는 것 이다.
```


>[!cue] MainBoard

##### Different 2: MainBoard
```txt
메인보드는 하나의 칩에 여러 장치들을 결합시키는 것으로 단일 칩에 기능을 포함시키는것이 아니기에 SoC라고 볼 수 없다.
```

> [!info] 하나로 패키징하는것도 아니기에 [[SiP]]또한 아니다.

---
# 특징

1. 성능 및 효율최적화가 가능하다.
2. 각 모듈들의 기능을 직접 설계 및 구현해야 하므로 개발 비용 및 유지보수 비용이 많이 들어간다.
3. 스케일 업이 불가능하다.
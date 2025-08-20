---
title:  "Niagara"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2025-07-21
last_modified_at: 2025-07-21
---

## Chapter 1 UE5 Series: The Beginner's Guide to Niagara

### 01-1 Basic
- FX의 종류는? Niagara Emitter와 Niagara System ( 02 : 45 )
- System Overview를 더 확대하고 싶으면? Ctrl을 누르고 스크롤링 한다 ( 05 : 09 )
- 주석 크기 고정하는 방법은? 압정버튼을 누른다 ( 06 : 13 )
- Stage의 Event Handler을 어떻게 활용할 수 있을까? 한 입자가 무언가를 수행하면 다른 이미터의 동작이 트리거 되게 할 수 있다 ( 09 : 21 )
- Niagara의 구동 방식은? 절차지향 ( 09 : 46 )

### 01-2 CREATING YOUR FIRST MODULE
- Emitter Update 에서 Emitter Life Cycle을 관리하는 모듈은? Emitter State ( 13 : 01 )

### 01-3 SPAWN MODULES
- Render 에서는 어떤것을 설정할 수 있는가? 렌더될 도형의 형태 ( 19 : 37 )

### 01-4 BASIC PARTICLE MODULES
- 파티클의 크기는 어디서 제어 가능한가? Particle Spawn ( 22 : 39 )
- 변수를 그래프로 조정할 수 있는 기능은? Float from Curve ( 27 : 34 )
- Float from Curve를 미세조정 하라면 어떻게 해야 하는가? 가운데 마우스 버튼으로 변곡점을 만들어 준다 ( 29 : 38 )
- Curve 탭에서는 무엇을 할 수 있는가? 변곡점들 보간 ( 29 : 58 )

### 01-5 NIAGARA SYSTEM
- Niagara System에 대해 설명하면? 여러 개의 Niagara Emitter를 저장할 수 있는 컨테이너와 같다 ( 30 : 44 )
- Niagara System에 Niagara Emitter를 불러오는 방법은? System Overview 탭에서 E 키를 누르고 Parent Emitters 에서 불러온다 ( 34 : 31 )
- Niagara Emitter를 활용해 Niagara System을 만드는 방법은? Niagara Emitter 우클릭후 Create Niagara System 클릭 ( 35 : 22 )

### 01-6 BASIC MOVING SPRITE
- Spawn Rate 모듈의 역할은? 파티클 생성 주기 관리 ( 37 : 07 )
- 입자를 움직이는 방법 두가지는? Add Velocity, Add Forces ( 38 : 13 )
- Add Velocity를 추가했을때 나오는 오류를 해결하려면? Solve Forces and Velocity 모듈을 추가해준다 ( 40 : 01 )
- 파티클을 랜덤한 위치에서 생성되게 하려면? Shape Location 모듈 활용 ( 42 : 56 )

### 01-7 FORCE MODULE
- FORCE 모듈들의 역할은? 각종 힘을 활용해 파티클이 다양하게 운동하게 만들어준다 ( 47 : 34 )
- 값을 그래프로 컨트롤 하려면? Color from Curve 기능 활용 ( 49 : 05 )

### 01-8 USING GPU FOR SIMULATION
- 가장 최적화된 모드는? GPUCompute Sim, Fixed ( 52 : 37 )

### 01-3 참고한 사이트
- [[참고](https://www.youtube.com/watch?v=SAAZRWBry_I&t=1156s)]

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
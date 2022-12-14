---
title:  "Unreal Engine C++ The Ultimate Shooter Course"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2022-12-13
---

## Chapter 1 Introduction

### 01-1 Introduction
- C++ tutorial series on the Internet, and no other course will you cover such a wide variety of unreal

### 01-2 Install an IDE
- Visual studio installer에서 C++을 사용한 게임 개발 체크하자

![config](https://user-images.githubusercontent.com/80055816/207264533-ed045cee-689f-47dd-acf3-04d52c598713.PNG){: width="100%" height="100%"}{: .align-center}

![config2](https://user-images.githubusercontent.com/80055816/207264842-9f56de74-8a18-42fa-a1e7-17db41b57371.PNG){: width="100%" height="100%"}{: .align-center}

![config3](https://user-images.githubusercontent.com/80055816/207264883-e4205773-3ba8-47bb-8fca-4dcba6975ce0.PNG){: width="100%" height="100%"}{: .align-center}

![config4](https://user-images.githubusercontent.com/80055816/207264946-8aeeaabc-5057-4610-858b-5a53e23563bc.PNG){: width="100%" height="100%"}{: .align-center}

### 01-3 Install Unreal Engine

![showcase](https://user-images.githubusercontent.com/80055816/207268243-b690d3cb-62a1-430c-94ba-d416bf0c5cfd.PNG){: width="100%" height="100%"}{: .align-center}

- 맨 위에서 부터 내 언리얼 엔진, 내 프로젝트, 내 에셋 순으로 나열되어 있다

### 01-4 C++ Refresher and UE4 Hierarchy
- 언리얼 계층 구조에 대해 나열하면? Object - Actor - Pawn - Character
- Object에 대해 설명하면? An object is a very basic type of class, it can store data, but it does not have the capability of being placed in a level
- Actor에 대해 설명하면? An actor can have a visual representation
- Pawn에 대해 설명하면? An pawn can be possessed by a controller, a controller is a special type of actor class that allows you to take user input, such as a keyboard
- Character에 대해 설명하면? A character has its own character, movement component, and this character movement component has functionality
- 언리얼 HAS A 관계에 대해 나열하면? Package - World - Level - Actor - Actor Component

### 01-5 Reflection and Garbage Collection
- Reflection에 대해 설명하면? 리플렉션(Reflection)은 프로그램이 실행시간에 자기 자신을 조사하는 기능입니다 가비지 콜렉션, 네트워크 리플리케이션, 블루프린트/C++ 커뮤니케이션, 에디터의 디테일 패널, 시리얼라이제이션 등 다수의 시스템에 탑재되어 있다
- Now, as soon as that pointer variable goes out of scope, the pointer gets deleted
- Unreal Engines Garbage collection system keeps track of how many variables reference any given object
- For a class to participate in Unreal Engines garbage collection system, it must make use of special macros that allow the class to be recognized ([**참고**](https://www.unrealengine.com/ko/blog/unreal-property-system-reflection))

### 01-6 How to Get Help
- ([**참고**](https://forums.unrealengine.com/categories?tag=unreal-engine)) 여기서 언리얼에 대한 질문을 할 수 있다
- 강의 영상중 특정 부분이 컴파일이 안되거나 하는 문제가 생기면 해당 영상의 질의응답 탭을 확인하자, 이 탭에서 내 질문에 대한 답변을 찾을수 없다면 직접 질문을 올릴수도 있다
- 질문 올릴때 유의점, Tag this video when it posts your questions so I'll know which video you're talking about
- 질문 올릴때 유의점, Mentioned that point in time in the video, in your question
- 질문 올릴때 유의점, If your question involves code, it's really helpful if you show me your code
- 질문 올릴때 유의점, If you have a compiler error, take a screenshot of the compiler error and include that in your question
- 질문 올릴때 유의점, If you have a piece of code that you don't understand, highlight that particular piece of code
- 강사님이 운영하는 디스코드 방에서 질문이나 의견등을 자유롭게 공유할 수 있으며, 협업할 멤버도 구할 수 있다
- 이 단원 자료에 이 강좌에 필요한 리소스, 디스코드 주소 등이 기재되어 있다

## Chapter 2 Project Setup

### 02-7 Project Setup

![set](https://user-images.githubusercontent.com/80055816/207659317-3c933a2d-adbd-4ee1-919d-796bbfde7b15.PNG){: width="100%" height="100%"}{: .align-center}

- 기본 프로젝트 세팅은 위와 같이 하면 된다

![projectmap](https://user-images.githubusercontent.com/80055816/207659560-2bb05525-7699-4982-8840-8594edeedaac.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 화면에 대한 설명은 ([**참고**](https://lifeisforu.tistory.com/326)) 여기에서 확인하면 된다
- It's called shooter game mode base, the class name is called a shooter game mode base because this game mode inherits ultimately from actor in the inheritance hierarchy
- So we have this shooter game mode based class And since we have access to it in C++, we can create variables and functions and things here if we want to, in the Unreal Engine, Ed, we can create a blueprint based on this class and we're going to stick it in the game mode folder
- 월드 세팅에 대한 설명은 ([**참고**](https://velog.io/@jijang/%EC%9B%94%EB%93%9C-%EC%84%B8%ED%8C%85)) 여기에서 확인하면 된다

![projectmode](https://user-images.githubusercontent.com/80055816/207664691-17660953-3654-4c54-815d-dbebb4e7a575.PNG){: width="100%" height="100%"}{: .align-center}

- Right here for the default game mode, we can set this to shooter game mode based BP and that will ensure that our shooter game mode based BP will be used for our entire project

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
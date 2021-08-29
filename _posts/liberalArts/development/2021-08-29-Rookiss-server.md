---
title:  "Rookiss님 언리얼 강의 Part4: 게임 서버"

categories:
  - Development
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2021-08-29
last_modified_at: 2021-08-29
---

## Chapter 00 오티

### 00-1 오티
- 게임서버는 온라인 상에서 여러 플레이어가 같이 플레이 할 수 있게 중개해주는 역할을 한다
- 전투, 아이템, 퀘스트, 업적, 인공지능 등의 기능을 모두 서버에서 제공한다
- 게임회사에서 일하다 보면 언젠가는 바닥부터 만드는 날이 오는데 그날을 대비하자

### 00-2 서버 오티
- 서버란 다른 컴퓨터에서 연결이 가능하도록 대기 상태로 상시 실행중인 프로그램
- 웹서버는 단순히 게임에 국한되지 않고, 웹 서비스를 만드는데 사용
- 게임서버는 완벽한 하나의 프레임워크가 존재하기 어렵다

### 00-3 환경 설정
- 정적 라이브러리의 장점은 빌드를할때 바이너리에 해당 라이브러리 내용이 포함이 된다

![config1](https://user-images.githubusercontent.com/80055816/131241580-5621071a-7d87-43c9-a4b8-c07a22c7a8fe.PNG){: width="70%" height="70%"}{: .align-center}

![config2](https://user-images.githubusercontent.com/80055816/131241599-0c75873b-7648-47fe-982e-b53b7ba8303a.PNG){: width="70%" height="70%"}{: .align-center}

![config3](https://user-images.githubusercontent.com/80055816/131241606-cb712729-b183-4305-b469-8419dcac2cc7.PNG){: width="70%" height="70%"}{: .align-center}

![config4](https://user-images.githubusercontent.com/80055816/131241615-aa4445f9-e182-4ea1-b948-963aeeeb44af.PNG){: width="70%" height="70%"}{: .align-center}

![config5](https://user-images.githubusercontent.com/80055816/131241621-9e48d899-6a59-4a12-a367-0fd08885545f.PNG){: width="70%" height="70%"}{: .align-center}

![config6](https://user-images.githubusercontent.com/80055816/131241626-d2693d18-2aad-48eb-81e2-ea9daa2347a2.PNG){: width="70%" height="70%"}{: .align-center}

![config7](https://user-images.githubusercontent.com/80055816/131241632-03afa367-1cdb-4bc5-a096-f8ccb52fad31.PNG){: width="70%" height="70%"}{: .align-center}

<br>

## Chapter 01 멀티쓰레드 프로그래밍
### 01-1 멀티쓰레드 개론
- 쓰레드는 영혼에 비유된다

![env](https://user-images.githubusercontent.com/80055816/131258170-860bcf40-7ac4-4f9e-905b-7a66891c15ed.png){: width="70%" height="70%"}{: .align-center}

- 멀티쓰레드 환경에서는 위의 이미지와 같이 Heap 영역과 데이터 영역을 모든 쓰레드들이 공유할 수 있다

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

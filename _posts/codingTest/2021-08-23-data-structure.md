---
title:  "Rookiss님 언리얼 강의 Part3: 자료구조와 알고리즘"

categories:
  - Data Structure
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2021-08-23
last_modified_at: 2021-08-23
---

## Chapter 00 오티

### 00-1 오티 
- 정렬, 레드 블랙 트리, 해쉬 테이블이 면접에서 자주 나온다

### 00-2 Big-O 표기법

![BIG-O](https://user-images.githubusercontent.com/80055816/130400676-3e92dcd2-18c8-496b-af3c-54d85ebc8a3d.png){: width="70%" height="70%"}{: .align-center}

- 입력 n의 크기에 따라 성능이 영향을 받는 정도는 위와 같다

<br>

## Chapter 01 미로 준비

### 01-1 환경 설정

![project](https://user-images.githubusercontent.com/80055816/130419000-f17a91a5-3452-432b-963d-ebe2f18c2b9c.PNG){: width="70%" height="70%"}{: .align-center}

- [프로젝트 속성] - [미리 컴파일된 헤더 사용] 해주자

![cpp](https://user-images.githubusercontent.com/80055816/130419187-bd733669-9bd9-4a3d-828b-df84d5963f06.PNG){: width="70%" height="70%"}{: .align-center}

- [Cpp파일 우클릭] - [미리 컴파일된 헤더 만들기] 해주자

### 01-2 맵 만들기

```cpp
for (int32 y = 0; y < _size; y++)
{
	for (int32 x = 0; x < _size; x++)
	{
		// 외곽만 막아주는 코드
		if ( x==0 || x==_size -1 || y==0 || y=_size -1 )
			_tile[y][x] = TileType::WALL;
		else
			_tile[y][x] = TileType::EMPTY;
	}
}
for (int32 y = 0; y < _size; y++)
{
	for (int32 x = 0; x < _size; x++)
	{
		// 외곽 및 길과 길사이를 막아주는 코드
		if (x % 2 == 0 || y % 2 == 0)
			_tile[y][x] = TileType::WALL;
		else
			_tile[y][x] = TileType::EMPTY;
	}
}
// 외곽과 원래 뚫려있는 길은 다시 뚫지 않는다
if (x % 2 == 0 || y % 2 == 0)
	continue;
```

- Mazes For Programmers 책은 미로를 만드는 다양한 알고리즘을 다루는 책이다

### 01-3 오른손 법칙

```cpp
// 현재 바로보는 방향을 기준으로 오른쪽으로 갈 수 있는지 확인
int 32 newDir = (_dir -1 + DIR_COUNT) % DIR_COUNT;
```

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

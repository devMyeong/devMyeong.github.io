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

## Chapter 02 선형 자료 기초

### 02-1 배열, 동적 배열, 연결 리스트
- 선형 구조의 대표적인 예는 배열, 연결 리스트, 스택, 큐
- 비선형 구조의 대표적인 예는 트리, 그래프
- 면접에서 배열, 동적 배열, 연결 리스트의 차이를 설명할 수 있어야 한다
- 동적 배열 할당 정책은 실제 사용할 공간보다 1.5 ~ 2배 정도 더 많이 공간을 할당한다

### 02-2 동적 배열 구현 연습

```cpp
// 증설 작업
int newCapacity = static_cast<int>(_capacity * 1.5);
// newCapacity가 0또는 1이면 안되기 때문에 처리해주는 조건문
if (newCapacity == _capacity)
	newCapacity++;
```

- vector의 reserve()와 resize()의 차이에 대해서 알아야 한다
- vector의 capacity와 size의 차이에 대해서 알아야 한다
- 코딩 테스트에서 vector 구현은 은근히 자주 나온다

### 02-3 연결 리스트 구현 연습

```cpp
// [head] <-> [1] <-> [prevNode] <-> [before] <-> [tail]
// [head] <-> [1] <-> [prevNode] <-> [newNode] <-> [before] <-> [tail]
Node<T>* AddNode(Node<T>* before, const T& value)
{
	Node<T>* newNode = new Node<T>(value);
	Node<T>* prevNode = before->_prev;

	// newNode의 왼쪽 노드와 연결
	prevNode->_next = newNode;
	newNode->_prev = prevNode;

	// newNode의 오른쪽 노드와 연결
	newNode->_next = before;
	before->_prev = newNode;

	_size++;

	return newNode;
}
~List()
{
	// 물고 있는 데이터들을 한번에 손쉽게 해제한다
	while (_size > 0)
		pop_back();

	delete _head;
	delete _tail;
}
// iterator는 Iterator<T>를 의미하게 정의한다
using iterator = Iterator<T>;
```

- 리스트는 어떠한 데이터를 탐색하는 시간은 느린데 해당 데이터를 삭제하는 것은 빠르다는 모순이 있다
- 리스트는 삭제 또는 추가할 위치를 저장하고 있을 때 그때 에서만 빠르다

### 02-4 스택
- Iterator 활용하면 어떤 컨테이너 인지 상관없이 코드를 하나만 만들어주면 되어서 인터페이스 통일이라는 큰 장점이 생긴다

```cpp
// stack의 내부의 데이터를 순차적으로 출력
while (s.empty() == false)
{
	// 최상위 원소
	int data = s.top();
	// 최상위 원소 삭제
	s.pop();

	cout << data << endl;
}
```

- stack 구현 문제도 코딩테스트로 나올수 있다
- stack은 배열형태로 구현해도 되고 연결리스트로 형태로 구현해도 된다

```cpp
// 컨테이너를 선택할 수 있게 만든 Stack 클래스
template<typename T, typename Container = vector<T>>
class Stack
{
public:
	void push(const T& value)
	{
		_container.push_back(value);
	}

	void pop()
	{
		_container.pop_back();
	}

	T& top()
	{
		return _container.back();
	}

	bool empty() { return _container.empty(); }
	int size() { return _container.size(); }

private:
	//vector<T> _container;
	//list<T> _container;
	Container _container;
};
```


<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

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
last_modified_at: 2021-08-27
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

// 요청한 할당량 보다 기존의 할당량이 더 많다면 return
if (_capacity >= capacity)
	return;
}
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
// iterator가 Iterator<T>를 의미하게 정의한다 ( alias declaration )
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

### 02-5 큐

```cpp
// Queue에서 데이터를 꺼내서 출력
while (q.empty() == false)
{
	int value = q.front();
	q.pop();
	cout << value << endl;
}

void push(const T& value)
{
	// TODO : 다 찼는지 체크
	if (_size == _container.size())
	{
		// ...
		// 새 공간으로 데이터 복사( front 부터 back 전까지만 )
		// [][][front][][][][][][][][][back][][][][]
		// [front][][][][][][][][][back][][][][][][][][][][][][][][][][][][][][]
		for (int i = 0; i < _size; i++)
		{
			int index = (_front + i) % _container.size();
			newData[i] = _container[index];
		}
	}

	...

	// % _container.size()를 해주는 이유는 큐에 데이터가 다찼으면
	// 큐 구조 특성상 인덱스가 다시 0으로 회귀 해야하기 때문이다
	// 참고로 back은 데이터가 push될 위치
	_container[_back] = value;
	// _container.size()는 버퍼의 크기를 의미한다
	_back = (_back + 1) % _container.size();
	// _size는 데이터 개수이다
	_size++;

}

void pop()
{
	// front는 데이터가 pop될 위치
	_front = (_front + 1) % _container.size();
	_size--;
}
```

### 02-6 오른손 법칙 개선
- 동적 배열, 연결 리스트는 면접에서 자주 등장하는 주제이다
- 동적 배열은 push로 데이터 삽입시 O(1)의 시간복잡도를 가진다, 중간 삽입 및 삭제는 O(N) 시간복잡도를 가진다, 임의 접근은 O(1) 시간복잡도를 가진다
- 연결 리스트는 데이터 삽입시 O(1)의 시간복잡도를 가진다, 임의 접근은 O(N)의 시간복잡도를 가진다

```cpp
for (int i = 0; i < _path.size() - 1; i++)
{
	// 스택 최상단의 내용과 내가 가려는 길이 일치한다면 되돌아 가고
	// 있다는 뜻이다 따라서 해당 루트를 제거해야 한다
	if (s.empty() == false && s.top() == _path[i + 1])
		s.pop();
	else
		s.push(_path[i]);
}

vector<Pos> path;
while (s.empty() == false)
{
	path.push_back(s.top());
	s.pop();
}
// 스택 특성상 reverse를 해줘야 한다 그렇지
// 않으면 목적지에서 출발지로 가게된다
std::reverse(path.begin(), path.end());
```

<br>

## Chapter 03 그래프 기초

### 03-1 그래프 기초
- 정점(Vertex) : 데이터를 표현(사물, 개념 등)
- 간선(Edge) : 정점들을 연결하는데 사용

![DIR](https://user-images.githubusercontent.com/80055816/131204304-b604713a-b77e-4da6-9192-1bfa080358a3.png){: width="70%" height="70%"}{: .align-center}

![WEIGHTED](https://user-images.githubusercontent.com/80055816/131204315-57bcb78b-249b-43f9-917c-682ee6e27eec.png){: width="70%" height="70%"}{: .align-center}

- 우리가 구현할 그래프는 위와 같다

```cpp
// STL
vector<int>& adj = adjacent[0];
// 3 이라는 정점을 찾았는지 확인하는 코드
bool connected2 = (std::find(adj.begin(), adj.end(), 3) != adj.end());

// 연결된 목록을 따로 관리
// [X][O][X][O][X][X] // 0 번째 정점에 1번 3번 정점이 연결 되어 있다
// [O][X][O][O][X][X] // 1 번째 정점에 0번 2번 3번 정점이 연결 되어 있다
// [X][X][X][X][X][X] // ...
// [X][X][X][X][O][X]
// [X][X][X][X][X][X]
// [X][X][X][X][O][X]

// 읽는 방법 : adjacent[from][to]
// 행렬을 이용한 그래프 표현 (2차원 배열)
// 메모리 소모가 심하지만, 빠른 접근이 가능하다
// (간선이 많은 경우 이점이 있다)
vector<vector<bool>> adjacent(6, vector<bool>(6, false));
adjacent[0][1] = true;
adjacent[0][3] = true;
adjacent[1][0] = true;
adjacent[1][2] = true;
adjacent[1][3] = true;
adjacent[3][4] = true;
adjacent[5][4] = true;
```

- CreateGraph_3() 방법은 메모리를 많이 소모한다는 단점이 존재하지만 굉장히 빠르게 접근할 수 있다는 장점이 생긴다

```cpp
vector<vector<int>> adjacent2 =
{
	// 아래는 가중치 그래프 인데 -1은 비어 있는 것을 의미한다
	vector<int> { -1, 15, -1, 35, -1, -1 }, // 0 번째 정점에 1번 3번 정점이 연결 되어 있다
	vector<int> { 15, -1, +5, 10, -1, -1 }, // 1 번째 정점에 0번 2번 3번 정점이 연결 되어 있다
	vector<int> { -1, -1, -1, -1, -1, -1 }, // ...
	vector<int> { -1, -1, -1, -1, +5, -1 },
	vector<int> { -1, -1, -1, -1, -1, -1 },
	vector<int> { -1, -1, -1, -1, +5, -1 },
};
```

### 03-2 DFS (깊이 우선 탐색)
- DFS (Depth First Search) 깊이 우선 탐색
- BFS (Breadth First Search) 너비 우선 탐색
- F10과 F11을 이용해서 디버깅을 하며 코드를 확인해보자

```cpp
// 인접 리스트 version
// 모든 인접 정점을 순회한다
for (int i = 0; i < adjacent[here].size(); i++)
{
	int there = adjacent[here][i];
	if (visited[there] == false)
		Dfs(there);
}

// 인접 행렬 version
// 모든 인접 정점을 순회한다
for (int there = 0; there < 6; there++)
{
	if (adjacent[here][there] == 0)
		continue;

	// 아직 방문하지 않은 곳이 있으면 방문한다
	if (visited[there] == false)
		Dfs(there);
}
```

### 03-3 BFS (너비 우선 탐색)

![bfs](https://user-images.githubusercontent.com/80055816/131243395-276f0cb5-118a-4c7c-b3b6-26f1ed0b6dfd.png){: width="70%" height="70%"}{: .align-center}

- BFS는 큐로 예약을 한다
- BFS는 코딩 면접에서 단골로 등장하기 때문에 안보고 구현해보는 연습을 하자

### 03-4 BFS를 이용한 길찾기 구현

```cpp
_path.clear();
// 거꾸로 거슬러 올라간다
pos = dest;

while (true)
{
	_path.push_back(pos);

	// 시작점은 자신이 곧 부모이다
	if (pos == parent[pos])
		break;

	pos = parent[pos];
}
```

- 코드를 외우려 하지말고 공책에 알고리즘을 그려보자 즉 흐름을 그려보자

### 03-5 다익스트라 알고리즘
- 가중치를 확인해 효율적인 간선만 채택한다

```cpp
// 발견목록을 queue가 아닌 list로 관리하는 이유는
// 먼저 발견한 애가 먼저 방문이 된다는 보장이 없기 때문
list<VertexCost> discovered; // 발견 목록

// 우월한 경로를 계속 갱신해줘야 한다
vector<int> best(6, INT32_MAX); // 각 정점별로 지금까지 발견한 최소 거리 이며 INT32_MAX는 단순히 엄청 큰값
```

<br>

## Chapter 04 힙과 우선순위 큐

### 04-1 트리 기초
- 트리는 계층적 구조를 갖는 데이터를 표현하기 위한 자료구조 이다

![treebase](https://user-images.githubusercontent.com/80055816/131501899-1f09ba6d-506f-4f74-a060-a8bc5858bc28.png){: width="70%" height="70%"}{: .align-center}

- 트리와 관련된 용어는 부모, 자식, 형제, 선조, 자손, 루트, 잎, 깊이, 높이, 트리의 재귀적 속성 및 서브트리가 있다

![sharedptr](https://user-images.githubusercontent.com/80055816/131501980-e4b34580-dca4-4d38-b6cf-03bc2a1cd373.PNG){: width="70%" height="70%"}{: .align-center}

![ourtree](https://user-images.githubusercontent.com/80055816/131502028-2b689c9e-7603-4eb9-ac86-5f15e12d4b30.png){: width="70%" height="70%"}{: .align-center}

- 깊이(depth)란 루트에서 어떤 노드에 노달하기 위해 거쳐야 하는 간선의 수 (aka. 몇 층?)
- 높이(height)란 가장 깊숙히 있는 노드의 깊이 (max(depth))

```cpp
// 아래 부분은 N사의 손코딩 테스트에 나온 문제( Height 구하기 )
int GetHeight(NodeRef root)
{
	int height = 1;

	for (NodeRef& child : root->children)
		// max 함수는 둘중 더 큰애를 골라준다
		height = max(height, GetHeight(child) + 1);

	return height;
}
```

### 04-2 힙 트리 이론
- 이진 트리란 각 노드가 <u>최대</u> 두 개의 자식 노드를 가지는 트리
- 이진 트리의 특징은 왼쪽을 타고 가면 현재 값보다 작으며 오른쪽을 타고 가면 현재 값보다 크다

![treeproblem](https://user-images.githubusercontent.com/80055816/131513101-e9173606-8962-4543-82fd-038f6f6d350f.png){: width="70%" height="70%"}{: .align-center}

- 위의 이진 검색트리 처럼 그냥 무식하게 추가하면, 한쪽으로 기울어져서 균형이 깨진다(연결 리스트랑 다를게 없다) 즉 트리 재배치를 통해 균형을 유지하는 것이 과제이다(AVL, Red-Black)

![heaptree](https://user-images.githubusercontent.com/80055816/131513186-66d29e6d-4ead-4d45-b879-17e9ec87ffee.png){: width="70%" height="70%"}{: .align-center}

- 힙트리는 부모 노드가 가진 값이 항상 자식 노드가 가진 값보다 크기만 하면 된다
- 힙트리는 마지막 레벨을 제외한 모든 레벨에 노드가 꽉 차 있다(완전 이진 트리)

![heaptree2](https://user-images.githubusercontent.com/80055816/131513254-c0d3cf82-b012-4ac6-8460-682e91f19562.png){: width="70%" height="70%"}{: .align-center}

- 힙트리는 마지막 레벨에 노드가 있을 때는 항상 왼쪽부터 순서대로 채워야 한다
- 힙트리는 노드 개수를 알면, 트리 구조는 무조건 확정할 수 있다
- 힙트리는 배열을 이용해서 힙 구조를 바로 표현할 수 있다( 09:43 참조 )
- 힙 트리 특성상 최대값은 무조건 루트 노드에 있는 값이다
- 공책과 팬을 이용해서 트리를 그려보고 데이터를 제거하는 실습을 해보자

### 04-3 우선순위 큐 구현 연습
- 가장 작은값 혹은 가장 큰값 둘중 하나를 데이터 중에서 계속 추출해야되는 필요성이 있을때 우선순위 큐가 활용된다

### 04-4 A* 길찾기 알고리즘
- `A*와 다익스트라 알고리즘` 의 가장 큰 차이점은 A*는 시작점 뿐만 아니라 목적지라는 명확한 개념도 생기게 된다

<br>

## Chapter 05 탐색 트리

### 05-1 이진 탐색
- 이진 탐색은 면접에서 자주 나온는 주제이다
- 이진 탐색은 배열에 데이터가 정렬되어 있다고 가정한다
- 데이터가 한번만 만들어지고 평생 바뀔일이 없다고 가정하면 배열로 관리해서 이진 탐색을 사용하는것 또한 우월한 방법이 될 수 있다

### 05-2 이진 탐색 트리
- 전위 순회 (preorder traverse), 중위 순회 (inorder), 후위 순회 (postorder)

### 05-3 레드 블랙 트리 #1
- 트리의 균형을 맞추는 대표적인 예시

![rb](https://user-images.githubusercontent.com/80055816/131720218-6db833e1-0d28-47d4-90d0-e8312e724271.PNG){: width="100%" height="100%"}{: .align-center}

### 05-4 레드 블랙 트리 #2
- 오늘 주제는 일평생 한번 만들까 말까한 내용이다
- 레드 블랙트리는 어떤 노드로부터 그에 속한 하위 리프 노드에 도달하는 모든 경로에는 같은 개수의 블랙 노드를 만난다 라는 특징이 있다는 것을 명심하자

### 05-5 레드 블랙 트리 #3

```cpp
// 먼저 BST 삭제 실행... ( 참고로 이 주제가 코딩 테스트에 나올일은 없음 )
// - Case1) 삭제할 노드가 Red -> 그냥 삭제! 끝!
// - Case2) root가 DB -> 그냥 추가 Black 삭제! 끝!
// - Case3) DB의 sibling 노드가 Red
// -- s = black, p = red (s <-> p 색상 교환)
// -- DB 방향으로 rotate(p) 
// -- goto other case
// - Case4) DB의 sibling 노드가 Black && sibling의 양쪽 자식도 Black
// -- 추가 Black을 parent에게 이전
// --- p가 Red이면 Black 됨.
// --- p가 Black이면 DB 됨.
// -- s = red
// -- p를 대상으로 알고리즘 이어서 실행 (DB가 여전히 존재하면)
// - Case5) DB의 sibling 노드가 Black && sibling의 near child = red, far child = black
// -- s <-> near 색상 교환
// -- far 방향으로 rotate(s)
// -- goto case 6
// - Case6) DB의 sibling 노드가 Black && sibling의 far child = red
// - p <-> s 색상 교환
// - far = black
// - rotate(p) (DB 방향으로)
// - 추가 Black 제거
```

<br>

## Chapter 06 정렬

### 06-1 기본 정렬(버블, 선택, 삽입)
- 코딩 면접에서 정렬은 단골 주제이다

```cpp
// 1) 버블 정렬 (Bubble Sort)
void BubbleSort(vector<int>& v)
{
	const int n = (int)v.size();

	// (N-1) + (N-2) + ... + 2 + 1
	// 등차수열의 합 = N * (N-1) / 2
	// O(N^2)
	for (int i = 0; i < n - 1; i++)
	{
		// n - 1 - i 에서 -i를 해주는 이유는 확보된
		// 자리는 스위칭이 일어나지 않게 하기 위함
		for (int j = 0; j < (n - 1 - i); j++)
		{
			if (v[j] > v[j + 1])
			{
				int temp = v[j];
				v[j] = v[j + 1];
				v[j + 1] = temp;
			}
		}
	}
}
```

- 삽입정렬은 문자열과 관련된 코딩 문제에서 자주 나온다

### 06-2 힙 정렬과 병합 정렬

```cpp
// 힙 정렬
void HeapSort(vector<int>& v)
{
	priority_queue<int, vector<int>, greater<int>> pq;
	
	// 데이터를 밀어넣는데 걸리는 시간은 logN
	// 총 데이터는 N개 따라서 시간 복잡도는 O(NlogN)
	for (int num : v)
		pq.push(num);

	v.clear();

	// O(NlogN)
	while (pq.empty() == false)
	{
		v.push_back(pq.top());
		pq.pop();
	}
}

// 병합 정렬
// 분할 정복 (Divide and Conquer)
// - 분할 (Divide)		문제를 더 단순하게 분할한다
// - 정복 (Conquer)		분할된 문제를 해결
// - 결합 (Combine)		결과를 취합하여 마무리

// 퀴즈 - a와 b가 정렬되어 있다고 가정하자 a와 b를
// 정렬된 상태로 합쳐서 리턴하는 함수를 구현하라
vector<int> Merge(vector<int> a, vector<int> b)
{
	vector<int> temp;

	return temp;
}
```

- 디버깅 해가면서 공부하자, 25:46 처럼 그림을 그려가면 공부하자

### 06-3 퀵 정렬

```cpp
// [5][1][3][7][9][2][4][6][8]
// p
//           high low	
```

<br>

## Chapter 07 해시 테이블

### 07-1 해시 테이블
- hash_map을 구현할 일은 거의 없다
- map vs hash_map(C++11 표준 unordered_map)의 차이를 설명할 수 있어야 한다
- map은 레드블랙트리 소위 균형이진 트리 구조이기 때문에 트리 구조로 관리된다
- hash_map은 메모리 손해는 있으나 속도 측면에서는 map보다 빠르다

<br>

## Chapter 08 최소 스패닝 트리

### 08-1 Disjoint Set(상호 배타적 집합)
- 이 주제 이후부터는 필수적이지는 않지만 배워두면 좋은 기법들을 설명한다
- 서로 다른팀인 상태에서 팀이 합쳐지는 상황이 빈번하게 발생한다고 가정할때 사용할 수 있는 알고리즘

### 08-2 Kruskal 알고리즘

![spa](https://user-images.githubusercontent.com/80055816/131801235-3fde8210-23fe-4d7d-aec4-8ab06f36371c.png){: width="100%" height="100%"}{: .align-center}

- 위의 이미지에서 노란선은 후보 간선
- 스패닝 트리란 간선의 수를 최소화해서, 모든 정점을 연결하는것
- 간선의 수를 최소화 한다는 말은 사이클이 생기지 않게 해야 한다는 뜻
- 간선을 연결해보면 필연적으로 간선의 개수가 N-1개가 된다

![mst](https://user-images.githubusercontent.com/80055816/131801313-7cb04968-dab1-4377-8352-8838b87cff14.png){: width="100%" height="100%"}{: .align-center}

- 최소 스패닝 트리 기반으로 통신 네트워크 구축시 구축 비용, 전송 시간 등을 최소로 해야 한다
- 크루스칼(KRUSKAL) MST 알고리즘의 특징은 탐욕적인(greedy) 방법을 사용

### 08-3 Kruskal 알고리즘을 이용한 맵 생성
- 오른쪽 벽과 아래쪽 벽이 일렬로 생기는 것을 방지할 수 있다

### 08-4 Prim 알고리즘을 이용한 맵 생성
- 특징은 하나의 시작점으로 구성된 트리에 간선을 하나씩 수집하며 진행

<br>

## Chapter 09 동적 계획법

### 09-1 동적 계획법 입문
- 이 주제는 현업에서는 딱히 쓸일이 없다
- 인공지능쪽 연구를 한다고 하면 중요하게 활용이 된다
- 코딩 면접에서는 종종 나오는 주제이다 ( N사 )

### 09-2 샘플 문제 : LIS
- 이 주제는 동적 계획법(DP)의 대표적인 예시
- 굉장히 많은 실습이 필요하다

### 09-3 샘플 문제 : TRIANGLE_PATH

```cpp
// 오늘의 주제 : 동적 계획법 (DP)
// TRIANGLE_PATH
// - (0,0)부터 시작해서 아래 or 아래우측으로 이동 가능
// - 만나는 숫자를 모두 더함
// - 더한 숫자가 최대가 되는 경로? 합?

// 6
// 1 2
// 3 7 4
// 9 4 1 7
// 2 7 5 9 4
```

### 09-4 샘플 문제 : TIC-TAE-TOE
- 공부중

### 09-5 실전 문제 : ENCHANT (K모 회사)
```cpp
// +1 +2 +3 +4
// +1 +2 +4
// +1 +3 +4
// +1 +4
// +2 +3 +4
// +2 +4
// +3 +4
```

### 09-6 실전 문제 : KART-RIDER (N모 회사)
- 공부중

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

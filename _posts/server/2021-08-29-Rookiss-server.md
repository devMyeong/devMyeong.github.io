---
title:  "Rookiss님 언리얼 강의 Part4: 게임 서버"

categories:
  - Server
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2021-08-29
last_modified_at: 2021-10-16
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

### 01-2 쓰레드 생성
- 국내는 윈도우서버를 많이 사용하나 해외는 리눅스서버를 선호한다

```cpp
#include <thread>

void HelloThread()
{
	cout << "Hello Thread!" << endl;
}

int main()
{
	// 메인쓰레드가 실행됨과 동시에 아래 코드에 해당하는
	// 쓰레드가 생성되어 병렬로 처리가 된다
	std::thread t(HelloThread);

	// CPU 코어 개수
	int32 count = t.hardware_concurrency();

	// 쓰레드마다 id
	auto id = t.get_id();

	// std:thread 객체에서 실제 쓰레드를 분리
	t.detach();
	
	// t 객체가 관리하고 있는 thread가 살아있는지 확인
	if (t.joinable())
	{
		// t 쓰레드 보다 메인 쓰레드가 먼저 종료 되는것을 방지
		t.join();
	}

	cout << "Hello Main" << endl;
}
```

![thread](https://user-images.githubusercontent.com/80055816/131530998-76f522a6-8f26-4ce2-8de3-66fe89ad47d5.PNG){: width="70%" height="70%"}{: .align-center}

- thread 탭을 이용해 각 thread의 현재 진행 상황을 볼 수 있다

```cpp
#include <thread>

void HelloThread_2(int32 num)
{
	cout << num << endl;
}

int main()
{
	vector<std::thread> v;

	for (int32 i = 0; i < 10; i++)
	{
		v.push_back(std::thread(HelloThread_2, i));
	}

	for (int32 i = 0; i < 10; i++)
	{
		if (v[i].joinable())
			v[i].join();
	}
}
```

### 01-3 Atomic
- [디버그] - [디스어셈블리] 사용법은 05:05 참조
- atomic : All-Or-Nothing
- `atomic<int32> sum = 0;`

### 01-4 Lock 기초

```cpp
#include "pch.h"
#include <iostream>
#include "CorePch.h"
#include <thread>
#include <atomic>
#include <mutex>
#include <vector>

using namespace std;

// stl 자료구조를 멀티쓰레드 환경에서 정상 동작 하게 하기위해 mutex를 활용하자
vector<int32> v;

// mutex의 특징은 Mutual Exclusive ( 상호배타적 )
mutex m;

// RAII ( Resource Acquisition Is Initialization )
template<typename T>
class LockGuard
{
public:
	LockGuard(T& m)
	{
		_mutex = &m;
		_mutex->lock();
	}

	~LockGuard()
	{
		_mutex->unlock();
	}

private:
	T* _mutex;
};

void Push()
{
	for (int32 i = 0; i < 10000; i++)
	{
		// 자동으로 mutex 걸기 및 해제 담당
		LockGuard<std::mutex> lockGuard(m);
		// 위의 기능은 std에도 정의되어 있다
		// std::lock_guard<std::mutex> lockGuard(m);
		// 위의 기능을 유도리 있게 만든 버전의 클래스
		// std::unique_lock<std::mutex> uniqueLock(m, std::defer_lock);
		// 위의 uniqueLock 객체는 아래의 코드 이후부터 동작한다
		// uniqueLock.lock();

		// 수동으로 mutex 걸기
		// m.lock();

		v.push_back(i);

		if (i == 5000)
		{
			// mutex 해제
			// 수동으로 mutex 제어시 break를 통해 나가기 전에 반드시 unlock 해줘야 한다
			// 이러한 실수를 방지하기 위한 클래스가 LockGuard
			//m.unlock();
			break;
		}

		// 수동으로 mutex 해제
		// m.unlock();
	}
}
```

### 01-5 DeadLock
- Lock을 하나만 활용하는게 아니라 매니저 별로 하나씩 있으면 경우에 따라 두개의 lock을 잡아야 하는 경우가 종종 있다 ( 07 : 45 )
- 데드락이 발생하는 이유는 lock에 접근하는 순서가 달라서다 ( 15 : 40 )
- Lock에 접근하는 순서를 정하면 데드락 문제를 해결할 수 있다 ( 16 : 10 )
- 결론은 데드락은 미리 예방할 수 없고 조심하면서 사용해야 한다
- 데드락은 그래프로 치면 사이클이 일어난 상황이다 이 특징을 이용해 데드락을 컨트롤 할 수 있다 ( 22 : 30 )

### 01-6 Lock 구현 이론
- 스핀락은 무작정 기다리는 정책
- 유저레벨에서 커널레벨로 바뀌는 것이 컨텍스트 스위칭 이다

### 01-7 SpinLock
- 스핀락은 면접에서 자주나오는 단골 주제이다
- C++ 에서 volatile는 컴파일러 에게 최적화를 하지말아 달라는 키워드 ( 06 : 03 )

```cpp
class SpinLock
{
public:
	void lock()
	{
		// CAS (Compare-And-Swap)
		bool expected = false;
		bool desired = true;

		// CAS 의사 코드
		/*
		if (_locked == expected)
		{
			expected = _locked;
			_locked = desired;
			return true;
		}
		else
		{
			expected = _locked;
			return false;
		}
		*/

		while (_locked.compare_exchange_strong(expected, desired) == false)
		{
			// 이 코드가 왜 필요한지는 영상 참조 ( 20 : 02 )
			expected = false;
		}
		// 이 코드가 한번에 묶인게 위의 코드이다 ( 원자적으로 실행되기 위해 )
		/*
		while(_locked)
		{

		}

		_locked = true;
		*/
	}

	void unlock()
	{
		_locked.store(false);
	}

private:
	atomic<bool> _locked = false;
};
``` 

### 01-8 Sleep

```cpp
class SpinLock
{
public:
	void lock()
	{
		// CAS (Compare-And-Swap)
		bool expected = false;
		bool desired = true;

		while (_locked.compare_exchange_strong(expected, desired) == false)
		{
			expected = false;

			// 100ms 동안 잠잔 다음에 다시 깨어나서 이어서 실행하라 ( 컨텍스트 스위칭 발생 )
			this_thread::sleep_for(std::chrono::milliseconds(100));
			// 위의 코드와 같의 의미이다
			this_thread::sleep_for(100ms);
		}

	}

	void unlock()
	{
		_locked.store(false);
	}

private:
	atomic<bool> _locked = false;
};
```

### 01-9 Event

```cpp
// 커널 오브젝트 - 커널에서 관리하는 오브젝트 ( 가끔 신호를 주고 받는 상황에서 사용하면 좋다 )
handle = ::CreateEvent(NULL /*보안 속성*/, FALSE /*bManualReset*/, FALSE /*bInitialState*/, NULL /*name*/);
// 이벤트 활성화
::SetEvent(handle);
// 커널에게 이벤트가 활성화 되면 깨워달라고 부탁한 상태
::WaitForSingleObject(handle, INFINITE);
// bManualReset이 false이면 수동으로 리셋 해줘야 한다
::ResetEvent(handle);
// 핸들 종료
::CloseHandle(handle);
```

### 01-10 Condition Variable
- Event의 문제점 숙지 필요 ( 00 : 49 )

```cpp
mutex m;
queue<int> q;

// 참고) CV는 User-Level Object ( 커널 오브젝트X )
// 커널 레벨 오브젝트 - 다른 프로그램과 이벤트를 활용해 동기화가 가능하다
// 유저 레벨 오브젝트 - 동일한 프로그램 내부에서만 사용할 수 있다
condition_variable cv;

void Producer()
{
	while (true)
	{
		// 1) Lock을 잡고
		// 2) 공유 변수 값을 수정
		// 3) Lock을 풀고
		// 4) 조건변수 통해 다른 쓰레드에게 통지
		{
			unique_lock<mutex> lock(m);
			q.push(100);
		}
	}

	cv.notify_one(); // wait중인 쓰레드가 있으면 딱 1개를 깨운다
}

void Consumer()
{
	while (true)
	{
		unique_lock<mutex> lock(m);
		// wait의 매개변수가 반드시 unique_lock 이어야
		// 하는 이유는 영상 참조 ( 10 : 26 )
		cv.wait(lock, []() { return q.empty() == false; });
		// 1) Lock을 잡고
		// 2) 조건 확인, 조건을 확인해야 하는 이유는 Spurious Wakeup 때문인데 영상 참조 ( 13: 53 )
		// - 만족O => 빠져 나와서 이어서 코드를 진행
		// - 만족X => Lock을 풀어주고 대기 상태

		// while 문이 필요없는 이유는 영상 참조 ( 12 : 25 )
		//while (q.empty() == false)
		{
			int data = q.front();
			q.pop();
			cout << q.size() << endl;
		}
	}
}
```

### 01-11 Future

```cpp
int main()
{
	// 멀티쓰레드와 비동기적 실행을 헷갈리지 말자 ( 32 : 47 )
	// 아래에서 배울 내용들은 비동기적 실행에 관한 내용이다

	// future를 사용하는 첫번째 방법, std::async
	{
		// 1) deferred -> lazy evaluation 지연해서 실행하세요
		// 2) async -> 별도의 쓰레드를 만들어서 실행하세요
		// 3) deferred | async -> 둘 중 알아서 골라주세요
		// Calculate는 전역함수인데 전역함수 말고 객체에 대해서 future를 사용하고 싶다면 영상 참조 ( 17 : 45 )
		// 언젠가 미래에 결과물을 뱉어줄거야!
		std::future<int64> future = std::async(std::launch::async, Calculate);

		// 처리가 다 되었는지 확인
		std::future_status status = future.wait_for(1ms);
		if (status == future_status::ready)
		{

		}

		// 결과물이 이제서야 필요하다
		int64 sum = future.get();
	}

	// future를 사용하는 두번째 방법, std::promise 
	{
		// 미래(std::future)에 결과물을 반환해줄꺼라 약속(std::promise) 해줘~ (계약서?)
		// PromiseWorker 쓰레드가 처리한다
		std::promise<string> promise;
		// 메인 쓰레드가 처리한다
		std::future<string> future = promise.get_future();

		thread t(PromiseWorker, std::move(promise));

		string message = future.get();

		t.join();

		// 이 방법은 결론적으로 promise에 데이터를 넣어주고 future객체를 통해 받아주는 형태
	}

	// future를 사용하는 세번째 방법, std::packaged_task
	{
		std::packaged_task<int64(void)> task(Calculate);
		std::future<int64> future = task.get_future();

		std::thread t(TaskWorker, std::move(task));

		int64 sum = future.get();

		t.join();
	}

	// 결론)
	// mutex, condition_variable까지 가지 않고 단순한 애들을 처리할 수 있는
	// 특히나, 한 번 발생하는 이벤트에 유용하다!
	// 닭잡는데 소잡는 칼을 쓸 필요 없다!
	// 1) async
	// 원하는 함수를 비동기적으로 실행
	// 2) primise
	// 결과물을 promise를 통해 future로 받아줌
	// 3) packaged_task
	// 원하는 함수의 실행 결과를 packaged_task를 통해 future로 받아줌
}
```

### 01-12 캐시

![core](https://user-images.githubusercontent.com/80055816/135119879-da11fb29-d0de-42d4-a388-16a0102832ef.png){: width="70%" height="70%"}{: .align-center}

- 캐시 히트에 대한 설명은 영상 참조 ( 10 : 22 )

### 01-13 CPU 파이프라인
- 가시성 문제 영상 참조 ( 5: 55 )
- 코드 재배치 문제 영상 참조 ( 8 : 35 )
- C++ 11 이전에는 모든 모델이 싱글 쓰레드 기준으로 구성되었다
- C++ 11 이후에는 모든 모델이 멀티 쓰레드 환경을 고려했다

![Pipeline](https://user-images.githubusercontent.com/80055816/135208037-251f5aa8-4bd5-4927-971c-7d1e28881f04.png){: width="70%" height="70%"}{: .align-center}

### 01-14 메모리 모델
- 여러 쓰레드가 동일한 메모리에 동시 접근하면 경합 조건(Race Condition)이 일어난다
- 위의 문제를 해결하기 위한 방법으로는 Lock(mutex)를 이용한 상호 배타적(Mutual Exclusive) 접근 방법과 원자적(Atomic) 연산을 이용하는 방법이 있다
- atomic 연산에 한해( 13 : 58 ), 모든 쓰레드가 동일 객체에 대해서 동일한 수정 순서를 관찰 ( 04 : 10 )
- atomic 연산을 한다고 해가지고 가시성 문제와 코드 재배치 문제가 해결되는 것이 아니다 오해하지 말자

```cpp
num.store(1);
// 이 코드는 위의 코드와 같은 의미이다 ( 이 코드는 메모리 정책을 포함하고 있다 )
num.store(1, memory_order::memory_order_seq_cst);
```

```cpp
int main()
{
	ready = false;
	value = 0;
	thread t(Producer);
	thread t2(Consumer);

	t1.join();
	t2.join();

	// Memory Model ( 정책 )
	// 1) Sequentially Consistent (seq_cst)
	// 2) Acquire-Release (acquire, release)
	// 3) Relaxed (relaxed)

	// 1) seq_cst ( 가장 엄격 = 컴파일러 최적화 여지 적음 = 직관적 )
	// 가시성 문제 바로 해결! 코드 재배치 바로 해결!

	// 2) acquire-release
	// 딱 중간!
	// release 명령 이전의 메모리 명령들이, 해당 명령 이후로 재배치 되는 것을 금지
	// 그리고 acquire로 같은 변수를 읽는 쓰레드가 있다면
	// release 이전의 명령들이 -> acquire 하는 순간에 관찰 가능 ( 가시성 보장 )

	// 3) relaxed ( 자유롭다 = 컴파일러 최적화 여지 많음 = 직관적이지 않음 )
	// 너무나도 자유롭다!
	// 코드 재배치도 멋대로 가능! 가시성 해결 NO!
	// 가장 기본 조건 ( 동일 객체에 대한 동일 관전 순서만 보장 )

	// 인텔, AMD의 경우 애동초 순차적 일관성을 보장해서
	// seq_cst를 써도 별다른 부하가 없음
	// ARM의 경우 꽤 차이가 있다
}
```

### 01-15 Thread Local Storage

![tls](https://user-images.githubusercontent.com/80055816/135763529-189dc729-6908-48d9-b412-bf98537a8952.png){: width="70%" height="70%"}{: .align-center}

- 데이터를 TLS에 한꺼번에 가져 올 수 있다 ( 06 : 20 )
- TLS는 나만의 전역 메모리 같은 것이다 ( 자기 쓰레드의 TLS만 접근할 수 있다 )

```cpp
// 일반 전역 변수가 아니라 자기 쓰레드 TLS만 접근할 수 있는 공간이 생긴다 
thread_local int32 LThreadId = 0;

// 이렇게도 응용이 가능하다
thread_local queue<int32> q;

// Thread ID 발급
void ThreadMain(int32 threadId)
{
	LThreadId = threadId;

	while (true)
	{
		cout << "Hi I am Thread" << LThreadId << endl;
		this_thread::sleep_for(1s);
	}
}

int main()
{
	vector<thread> threads;

	for (int32 i = 0; i < 10; i++)
	{
		int32 threadId = i + 1;
		threads.push_back(thread(ThreadMain, threadId));
	}

	for (thread& t : threads)
		t.join();
}
```

### 01-16 Lock-Based Stack / Queue
- 락 기반의 큐와 스택을 만들어 보는 수업
- 락 프리 기반으로 뭔가 만드는 것 자체가 꾸준히 연구가 되고 있는 학문이다

### 01-17 Lock-Free Stack 1

```cpp
// Compare and Swap이 핵심!
/*
if (_head == node->next)
{
	_head = node;
	return true;
}
else
{
	node->next = _head;
	return false;
}
*/

while (_head.compare_exchange_weak(node->next, node) == false)
{
	//node->next = _head;
}
```

### 01-18 Lock-Free Stack 2

```cpp
// 1) head 읽기
// 2) head->next 읽기
// 3) head = head->next
// 4) data 추출해서 반환
// 5) 추출한 노드를 삭제
// 오늘의 주제는 5) 에서 TryPop() 함수를 이용하여 할당된 공간을 삭제해야 메모리 누수를
// 막을 수 있다 그런데 서로 다른 쓰레드가 TryPop() 함수를 동시에 접근하면 문제가 되니 이를 해결해 보자
```

### 01-19 Lock-Free Stack 3
- 오늘의 주제는 스마트 포인터를 활용해 TryPop()을 구현해보자
- 락 프리 프로그래밍은 생각나는데로 막 짜면 안된다 ( 31 : 45 )
- 락 프리 프로그래밍은 자주 할 일이 없다 ( 34 : 50 )

### 01-20 Lock-Free Queue
- 이 수업의 내용은 앞으로 사용되지 않을 내용이다
- Queue는 Push() 할때도 경합이 붙는다

### 01-21 ThreadManager
- 락 프리 스택,큐를 직접 만들어 쓰지말고 MS에서 제공하는 라이브러리를 이용하자

```cpp
// using을 사용할 경우 좋은점은 typedef랑 다르게 template을 대상으로도 작동을 잘 한다
template<typename T>
using Atomic = std::atomic<T>;
using Mutex = std::mutex;
using CondVar = std::condition_variable;
using UniqueLock = std::unique_lock<std::mutex>;
using LockGuard = std::lock_guard<std::mutex>;

// 인위적인 크래쉬를 만들어 내는 메크로
#define CRASH(cause)
// 특정 조건이 아니면 크래쉬를 만들어 내는 메크로
#define ASSERT_CRASH(expr)

// 오늘의 수업 주제는 지금까지 배웠던 기능들을 아래와 같이 랩핑 하는 것이다
int main()
{
	for (int32 i = 0; i < 5; i++)
	{
		GThreadManager->Launch(ThreadMain);
	}

	GThreadManager->Join();
}
```

### 01-22 Reader-Writer Lock
- Write를 하게 될 경우에는 상호 배타적으로 동작하고 Read를 할 경우 상호 배타적으로 동작하지 않는 특성을 지니는 락

### 01-23 DeadLock 탐지
- 그래프를 활용해 사이클을 탐지하고, 사이클이 탐지 되었다는것은 데드락 상황이라고 생각하면 된다

### 01-24 연습 문제
- 1 ~ MAX_NUMBER까지의 소수 개수를 멀티쓰레드를 활용하여 구하기

<br>

## Chapter 02 메모리 관리

### 02-1 Reference Counting
- Reference Counting 기술을 적용하면 어떤 객체가 참조되고 있는 개수를 카운팅 할 수 있다
- 멀티쓰레드 환경에서는 refCount 변수를 atomic 타입으로 선언해야 한다
- 스마트 포인터가 Reference Counting을 관리하면 경쟁 상황(race condition)을 막아준다

### 02-2 스마트 포인터
- 스마트 포인터의 순환(Cycle) 문제는 weak_ptr을 이용하여 shared_ptr을 보충해 줌으로써 해결할 수 있다 ( 15 : 17 )
- weak_ptr은 상대방의 생명주기에는 영향을 주지 않으며 상대방의 객체가 살아있는지를 테스트하기 위해 존재한다 ( 27 : 10 )

### 02-3 Allocator
- new 키워드 흐름을 프로그래머가 가로챌 수 있다 ( 05 : 15 )
- placement new 문법을 활용하면 메모리 할당과 생성자 호출을 분리할 수 있다 ( 16 : 16 )

### 02-4 Stomp Allocator
- VS툴을 이용하여 메모리 상태 보는 방법 ( 03 : 51 )
- Use-After-Free 문제는 스마트 포인터로 해결할 수 있다 ( 05 : 05 )
- Vector clear 관련 문제 ( 07 : 13 )
- Casting 관련 문제 ( 08 : 50 )
- 오늘의 주제는 메모리 오염과 관련된 문제를 잡기 위해서 넣어줄 기능
- 독립적인 프로그램 끼리는 서로 간섭할 수 없다 ( 14 : 06 )
- 우리가 사용하는 메모리는 가상 주소이다 ( 가상 메모리 )

![memory](https://user-images.githubusercontent.com/80055816/137866526-2f6bbe5a-bd62-4304-a357-84bdae871df6.png){: width="70%" height="70%"}{: .align-center}

- 페이지 관련 설명( 17 : 10 ), 페이지 단위로 보안 레벨을 설정 할 수 있다
- new 키워드는 운영체제에게 직접적으로 메모리 할당을 요청하는 명령어가 아니다 ( 22 : 00 )
- new, malloc을 이용하지 않고 윈도우 API를 이용하여 직접 메모리를 관리하면 실행속도는 떨어지지만 메모리 침범 이슈는 해결할 수 있다 ( 27 : 44 )
- Stomp allocator를 활용하면 Use-After-Free 문제를 해결할 수 있다
- Stomp allocator를 수정하면 오버플로우 문제도 해결할 수 있다 ( 34 : 45 )

### 02-5 STL Allocator
- STL들은 내부적으로 메모리 할당자 new, delete를 사용하고 있는데 이것을 고쳐보고 싶은것이 오늘의 주제
- 우리가 만든 Custom Allocator를 STL에 붙여보자

### 02-6 Memory Pool 1
- 메모리를 해제하지 말고 어딘가에 보관했다가 나중에 필요해지면 임시보관 장소에서 다시 꺼내 사용하는 개념

### 02-7 Memory Pool 2
- 이전 강의에서 아쉬운 점 첫번째, 여러개의 쓰레드들이 경합을 벌여야 한다 ( 00 : 38 )
- 이전 강의에서 아쉬운 점 두번째, 어떤 데이터를 넣어주기 위해서 그 데이터를 할당하기 위한 공간들도 같이 만들어 지는데 이를 좀더 개선한다 ( 01 : 00 )

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

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
- Lock-Free 계열의 코드는 직접 만들어서 사용하지 말자 ( 47 : 58 )

### 02-8 Memory Pool 3
- MS에서 제공한는 SList를 활용해보자( 00 : 38 )

### 02-9 Object Pool
- 동일한 클래스끼리 모아서 관리하는 풀을 오브젝트 풀이라고 한다

### 02-10 TypeCast
- modern c++ design 영문판 책이 유명하다
- 오늘의 주제는 캐스팅을 안전하게 하는 방법을 알아보자

<br>

## Chapter 03 네트워크 프로그래밍

### 03-1 소켓 프로그래밍 기초 1

![socket](https://user-images.githubusercontent.com/80055816/139092602-33de2d10-e0d6-4df9-8479-b490726d3697.png){: width="70%" height="70%"}{: .align-center}

### 03-2 소켓 프로그래밍 기초 2
- 무엇을 하는 함수입니까 라는 질문은 프로그래머라면 하면 안되는 질문이다 ( 구글링 하자 )
- WSAStartup() 함수와 WSACleanup() 함수는 세트라고 생각하면 된다
- 네트워크 상에서 사용하는 공식적인 규칙은 Big-Endian이다
- SOCKET 또한 종료시점에 closesocket() 함수를 이용해서 소켓 리소스를 반환해야 한다
- 이분야는 함수 단위로 쪼개면서 다 이해할 필요가 없고 전체적인 큰 흐름만 이해하면 된다 ( 31 : 00 )
- 서버코드에 있는 SOCKADDR_IN 변수에 클라이언트 주소가 들어간다
- 서버코드에 있는 SOCKET에 대해 잘 알아두자 ( 40 : 24 )
- 여러개 프로젝트 동시에 실행 시키는 방법 ( 43 : 17 )

### 03-3 TCP 서버 실습
- SOCKET에 관한 설명 복습 ( 06 : 25 )
- 에코 서버 설명 ( 09 : 09 )
- 블로킹 함수 설명 ( 12 : 34 )
- 블로킹 함수의 실질적인 동작 형태 ( 13 : 36 )

### 03-4 TCP vs UDP
- 네트워크 패킷을 전송할때 5가지 이상의 단계(정책)로 구분되어 진다 ( 어플리케이션, 트랜스포트, 네트워크, 데이터 링크, 피지컬 )
- 오늘의 주제는 트랜스포트 단계
- TCP의 연결 지향성 : 연결형 서비스, 연결을 위해 할당되는 논리적인 경로가 있다, 전송 순서가 보장된다
- UDP의 연결 지향성 : 비연결형 서비스, 연결이라는 개념이 없다, 전송 순서가 보장되지 않는다, 경계(Boundary)의 개념이 있다
- TCP 속도와 신뢰성 : 분실이 일어나면 책임지고 다시 전송한다, 물건을 주고 받을 상황이 아니면 일부만 보냄(흐름 / 혼잡제어), 고려할 것이 많으니 속도가 Bad
- UDP 속도와 신뢰성 : 분실에 대한 책임 없음(신뢰성 Bad), 일단 보내고 생각한다, 단순하기 때문에 속도가 Good
- TCP 데이터 경계 : 경계(Boundary)의 개념이 없다
- UDP 데이터 경계 : 경계(Boundary)의 개념이 있다

### 03-5 UDP 서버 실습
- Connected UDP 개념 ( 18 : 22 )

### 03-6 소켓 옵션
- setsockopt() 함수와 getsockopt() 함수를 이용해 소켓옵션을 설정하거나 가져올 수 있다
- SOL_SOCKET와 관련된 주요 옵션 ( 3 : 00 )

### 03-7 논블로킹 소켓

```cpp
// 블로킹(Blocking) 소켓 함수 완료 시점
// accept -> 접속한 클라가 있을 때
// connect -> 서버 접속 성공했을 때
// send, sendto -> 요청한 데이터를 송신 버퍼에 복사했을 때
// recv, recvfrom -> 수신 버퍼에 도착한 데이터가 있고, 이를 유저레벨 버퍼에 복사했을 때

// ::ioctlsocket() 함수를 이용해 논블로킹 방식의 소켓을 만들 수 있다

if (clientSocket == INVALID_SOCKET)
{
	// 원래 블록했어야 했는데... 너가 논블로킹으로 하라며?
	if (::WSAGetLastError() == WSAEWOULDBLOCK)
		continue;

	// Error
	break;
}
```

- 논블로킹 방식이 반드시 좋은것은 아니다 이유는 영상 참조 ( 18 : 40 )

### 03-8 Select 모델
- Select 모델은 윈도우, 리눅스 모두 존재한다

```cpp
// Select 모델 = (select 함수가 핵심이 되는)
// 소켓 함수 호출이 성공할 시점을 미리 알 수 있다!
// 문제 상황)
// 수신버퍼에 데이터가 없는데, read 한다거나!
// 송신버퍼가 꽉 찼는데, write 한다거나!
// - 블로킹 소켓 : 조건이 만족되지 않아서 블로킹되는 상황 예방
// - 논블로킹 소켓 : 조건이 만족되지 않아서 불필요하게 반복 체크하는 상황을 예방

// socket set
// 1) 읽기[ 2 ] 쓰기[ ] 예외(OOB)[ ] 관찰 대상 등록
// OutOfBand는 send() 마지막 인자 MSG_OOB로 보내는 특별한 데이터
// 받는 쪽에서도 recv OOB 세팅을 해야 읽을 수 있음
// 2) select(readSet, writeSet, exceptSet); -> 관찰 시작
// 3) 적어도 하나의 소켓이 준비되면 리턴 -> 낙오자는 알아서 제거됨
// 4) 남은 소켓 체크해서 진행

// fd_set set;
// FD_ZERO : 비운다
// ex) FD_ZERO(set);
// FD_SET : 소켓 s를 넣는다
// ex) FD_SET(s, &set);
// FD_CLR : 소켓 s를 제거
// ex) FD_CLR(s, &set);
// FD_ISSET : 소켓 s가 set에 들어있으면 0이 아닌 값을 리턴한다
```

### 03-9 WSAEventSelect 모델

```cpp
// WSAEventSelect = (WSAEventSelect 함수가 핵심이 되는)
// 소켓과 관련된 네트워크 이벤트를 [이벤트 객체]를 통해 감지

// 이벤트 객체 관련 함수들
// 생성 : WSACreateEvent (수동 리셋 Manual-Reset + Non-Signaled 상태 시작)
// 삭제 : WSACloseEvent
// 신호 상태 감지 : WSAWaitForMultipleEvents
// 구체적인 네트워크 이벤트 알아내기 : WSAEnumNetworkEvents

// 소켓 <-> 이벤트 객체 연동
// WSAEventSelect(socket, event, networkEvents);
// - 관심있는 네트워크 이벤트
// FD_ACCEPT : 접속한 클라가 있음 accept
// FD_READ : 데이터 수신 가능 recv, recvfrom
// FD_WRITE : 데이터 송신 가능 send, sendto
// FD_CLOSE : 상대가 접속 종료
// FD_CONNECT : 통신을 위한 연결 절차 완료
// FD_OOB

// 주의 사항
// WSAEventSelect 함수를 호출하면, 해당 소켓은 자동으로 넌블로킹 모드 전환
// accept() 함수가 리턴하는 소켓은 listenSocket과 동일한 속성을 갖는다
// - 따라서 clientSocket은 FD_READ, FD_WRITE 등을 다시 등록 필요
// - 드물게 WSAEWOULDBLOCK 오류가 뜰 수 있으니 예외 처리 필요
// 중요)
// - 이벤트 발생 시, 적절한 소켓 함수 호출해야 함
// - 아니면 다음 번에는 동일 네트워크 이벤트가 발생 X
// ex) FD_READ 이벤트 떴으면 recv() 호출해야 하고, 안하면 FD_READ 두 번 다시 X

// 1) count, event
// 2) waitAll : 모두 기다림? 하나만 완료 되어도 OK?
// 3) timeout : 타임아웃
// 4) 지금은 false
// return : 완료된 첫번째 인덱스
// WSAWaitForMultipleEvents

// 1) socket
// 2) eventObject : socket 과 연동된 이벤트 객체 핸들을 넘겨주면, 이벤트 객체를 non-signaled
// 3) networkEvent : 네트워크 이벤트 / 오류 정보가 저장
// WSAEnumNetworkEvents
```

### 03-10 Overlapped 모델 (이벤트 기반)
- 동기(synchronous : 동시에 일어나는) vs 비동기(asynchronous : 동시에 일어나지 않는)
- 지금까지 사용한 send, recv는 동기(synchronous) 함수
- 오늘의 수업은 Async-NonBlocking 조합에 대한 내용이다 자세한 내용은 영상 참조( 11 : 40 )

```cpp
// Overlapped IO (비동기 + 논블로킹)
// - Overlapped 함수를 건다 (WSARecv, WSASend)
// - Overlapped 함수가 성공했는지 확인 후
// -> 성공했으면 결과 얻어서 처리
// -> 실패했으면 사유를 확인

// 1) 비동기 입출력 소켓
// 2) WSABUF 배열의 시작 주소 + 개수 // Scatter-Gather
// 3) 보내고/받은 바이트 수
// 4) 상세 옵션인데 0
// 5) WSAOVERLAPPED 구조체 주소값
// 6) 입출력이 완료되면 OS가 호출할 콜백 함수
// WSASend
// WSARecv

// Overlapped 모델 (이벤트 기반)
// - 비동기 입출력 지원하는 소켓 생성 + 통지 받기 위한 이벤트 객체 생성
// - 비동기 입출력 함수 호출 (1에서 만든 이벤트 객체를 같이 넘겨줌)
// - 비동기 작업이 바로 완료되지 않으면, WSA_IO_PENDING 오류 코드
// 운영체제는 이벤트 객체를 signaled 상태로 만들어서 완료 상태 알려줌
// - WSAWaitForMultipleEvents 함수 호출해서 이벤트 객체의 signal 판별
// - WSAGetOverlappedResult 호출해서 비동기 입출력 결과 확인 및 데이터 처리

// 1) 비동기 소켓
// 2) 넘겨준 overlapped 구조체
// 3) 전송된 바이트 수
// 4) 비동기 입출력 작업이 끝날때까지 대기할지?
// false
// 5) 비동기 입출력 작업 관련 부가 정보. 거의 사용 안 함.
// WSAGetOverlappedResult
```

### 03-11 Overlapped 모델 (콜백 기반)

```cpp
// Overlapped 모델 (Completion Routine 콜백 기반)
// - 비동기 입출력 지원하는 소켓 생성
// - 비동기 입출력 함수 호출 (완료 루틴의 시작 주소를 넘겨준다)
// - 비동기 작업이 바로 완료되지 않으면, WSA_IO_PENDING 오류 코드
// - 비동기 입출력 함수 호출한 쓰레드를 -> Alertable Wait 상태로 만든다
// ex) WaitForSingleObjectEx, WaitForMultipleObjectsEx, SleepEx, WSAWAitForMultipleEvents
// - 비동기 IO 완료되면, 운영체제는 완료 루틴 호출
// - 완료 루틴 호출이 모두 끝나면, 쓰레드는 Alertable Wait 상태에서 빠져나온다

// 1) 오류 발생시 0 아닌 값
// 2) 전송 바이트 수
// 3) 비동기 입출력 함수 호출 시 넘겨준 WSAOVERLAPPED 구조체의 주소값
// 4) 0
//void CompletionRoutine()

// Select 모델
// - 장점) 윈도우/리눅스 공통. 
// - 단점) 성능 최하 (매번 등록 비용), 64개 제한
// WSAEventSelect 모델
// - 장점) 비교적 뛰어난 성능
// - 단점) 64개 제한
// Overlapped (이벤트 기반)
// - 장점) 성능
// - 단점) 64개 제한
// Overlapped (콜백 기반)
// - 장점) 성능
// - 단점) 모든 비동기 소켓 함수에서 사용 가능하진 않음 (accept). 빈번한 Alertable Wait으로 인한 성능 저하
// IOCP

// Reactor Pattern (~뒤늦게. 논블로킹 소켓. 소켓 상태 확인 후 -> 뒤늦게 recv send 호출)
// Proactor Pattern (~미리. Overlapped WSA~)
```

- APC에 대한 개념 설명 영상 참조 ( 13 : 20 )

### 03-12 Completion Port 모델

```cpp
// Overlapped 모델 (Completion Routine 콜백 기반)
// - 비동기 입출력 함수 완료되면, 쓰레드마다 있는 APC 큐에 일감이 쌓임
// - Alertable Wait 상태로 들어가서 APC 큐 비우기 (콜백 함수)
// 단점) APC큐 쓰레드마다 있다! Alertable Wait 자체도 조금 부담!
// 단점) 이벤트 방식 소켓:이벤트 1:1 대응

// IOCP (Completion Port) 모델
// - APC -> Completion Port (쓰레드마다 있는건 아니고 1개. 중앙에서 관리하는 APC 큐?)
// - Alertable Wait -> CP 결과 처리를 GetQueuedCompletionStatus
// 쓰레드랑 궁합이 굉장히 좋다!

// CreateIoCompletionPort
// GetQueuedCompletionStatus

<br>

## Chapter 04 네트워크 프로그래밍

### 04-1 Socket Utils
- SocketUtils 클래스는 소켓 관련 초기화를 담당
- NetAddress 클래스는 클라이언트 주소 관리 담당

### 04-2 IocpCore
- IocpCore 클래스는 IOPC의 핵심적인 부분을 담당

### 04-3 Server Service
- Service 클래스는 여러 기능들을 커스터 마이징 하는 역할을 한다

### 04-4 Session 1
- session이 하는 역할은 패킷을 받는일

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

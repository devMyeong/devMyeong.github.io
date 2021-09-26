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

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

---
title:  "Gof Design Pattern"

categories:
  - Design Pattern
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2021-08-25
last_modified_at: 2021-08-25
---

## Chapter 00 스트래티지 패턴

### 00-1 스트래티지 패턴 (Strategy Pattern)

```cpp
//----------------------------------------------------------------------------------------------------------------
// 스트래티지 패턴이란 여러 알고리즘을 하나의 추상적인 접근점을 만들어 접근 점에서 서로 교환 가능하도록 하는 패턴
//----------------------------------------------------------------------------------------------------------------

#include "stdafx.h"

class Weapon {
public:
	Weapon() {}
	virtual ~Weapon() {}

public:
	virtual void Attack() {}
};

class Knife : public Weapon
{
	// Weapon을(를) 통해 상속됨
	virtual void Attack() override
	{
		cout << "칼 공격" << endl;
	}
};

class Sword : public Weapon
{
	// Weapon을(를) 통해 상속됨
	virtual void Attack() override
	{
		cout << "검 공격" << endl;
	}
};

class Player
{
	// 접근점
private:
	Weapon* m_weapon;

public:
	Player() : m_weapon(nullptr) {}
	virtual ~Player() {}

	// 교환 가능
public:
	void setWeapon(Weapon* weapon)
	{
		m_weapon = weapon;
	}

	void Attack()
	{
		if (m_weapon == nullptr)
		{
			cout << "맨손 공격" << endl;
		}
		else
		{
			// 델리게이트 : 델리게이트란 특정 객체의 기능을 사용하기 위해 다른 객체의 기능을 호출하는 것
			m_weapon->Attack();
		}

	}
};

void main()
{
	Player* player = new Player();
	player->Attack();

	// new Knife()를 해줬으니 메모리 해제를 해줘야 하지만 생략한다
	player->setWeapon(new Knife());
	player->Attack();

	// new Sword()를 해줬으니 메모리 해제를 해줘야 하지만 생략한다
	player->setWeapon(new Sword());
	player->Attack();

	delete player;
	player = nullptr;
}
```

<br>

## Chapter 01 어댑터 패턴

### 01-1 어댑터 패턴(Adapter Pattern)

```cpp
//-------------------------------------------------------------------------
// 어댑터 패턴을 활용하면 연관성 없는 두 객체를 묶어 사용할 수 있다
// 어댑터 패턴을 활용하면 주어진 알고리즘을 요구사항에 맞춰 사용할 수 있다
// 아래 코드에서는 주어진 알고리즘을 Math로 가정
//-------------------------------------------------------------------------

#include "stdafx.h"

class Math
{
public:
	Math()						{					}
	virtual ~Math()				{					}

public:
	// 두배
	double twoTime(double num)	{ return num * 2;	}

	// 절반
	double half(double num)		{ return num / 2;	}

	// 강화된 알고리즘
	double doubled(double d)	{ return d * 2;		}

};

class Adapter
{
public:
	Adapter()					{					}
	virtual ~Adapter()			{					}

	// 원하는 기능
public:
	virtual float	twiceOf(float f)	= 0;
	virtual float	halfOf(float f)		= 0;

};

class AdapterImpl : public Adapter
{
public:
	AdapterImpl()				{					}
	virtual ~AdapterImpl()		{					}

	// 원하는 기능
public:
	float twiceOf(float f)
	{
		// Math 알고리즘을 바로 사용할 수 없어서 구현체에서
		// 주어진 알고리즘을 사용할 수 있게 변경해준다
		//return (float) Math().twoTime((double)f);
		return (float)Math().doubled((double)f);
	}
	float halfOf(float f)
	{
		cout << "절반 함수 호출 시작" << endl;
		return (float)Math().half((double)f);
	}

};

void main()
{
	Adapter* adapter = new AdapterImpl();

	cout << adapter->twiceOf((float)100.0) << endl;
	cout << adapter->halfOf((float)80.0) << endl;

	delete adapter;
	adapter = nullptr;

}
```

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

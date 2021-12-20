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
	Math() {}
	virtual ~Math() {}

public:
	// 두배
	double twoTime(double num) { return num * 2; }

	// 절반
	double half(double num) { return num / 2; }

	// 강화된 알고리즘
	double doubled(double d) { return d * 2; }

};

class Adapter
{
public:
	Adapter() {}
	virtual ~Adapter() {}

	// 원하는 기능
public:
	virtual float	twiceOf(float f) = 0;
	virtual float	halfOf(float f) = 0;

};

class AdapterImpl : public Adapter
{
public:
	AdapterImpl() {}
	virtual ~AdapterImpl() {}

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

## Chapter 02 템플릿 메소드 패턴

### 02-1 템플릿 메소드 패턴(Template Method Pattern)

```cpp
//----------------------------------------------
// 알고리즘의 구조를 메소드에 정의하고
// 하위 클래스에서 알고리즘 구조의
// 변경없이 알고리즘을 재정의 하는 패턴
//----------------------------------------------

#include "stdafx.h"
#include<string>

class GameConnectHelper
{
public:
	GameConnectHelper() {}
	virtual ~GameConnectHelper() {}

	// 외부에는 노출되면 안되고 하위 클래스 에서는 사용할 수 있어야 하는 알고리즘들
protected:
	virtual string doSecurity(string string) = 0;
	virtual bool authentication(string id, string password) = 0;
	virtual int authorization(string userName) = 0;
	virtual string connection(string info) = 0;

public:
	// 템플릿 메소드
	string requestConnection(string str)
	{
		// 보안 작업 -> 암호화 된 문자열을 복호화
		string decodedInfo = doSecurity(str);

		// 반환된 것을 가지고 아이디, 암호를 할당한다
		string id = "aaa";
		string password = "bbb";

		if (authentication(id, password) == false)
		{
			cout << "예외 처리 작업" << endl;
			cout << "아이디 암호 불일치" << endl;
		}

		string userName = "userName";
		int i = authorization(userName);

		switch (i)
		{
		case 0:
			cout << "게임 매니저" << endl;
			break;
		case 1:
			cout << "유료 회원" << endl;
			break;
		case 2:
			cout << "무료 회원" << endl;
			break;
		case 3:
			cout << "권한 없음" << endl;
			break;
		default:
			cout << "기타 상황" << endl;
			break;
		}

		return connection(decodedInfo);
	}

};

class DefaultGameConnectHelper : public GameConnectHelper
{
public:
	DefaultGameConnectHelper() {}
	virtual ~DefaultGameConnectHelper() {}

	// GameConnectHelper을(를) 통해 상속됨
	virtual string doSecurity(string string) override
	{
		cout << "디코드" << endl;
		return string;
	}
	virtual bool authentication(string id, string password) override
	{
		cout << "아이디/암호 확인 과정" << endl;
		return true;
	}
	virtual int authorization(string userName) override
	{
		// 셧다운제가 생겨서 청소년의 접속을 막아야 한다면
		// 이 함수에서 권한 확인을 한 뒤 청소년에 해당하는
		// 코드를 return 하면 된다
		cout << "권한 확인" << endl;
		return 0;
	}
	virtual string connection(string info) override
	{
		// 이 단계에서는 접속단계 에서 필요한 정보들을 넘겨준다
		cout << "마지막 접속단계!" << endl;
		return info;
	}
};

void main()
{
	GameConnectHelper* helper = new DefaultGameConnectHelper();

	helper->requestConnection("아이디 암호 등 접속 정보");

	delete helper;
	helper = nullptr;
}
```

<br>

## Chapter 03 팩토리 메소드

### 03-1 팩토리 메소드(Factory Method Pattern)

```cpp
//-------------------------------------------------------------------------------------
// Factory method는 부모(상위) 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며
// 자식(하위) 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이기도 하다
//-------------------------------------------------------------------------------------

#include "stdafx.h"

class Item
{
public:
	Item() {}
	virtual ~Item() {}

public:
	virtual void use() = 0;
};

class HpPotion : public Item
{
public:
	HpPotion() {}
	virtual ~HpPotion() {}

public:
	// Item을(를) 통해 상속됨
	virtual void use() override
	{
		cout << "체력 회복" << endl;
	}
};

class MpPotion : public Item
{
public:
	MpPotion() {}
	virtual ~MpPotion() {}

public:
	// Item을(를) 통해 상속됨
	virtual void use() override
	{
		cout << "마력 회복" << endl;
	}
};

class ItemCreator
{
public:
	ItemCreator() {}
	virtual ~ItemCreator() {}

public:
	// 팩토리 메소드
	Item* create()
	{
		Item* item;

		// step 1
		requestItemInfo();
		// step 2
		item = createItem();
		// step 3
		createItemLog();

		return item;
	}

protected:
	// 아이템을 생성하기 전에 데이터 베이스에서 아이템 정보를 요청합니다
	virtual void requestItemInfo() = 0;
	// 아이템을 생성 후 아이템 복제 등의 불법을 방지하기 위해 데이터 베이스에 아이템 생성 정보를 남깁니다
	virtual void createItemLog() = 0;
	// 아이템을 생성하는 알고리즘
	virtual Item* createItem() = 0;
};

class HpCreator : public ItemCreator
{
public:
	HpCreator() {}
	virtual ~HpCreator() {}

public:
	// ItemCreator을(를) 통해 상속됨
	virtual void requestItemInfo() override
	{
		cout << "데이터베이스에서 체력 회복 물약의 정보를 가져옵니다" << endl;
	}

	virtual void createItemLog() override
	{
		cout << "체력 회복 물약을 새로 생성 했습니다 + 현재 날짜 및 시간" << endl;
	}

	virtual Item * createItem() override
	{
		return new HpPotion();
	}

};

class MpCreator : public ItemCreator
{
public:
	MpCreator() {}
	virtual ~MpCreator() {}

public:
	// ItemCreator을(를) 통해 상속됨
	virtual void requestItemInfo() override
	{
		cout << "데이터베이스에서 마력 회복 물약의 정보를 가져옵니다" << endl;
	}

	virtual void createItemLog() override
	{
		cout << "마력 회복 물약을 새로 생성 했습니다 + 현재 날짜 및 시간" << endl;
	}

	virtual Item * createItem() override
	{
		return new MpPotion();
	}

};

void main(void)
{
	ItemCreator* creator = new HpCreator();
	Item* item = creator->create();
	item->use();

	delete creator;
	creator = nullptr;
	delete item;
	item = nullptr;

	creator = new MpCreator();
	item = creator->create();
	item->use();

	delete creator;
	creator = nullptr;
	delete item;
	item = nullptr;
}
```

<br>

## Chapter 04 싱글톤 패턴

### 04-1 싱글톤 패턴(Singleton Pattern)

```cpp
//------------------------------------------------------------------------------------------
// 소프트웨어 디자인 패턴에서 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, 생성자가
// 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는
// 최초의 생성자가 생성한 객체를 리턴한다 이와 같은 디자인 유형을 싱글턴 패턴이라고 한다
//------------------------------------------------------------------------------------------

#include "stdafx.h"

class SystemSpeaker
{
// 외부에서 new를 이용해서 생성하는 것을 막는다
private:
	SystemSpeaker() : m_iVolume(5) {}
	virtual ~SystemSpeaker() { }

public:
	static SystemSpeaker* GetInstance()
	{
		if (instance == nullptr)
		{
			// 스피커와 통신 할수 있게끔 하는 작업을 여기에 넣어주어야 한다
			instance = new SystemSpeaker();
		}
		return instance;
	}

	static void ReleaseInst()
	{
		if (instance)
		{
			delete instance;
			instance = nullptr;
			cout << "메모리가 정상적으로 해제되었습니다" << endl;
		}
		else
			cout << "이미 메모리가 해제되었습니다" << endl;
	}

	int GetVolume()
	{
		return this->m_iVolume;
	}

	void SetVolume(int volume)
	{
		this->m_iVolume = volume;
	}

private:
	static SystemSpeaker* instance;
	int m_iVolume;
};

SystemSpeaker* SystemSpeaker::instance = nullptr;

void main(void)
{
	SystemSpeaker* speaker1 = SystemSpeaker::GetInstance();
	SystemSpeaker* speaker2 = SystemSpeaker::GetInstance();

	// 5, 5 
	cout << speaker1->GetVolume() << endl;
	cout << speaker2->GetVolume() << endl;

	speaker1->SetVolume(11);
	// 11, 11
	cout << speaker1->GetVolume() << endl;
	cout << speaker2->GetVolume() << endl;

	speaker2->SetVolume(22);
	// 22, 22
	cout << speaker1->GetVolume() << endl;
	cout << speaker2->GetVolume() << endl;

	speaker1->ReleaseInst();
	speaker2->ReleaseInst();
}
```

<br>

## Chapter 05 프로토 타입 패턴

### 05-1 프로토 타입 패턴(Prototype Pattern) 1

```cpp
//--------------------------------------------------------------------------------------------------
// 프로토 타입 패턴이란 생산 비용이 높은 인스턴스를 복사를 통해서 쉽게 생성 할 수 있도록 하는 패턴
// 인스턴스 생산 비용이 높은 경우는 종류가 너무 많아서 클래스로 정리되지 않는 경우와 클래스로 부터
// 인스턴스 생성이 어려운 경우가 있다
//--------------------------------------------------------------------------------------------------

#include "stdafx.h"

class Unit {
public:
	virtual ~Unit() {}
	virtual Unit *clone() = 0;
};

class DoctorStrange : public Unit {
public:
	DoctorStrange(int hp, int atk, int spd) : mHp(hp), mAtk(atk), mSpd(spd) {}

	virtual Unit *clone() {
		// 깊은 복사가 이루어지고 있다
		return new DoctorStrange(mHp, mAtk, mSpd);
	}

private:
	int mHp;
	int mAtk;
	int mSpd;
};

class CopyUnit {
public:
	CopyUnit(Unit *prototype) : mPrototype(prototype) {}
	Unit *spawn() {
		return mPrototype->clone();
	}

private:
	Unit *mPrototype;
};

void main(void)
{
	DoctorStrange* doctorStrange = new DoctorStrange(100, 50, 25);
	CopyUnit* copyUnit = new CopyUnit(doctorStrange);
}
```

### 05-2 프로토 타입 패턴(Prototype Pattern) 2
- 프로토 타입 패턴을 사용할때는 깊은 복사와 얕은 복사중에 어떤 복사를 선택할 것인지 잘 판단해야 한다

<br>

## Chapter 06 빌더 패턴

### 06-1 빌더 패턴(Builder Pattern) 1

```cpp
//-----------------------------------------------------------------------------------------
// 빌더 패턴이란 복잡한 단계를 거쳐야 생성되는 객체의 구현을 서브 클래스에게 넘겨주는 패턴
//-----------------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>

class Computer;

class BluePrint
{
public:
	BluePrint() {}
	virtual ~BluePrint() {}

public:
	virtual void setCpu() = 0;
	virtual void setRam() = 0;
	virtual void setStorage() = 0;
	virtual Computer* getComputer() = 0;
};

class ComputerFactory
{
public:
	ComputerFactory() {}
	virtual ~ComputerFactory() {}

public:
	void setBlueprint(BluePrint* print)
	{
		this->print = print;
	}
	void make()
	{
		print->setRam();
		print->setCpu();
		print->setStorage();
	}

	Computer* getComputer()
	{
		return print->getComputer();
	}

private:
	BluePrint* print;
};

class Computer
{
public:
	Computer(string cpu, string ram, string storage) : cpu(cpu), ram(ram), storage(storage)
	{
	}
	virtual ~Computer() {}

public:
	string getCpu()
	{
		return cpu;
	}
	void setCpu(string cpu)
	{
		this->cpu = cpu;
	}
	string getRam()
	{
		return ram;
	}
	void setRam(string ram)
	{
		this->ram = ram;
	}
	string getStorage()
	{
		return storage;
	}
	void setStorage(string storage)
	{
		this->storage = storage;
	}
	string toString()
	{
		return cpu + ", " + ram + ", " + storage;
	}

private:
	string cpu;
	string ram;
	string storage;
};

class LgGramBlueprint : public BluePrint
{
public:
	LgGramBlueprint() { }
	virtual ~LgGramBlueprint() { }

public:


	// BluePrint을(를) 통해 상속됨
	virtual void setCpu() override
	{
		cpu = "i7";
	}

	virtual void setRam() override
	{
		ram = "8g";
	}

	virtual void setStorage() override
	{
		storage = "SSD 256";
	}

	virtual Computer* getComputer()
	{
		return new Computer(cpu, ram, storage);
	}

private:
	string cpu;
	string ram;
	string storage;

};

void main(void)
{
	ComputerFactory* factory = new ComputerFactory();
	
	// 팩토리에 LgGramBlueprint라는 설계도를 넣어준다
	factory->setBlueprint(new LgGramBlueprint());
	
	// 설계도를 바탕으로 만든다
	factory->make();
	
	// 만들어진 결과값을 가지고 온다
	Computer* computer = factory->getComputer();

	cout << computer->toString() << endl;

	delete factory;
	factory = nullptr;

	delete computer;
	computer = nullptr;
}
```

### 06-2 빌더 패턴(Builder Pattern) 2

```cpp
//--------------------------------------------------------------------
// 많은 인자를 가진 객체의 생성을 다른 객체의 도움으로 생성하는 패턴
//--------------------------------------------------------------------

#include "stdafx.h"
#include <string>

class Computer
{
public:
	// 매개 변수가 여기서는 3개지만 100개쯤 된다고 가정해보자
	Computer(string cpu, string ram, string storage) : cpu(cpu), ram(ram), storage(storage)
	{
	}
	virtual ~Computer() {}

public:
	string getCpu()
	{
		return cpu;
	}
	void setCpu(string cpu)
	{
		this->cpu = cpu;
	}
	string getRam()
	{
		return ram;
	}
	void setRam(string ram)
	{
		this->ram = ram;
	}
	string getStorage()
	{
		return storage;
	}
	void setStorage(string storage)
	{
		this->storage = storage;
	}
	string toString()
	{
		return cpu + ", " + ram + ", " + storage;
	}

private:
	string cpu;
	string ram;
	string storage;
};

class ComputerBuilder
{
private:
	// 매개 변수가 여기서는 3개지만 100개쯤 된다고 가정해보자
	ComputerBuilder() { computer = new Computer("default", "default", "default"); }
	virtual ~ComputerBuilder() { delete computer; computer = nullptr; }

public:
	static ComputerBuilder* start()
	{
		return new ComputerBuilder();
	}

	static ComputerBuilder* startWithCpu(string cpu)
	{
		ComputerBuilder* builder = new ComputerBuilder();
		builder->computer->setCpu(cpu);
		return builder;
	}

	ComputerBuilder* setCpu(string string)
	{
		computer->setCpu(string);
		return this;
	}

	ComputerBuilder* setRam(string string)
	{
		computer->setRam(string);
		return this;
	}

	ComputerBuilder* setStorage(string string)
	{
		computer->setStorage(string);
		return this;
	}

	Computer* build()
	{
		return this->computer;
	}

private:
	Computer* computer;
};

void main(void)
{
	// 지금 넣어주고 있는 값이 무엇인지 함수명을 보고 알수있는 장점이 있다 ( 방식 1 )
	Computer* computer = ComputerBuilder::start()->setCpu("i7")->setRam("8g")->setStorage("128g SSD")->build();

	// 지금 넣어주고 있는 값이 무엇인지 함수명을 보고 알수있는 장점이 있다 ( 방식 2 )
	//Computer* computer = ComputerBuilder::start()->startWithCpu("i7")->setRam("8g")->setStorage("128g SSD")->build();

	cout << computer->toString() << endl;
}
```

<br>

## Chapter 07 추상 팩토리 패턴

### 07-1 추상 팩토리 패턴(Abstract Factory Pattern) 1

```cpp
//-------------------------------------------------------------------------------------------
// 관련있는 클래스를 하나의 팩토리로 묶어줘서 동일한 방식으로 생성할 수 있게 도와주는 패턴
//-------------------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>

class Body
{
public:
	Body() {}
	virtual ~Body() {}
};

class Wheel
{
public:
	Wheel() {}
	virtual ~Wheel() {}
};

class SamBody : public Body
{

};

class SamWheel : public Wheel
{

};

class GtBody : public Body
{

};

class GtWheel : public Wheel
{

};

class BikeFactory
{
public:
	BikeFactory() {}
	virtual ~BikeFactory() {}

public:
	virtual Body* createBody() = 0;
	virtual Wheel* createWheel() = 0;
};

class SamFactory : public BikeFactory
{
public:
	SamFactory() {}
	virtual ~SamFactory() {}

	// BikeFactory을(를) 통해 상속됨
	virtual Body * createBody() override
	{
		return new SamBody();
	}
	virtual Wheel * createWheel() override
	{
		return new SamWheel();
	}
};

class GtBikeFactory : public BikeFactory
{
public:
	GtBikeFactory() {}
	virtual ~GtBikeFactory() {}

	// BikeFactory을(를) 통해 상속됨
	virtual Body * createBody() override
	{
		return new GtBody();
	}
	virtual Wheel * createWheel() override
	{
		return new GtWheel();
	}
};

void main(void)
{
	//BikeFactory* factory = new SamFactory();
	BikeFactory* factory = new GtBikeFactory();

	Body* body = factory->createBody();
	Wheel* wheel = factory->createWheel();
}
```

### 07-2 추상 팩토리 패턴(Abstract Factory Pattern) 2

```cpp
//-------------------------------------------------------------------------------------------
// 관련있는 클래스를 하나의 팩토리로 묶어줘서 동일한 방식으로 생성할 수 있게 도와주는 패턴
//-------------------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>

class Button;
class TextArea;

class Button
{
public:
	Button() {}
	virtual ~Button() {}

public:
	virtual void click() = 0;
};

class TextArea
{
public:
	TextArea() {}
	virtual ~TextArea() {}

public:
	virtual string getText() = 0;
};

class LinuxButton : public Button
{
public:
	LinuxButton() {}
	virtual ~LinuxButton() {}

	// Button을(를) 통해 상속됨
	virtual void click() override
	{
		cout << "리눅스 버튼" << endl;
	}
};

class LinuxTextArea : public TextArea
{
public:
	LinuxTextArea() {}
	virtual ~LinuxTextArea() {}

	// TextArea을(를) 통해 상속됨
	virtual string getText() override
	{
		return "리눅스 텍스트 에어리어";
	}
};

class MacButton : public Button
{
public:
	MacButton() {}
	virtual ~MacButton() {}

	// Button을(를) 통해 상속됨
	virtual void click() override
	{
		cout << "맥 버튼" << endl;
	}
};

class MacTextArea : public TextArea
{
public:
	MacTextArea() {}
	virtual ~MacTextArea() {}

	// TextArea을(를) 통해 상속됨
	virtual string getText() override
	{
		return "맥 텍스트 에어리어";
	}
};

class WinButton : public Button
{
public:
	WinButton() {}
	virtual ~WinButton() {}

	// Button을(를) 통해 상속됨
	virtual void click() override
	{
		cout << "윈도우 버튼" << endl;
	}
};

class WinTextArea : public TextArea
{
public:
	WinTextArea() {}
	virtual ~WinTextArea() {}

	// TextArea을(를) 통해 상속됨
	virtual string getText() override
	{
		return "윈도우 텍스트 에어리어";
	}
};

class GuiFac
{
public:
	GuiFac() {}
	virtual ~GuiFac() {}

public:
	virtual Button* createButton() = 0;
	virtual TextArea* createTextArea() = 0;
};

class LinuxGuiFac : public GuiFac
{
public:
	LinuxGuiFac() {}
	virtual ~LinuxGuiFac() {}

	// GuiFac을(를) 통해 상속됨
	virtual Button* createButton() override
	{
		return new LinuxButton();
	}
	virtual TextArea * createTextArea() override
	{
		return new LinuxTextArea();
	}
};

class MacGuiFac : public GuiFac
{
public:
	MacGuiFac() {}
	virtual ~MacGuiFac() {}

	// GuiFac을(를) 통해 상속됨
	virtual Button* createButton() override
	{
		return new MacButton();
	}
	virtual TextArea * createTextArea() override
	{
		return new MacTextArea();
	}
};

class WinGuiFac : public GuiFac
{
public:
	WinGuiFac() {}
	virtual ~WinGuiFac() {}

	// GuiFac을(를) 통해 상속됨
	virtual Button* createButton() override
	{
		return new WinButton();
	}
	virtual TextArea * createTextArea() override
	{
		return new WinTextArea();
	}
};

class FactoryInstance
{
public:
	FactoryInstance() {}
	virtual ~FactoryInstance() {}

public:
	// 이 부분을 라이브러리 형태로 제공한다고 가정한다 즉 각각의 팩토리를 감춘다
	static GuiFac* getGuiFac()
	{
		// 이 부분에서 어떤 팩토리가 생성될지 선택된다
		switch (getOsCode())
		{
		case 0:	return new MacGuiFac();
		case 1:	return new LinuxGuiFac();
		case 2:	return new WinGuiFac();
		}
	}
	
private:
	static int getOsCode()
	{
		return 0;
	}
};

void main(void)
{
	GuiFac* fac = FactoryInstance::getGuiFac();

	// 이 밑에 소스들은 건들일 필요가 없다
	Button* button = fac->createButton();
	TextArea* area = fac->createTextArea();

	button->click();
	cout << area->getText() << endl;
}
```

<br>

## Chapter 08 브릿지 패턴

### 08-1 브릿지 패턴 (Bridge Pattern)

```cpp
//-----------------------------------------------------------------------------------------
// 브릿지 패턴 에서는 기능 계층과 구현 계층을 분리한다
//-----------------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>

class MorseCodeFunction
{
public:
	MorseCodeFunction() {}
	virtual ~MorseCodeFunction() {}

public:
	virtual void dot()	 = 0;
	virtual void dash()	 = 0;
	virtual void space() = 0;
};

class DefaultMCF : public MorseCodeFunction
{
public:
	DefaultMCF() {}
	virtual ~DefaultMCF() {}

	// MorseCodeFunction을(를) 통해 상속됨
	void dot() override
	{
		cout << ".";
	}
	void dash() override
	{
		cout << "-";
	}
	void space() override
	{
		cout << " ";
	}
};

class SoundMCF : public MorseCodeFunction
{
public:
	SoundMCF() {}
	virtual ~SoundMCF() {}

	// MorseCodeFunction을(를) 통해 상속됨
	void dot() override
	{
		cout << "삣";
	}
	void dash() override
	{
		cout << "삐~";
	}
	void space() override
	{
		cout << " ";
	}
};

class FlashMCF : public MorseCodeFunction
{
public:
	FlashMCF() {}
	virtual ~FlashMCF() {}

	// MorseCodeFunction을(를) 통해 상속됨
	void dot() override
	{
		cout << " 번쩍 ";
	}
	void dash() override
	{
		cout << " 반짝 ";
	}
	void space() override
	{
		cout << " - ";
	}
};

class MorseCode
{
private:
	MorseCodeFunction* function;

public:
	MorseCode(MorseCodeFunction* function)
	{
		this->function = function;
	}
	virtual ~MorseCode() {}

	void setFunction(MorseCodeFunction* function)
	{
		this->function = function;
	}

	void dot()
	{
		// 델리게이트
		function->dot();
	}
	void dash()
	{
		// 델리게이트
		function->dash();
	}
	void space()
	{
		// 델리게이트
		function->space();
	}
};

class PrintMorseCode : public MorseCode
{
public:
	PrintMorseCode(MorseCodeFunction* function) : MorseCode(function) {}
	virtual ~PrintMorseCode() {}

public:
	PrintMorseCode* g()
	{
		dash(); dash(); dot(); space();
		return this;
	}
	PrintMorseCode* a()
	{
		dot(); dash(); space();
		return this;
	}
	PrintMorseCode* r()
	{
		dot(); dash(); dot(); space();
		return this;
	}
	PrintMorseCode* m()
	{
		dash(); dash(); space();
		return this;
	}
};

void main(void)
{
	//PrintMorseCode* code = new PrintMorseCode(dynamic_cast<MorseCodeFunction*>(new DefaultMCF()));
	//PrintMorseCode* code = new PrintMorseCode(dynamic_cast<MorseCodeFunction*>(new SoundMCF()));
	PrintMorseCode* code = new PrintMorseCode(dynamic_cast<MorseCodeFunction*>(new FlashMCF()));

	// 체이닝 적용 안한 상태
	//code->g(); code->a(); code->r(); code->a(); code->r();

	// 체이닝 적용한 상태
	code->g()->a()->r()->a()->m();
}
```

<br>

## Chapter 09 컴포짓 패턴

### 09-1 컴포짓 패턴 (Composite Pattern)

![tree](https://user-images.githubusercontent.com/80055816/141460255-8ce6c1cf-8fae-4ec0-a239-6db663b4b89d.PNG){: width="70%" height="70%"}{: .align-center}

- 오늘은 위의 트리 형태를 기반으로 컨테이너와 내용물을 같게 다룬는 패턴을 배운다

```cpp
//-----------------------------------------------------------------------------------------
// 컴포짓 패턴 에서는 컨테이너와 내용물을 같게 다룬다
//-----------------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>
#include <list>

class Component
{
private:
	string name;

public:
	Component(string name) { this->name = name; }
	virtual ~Component() {}
};

class File : public Component
{
public:
	File(string name) : Component(name) {}
	virtual ~File() {}

private:
	string data;
};

class Folder : public Component
{
private:
	list<Component*> children;

public:
	Folder(string name) : Component(name) {}
	virtual ~Folder() {}

	void addComponent(Component* componet)
	{
		children.push_back(componet);
	}

	void removeComponet(Component* componet)
	{
		children.remove(componet);
	}

};

void main(void)
{
	Folder *root = new Folder("root"), *home = new Folder("home"), *garam = new Folder("garam"), *music = new Folder("music"), *picture = new Folder("picture"), *doc = new Folder("doc"), *usr = new Folder("usr");
	File *track1 = new File("track1"), *track2 = new File("track2"), *pic1 = new File("pic1"), *doc1 = new File("doc1"), *java = new File("java");

	root->addComponent(home);
		home->addComponent(garam);
			garam->addComponent(music);
				music->addComponent(track1);
				music->addComponent(track2);
			garam->addComponent(picture);
				picture->addComponent(pic1);
			garam->addComponent(doc);
				doc->addComponent(doc1);

	root->addComponent(usr);
		usr->addComponent(java);
}
```

<br>

## Chapter 10 데코레이터 패턴

### 10-1 데코레이터 패턴 (Decorator Pattern)

```cpp
//-----------------------------------------------------------------
// 동적으로 책임 추가가 필요할 때 데코레이터 패턴을 사용할 수 있다 
//-----------------------------------------------------------------

#include "stdafx.h"
#include <string>
#include <list>

class AirPlane { virtual void Attack() = 0; }
class FrontAttackAirPlane : public AirPlane
{
    void Attack() { cout << "Front Attack"; }
};
class Decorator : public AirPlane
{
    Decorator( AirPlane* pAir ) { pComponent = pAir; }
    virtual void Attack()
	{
        if ( pComponent != 0 ) { pComponent->Attack(); }
    }
private:
    AirPlane* pComponent;
};
class SideAttackAirPlane : public Decorator
{
    SideAttackAirPlane(AirPlane* pAir) : Decorator(pAir) {}
    void Attack()
	{ 
        Decorator::Attack();
        cout << "Side Attack"; 
    }
};
class RearAttackAirPlane : public Decorator
{
    RearAttackAirPlane(AirPlane* pAir) : Decorator(pAir) {}
    void Attack()
	{ 
        Decorator::Attack();
        cout << "Rear Attack"; 
    }
};

int main()
{
    AirPlane* pFront = new FrontAttackAirPlane;		// 기본 공격
    AirPlane* pSide = new SideAttackAirPlane(pFront);
    AirPlane* pRear = new RearAttackAirPlane(pFront);
    
    pRear->Attack();
}
```

<br>

## Chapter 11 방문자 패턴

### 11-1 방문자 패턴 (Visitor Pattern)

```cpp
//------------------------------------------------------------------
// 방문자 패턴을 이용하여 객체에서 처리를 분리할 수 있다
//------------------------------------------------------------------

#include "stdafx.h"
#include <string>
#include <list>
#include <vector>

#include<iostream>
#include <list>
using namespace std;

class mylist;

class visitor {
public:
	virtual void visit(mylist* l) = 0;
};

class Acceptor {
	virtual void accept(visitor* v) = 0;
};


class mylist : public Acceptor {
	list<int> data = { 1,2,3,4,5,6,7,8,9,10 };

public:
	list<int>* getList() { return &data; }
	virtual void accept(visitor *v)
	{
		v->visit(this);
	}

};

class doubleVisitor : public visitor
{
public:
	virtual void visit(mylist* l)
	{
		list<int>* p = l->getList();
		for (auto& x : *p)
			x *= 2;
	}

};
class printVisitor : public visitor
{
public:
	virtual void visit(mylist* l)
	{
		list<int>* p = l->getList();
		for (auto x : *p)
			cout << x << endl;
	}

};

int main()
{
	mylist x;
	doubleVisitor dv;
	x.accept(&dv);

	printVisitor pv;
	x.accept(&pv);
}
```

<br>

## Chapter 12 책임사슬 패턴

### 12-1 책임사슬 패턴 (Chain of Resposibility)

```cpp
//-----------------------------------------------------------------------
// 책임사슬 패턴을 활용하여 다양한 처리 방식을 유연하게 연결 할 수 있다
//-----------------------------------------------------------------------

#include "stdafx.h"
#include <string>
#include <list>
#include <vector>

#include<iostream>
#include <list>
using namespace std;

// 소스코드 출처 : copynull.tistory.com/143
// 문의자 인터페이스 클래스
class 문의자
{
public:
	virtual const TCHAR* Class() = 0;
};
class 초등학생 : public 문의자
{
public:
	const TCHAR* Class() { return L"초등학생"; }
};
class 중학생 : public 문의자
{
public:
	const TCHAR* Class() { return L"중학생"; }
};
class 고등학생 : public 문의자
{
public:
	const TCHAR* Class() { return L"고등학생"; }
};

//------------------------------------------------------------------
// 수학상담문의 인터페이스 클래스
class 수학상담문의
{
public:
	수학상담문의(수학상담문의* pHandle) : pHandler(pHandle) {}
	~수학상담문의() { if (pHandler) delete pHandler; }

public:
	virtual void 상담문의(문의자* pUser)
	{
		if (pHandler != NULL)
			pHandler->상담문의(pUser);
	}

private:
	수학상담문의* pHandler;
};

//------------------------------------------------------------------
// 초등수학상담 상속 클래스
class 초등수학상담 : public 수학상담문의
{
public:
	초등수학상담(수학상담문의* pHandle) : 수학상담문의(pHandle) {}

public:
	void 상담문의(문의자* pUser) override
	{
		if (dynamic_cast<초등학생*>(pUser) != NULL)
			cout << "초등학생 문의 상담입니다." << endl;
		else
			수학상담문의::상담문의(pUser);
	}
};

//------------------------------------------------------------------
// 중등수학상담 상속 클래스
class 중등수학상담 : public 수학상담문의
{
public:
	중등수학상담(수학상담문의* pHandle) : 수학상담문의(pHandle) {}

public:
	void 상담문의(문의자* pUser) override
	{
		if (dynamic_cast<중학생*>(pUser) != NULL)
			cout << "중학생 문의 상담입니다." << endl;
		else
			수학상담문의::상담문의(pUser);
	}
};

//------------------------------------------------------------------
// 고등수학상담 상속 클래스
class 고등수학상담 : public 수학상담문의
{
public:
	고등수학상담(수학상담문의* pHandle) : 수학상담문의(pHandle) {}

public:
	void 상담문의(문의자* pUser) override
	{
		if (dynamic_cast<고등학생*>(pUser) != NULL)
			cout << "고등학생 문의 상담입니다." << endl;
		else
			수학상담문의::상담문의(pUser);
	}
};

//------------------------------------------------------------------
// Main
int _tmain(int argc, _TCHAR* argv[])
{
	문의자* pUser = new 고등학생();
	수학상담문의* pHandler = new 초등수학상담(new 중등수학상담(new 고등수학상담(NULL)));

	pHandler->상담문의(pUser);

	delete pHandler;
	delete pUser;

	return 0;
}
```

<br>

### 12-2 책임사슬 패턴 (Chain of Resposibility)

```cpp
//-------------------------------------------------------------------------------
// 책임사슬 패턴을 동적으로 활용하여 다양한 처리 방식을 유연하게 연결 할 수 있다
//-------------------------------------------------------------------------------

#include "stdafx.h"
#include <string>
#include <list>
#include <vector>

#include<iostream>
#include <list>

using namespace std;

class Attack
{
private:
	int m_Amount;

public:
	Attack(int amount)
	{
		m_Amount = amount;
	}
	virtual ~Attack() = default;

	void setAmount(int amount)
	{
		m_Amount = amount;
	}

	int getAmount()
	{
		return m_Amount;
	}

};

class Defense
{
public:
	virtual void defense(Attack* attack) = 0;
};

class Armor : public Defense
{
public:
	Armor()
	{

	}

	Armor(int def)
	{
		m_def = def;
	}

private:
	Defense* m_nextDefense;
	int m_def; // 방어력

private:
	void proccess(Attack* attack)
	{
		int amount = attack->getAmount();
		amount -= m_def;
		attack->setAmount(amount);
	}

public:
	// Defense을(를) 통해 상속됨
	virtual void defense(Attack* attack) override
	{
		// 이 부분이 Key Point !!
		proccess(attack);

		if (m_nextDefense != nullptr)
		{
			m_nextDefense->defense(attack);
		}
	}

	void SetDef(int def)
	{
		m_def = def;
	}

	void SetNextDefense(Defense* nextDefense)
	{
		m_nextDefense = nextDefense;
	}

};

int _tmain(int argc, _TCHAR* argv[])
{
	Attack* attack = new Attack(100);
	Armor* armor1  = new Armor(10);
	Armor* armor2  = new Armor(15);

	armor1->SetNextDefense(armor2);

	armor1->defense(attack);

	cout << attack->getAmount() << endl;
}
```

<br>

## Chapter 13 퍼사드 패턴

### 13-1 퍼사드 패턴 (Facade)

```cpp
//------------------------------------------------------------------------------------------------------
// 어떠한 일련의 과정을 간단하게 표현하는 패턴 // 소스 출처 : m.blog.naver.com/sasayakki/221301960065
//------------------------------------------------------------------------------------------------------
class CarFacade
{
public:
    Car car;

    CarFacade(Car car)
	{
        this.car = car;
    }

    void drive()
	{
        car.enginStart();
        car.doorLock();
        car.wheelsRoll();
    }

    void stop()
	{
        car.enginStop();
        car.doorUnlock();
        car.wheelsStop();
    }

    void park()
	{
        car.enginStop();
        car.doorLock();
        car.wheelsStop();
    }

}

class Car
{
    public void enginStop(){ cout << "engine stop"; }
    public void enginStart(){ cout << "engine start"; }
    public void doorLock(){ cout << "door locked"; }
    public void doorUnlock(){ cout << "door unlocked"; }
    public void wheelsRoll(){ cout << "wheels roll"; }
    public void wheelsStop(){ cout << "wheels stop"; }
}
```

<br>

## Chapter 14 옵저버 패턴

### 14-1 옵저버 패턴 (Observer) 1

```cpp
//----------------------------------------------------------------------------------------------------------------
// 옵저버 패턴을 활용하면 이벤트 발생후 객체 외부에서 처리를 할 수 있다 // 소스 출처 : copynull.tistory.com/140
//----------------------------------------------------------------------------------------------------------------

#include <string>
#include <list>
#include <vector>
#include <iostream>
#include <atlcoll.h>

using namespace std;

//------------------------------------------------------------------
// Observer 인터페이스
class 옵저버_인터페이스
{
public:
	virtual void 방송시청(const string news) = 0;
	virtual const string 이름() const = 0;
};

//------------------------------------------------------------------
// ConcreteObserver 상속 클래스
class 시청자 : public 옵저버_인터페이스
{
public:
	시청자(const string name) : mName(name) {}

public:
	void 방송시청(const string news) override { cout << mName << " : " << news << endl; }
	const string 이름() const { return mName.c_str(); }

private:
	string mName;
};

//------------------------------------------------------------------
// Subject 인터페이스 (옵저버들 관리 및 통지)
class 방송사_인터페이스
{
public:
	void 방송시청등록(옵저버_인터페이스* o) { mList.AddTail(o); cout << "방송 신청 : " << o->이름() << endl; }
	void 방송시청해제(옵저버_인터페이스* o) { mList.RemoveAt(mList.Find(o));  cout << "방송 해지 : " << o->이름() << endl; }

protected:
	void 송출(const string news)
	{
		POSITION pos = mList.GetHeadPosition();
		while (pos != NULL)
			mList.GetNext(pos)->방송시청(news);
	}

public:
	virtual void 뉴스(const string news) = 0;

private:
	CAtlList<옵저버_인터페이스*> mList;
};

//------------------------------------------------------------------
// ConcreteSubject 상속 클래스
class 코리아방송 : public 방송사_인터페이스
{
public:
	void 뉴스(const string news) override { 송출(news); }
};


//------------------------------------------------------------------
// Main
void main(void)
{
	방송사_인터페이스* pSubject = new 코리아방송;

	옵저버_인터페이스* pObserver1 = new 시청자("홍길동");
	옵저버_인터페이스* pObserver2 = new 시청자("태평양");
	옵저버_인터페이스* pObserver3 = new 시청자("비둘기");

	pSubject->방송시청등록(pObserver1);
	pSubject->방송시청등록(pObserver2);
	pSubject->방송시청등록(pObserver3);

	pSubject->뉴스("날씨를 말씀드리겠습니다.");

	pSubject->방송시청해제(pObserver1);
	pSubject->방송시청해제(pObserver2);

	pSubject->뉴스("뉴스 속보입니다.");

	delete pSubject;
	delete pObserver1;
	delete pObserver2;
	delete pObserver3;

}
```

### 14-2 옵저버 패턴 (Observer) 2
- 자바에서 기본적으로 제공하는 클래스와 인터페이스를 활용하여 옵저버 패턴을 구현한다

### 14-3 옵저버 패턴 (Observer) 3
- 제네릭
- 델리게이트
- 내부에 옵저버를 넣는다

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

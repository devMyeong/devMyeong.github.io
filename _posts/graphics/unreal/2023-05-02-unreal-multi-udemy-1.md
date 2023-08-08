---
title:  "Unreal C++ Multiplayer Master: Intermediate Game Development (1)"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2023-05-02
last_modified_at: 2023-05-02
---

## Chapter 1 Puzzle Platforms - Connecting Players

### 01-1 Course Promo
- So this course isn't for beginners Like a lot of our courses, it actually requires that you have experience making a couple of games with Unreal and C++

### 01-2 Introduction to Puzzle Platforms
- So, yeah, let's get started in to this section where we're going to be once again connecting between instances on the same machine or on the local network and getting a general overview of how the multiplayer space looks with unreal

### 01-3 Differences Between UE5 and UE4
- So in this vignette we've seen how Unreal four and Unreal five are actually more similar than they at first appear

### 01-4 Connecting Two Players

![two](https://user-images.githubusercontent.com/80055816/235619210-7668a04e-78b4-46be-afa8-fd4caf2e085c.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/235620178-5a129919-2493-4eef-b565-4d60a2c4e50c.PNG){: width="100%" height="100%"}{: .align-center}

![modeworld](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/e49bbf66-6b7a-4abd-85ff-d0a203bd44e5){: width="100%" height="100%"}{: .align-center}

- What happen Under the Hood? (1) Unreal loads the Map (2) The Map specifies a GameMode (3) The PlayerController joins the Map (4) It asks the GameMode to spawn a Pawn (5) The Pawn is linked th the PlayerController

### 01-5 How to Be an Active Student
- (1) Lecture Project Changes (2) This Lecture Discussions (3) Course Slides (4) Udemy QnA, Please use brace to attach the code (5) Discord

### 01-6 Surveying the Multiplayer Space

![type](https://user-images.githubusercontent.com/80055816/235854794-0dd6f1de-05ac-467e-853a-d9e216dd3687.PNG){: width="100%" height="100%"}{: .align-center}

- Session-Based Stages (1) Discovery (2) Connection (3) Synchronisation

### 01-7 Meet the Client-Server Model

![input](https://user-images.githubusercontent.com/80055816/235873417-649e869d-5994-4340-8467-69145db13918.PNG){: width="100%" height="100%"}{: .align-center}

![peer](https://user-images.githubusercontent.com/80055816/235873557-a4c6e08f-cfbe-4075-8cd3-caec7d19167e.PNG){: width="100%" height="100%"}{: .align-center}

![server](https://user-images.githubusercontent.com/80055816/235873638-0281ae10-9b0b-4399-8742-870cdde0a6e9.PNG){: width="100%" height="100%"}{: .align-center}

![person](https://user-images.githubusercontent.com/80055816/235876712-ae8d63a9-1885-4956-87fa-39ae518bdd87.PNG){: width="100%" height="100%"}{: .align-center}

![connect](https://user-images.githubusercontent.com/80055816/235873752-383563be-43d6-4c45-bff6-a1f9378a3ac4.PNG){: width="100%" height="100%"}{: .align-center}

![local](https://user-images.githubusercontent.com/80055816/235875974-1a42a4c9-24d4-40fa-b6b7-8227b09771cd.PNG){: width="100%" height="100%"}{: .align-center}

- Connect error solution, 16:23
- 데디케이티드 서버에 대한 정보는 오른쪽 링크를 참고하자 ([**참고**](https://docs.unrealengine.com/4.26/ko/InteractiveExperiences/Networking/Server/)), ([**참고**](https://docs.unrealengine.com/4.27/ko/InteractiveExperiences/Networking/HowTo/DedicatedServers/)), ([**참고**](https://kyoun.tistory.com/97))

### 01-8 Tips For Not Spawning
- Connect error solution

### 01-9 Detecting Where Code is Running

![actor](https://user-images.githubusercontent.com/80055816/236115230-4d9ff59e-0fee-4f16-8d61-06cf373e41b1.PNG){: width="100%" height="100%"}{: .align-center}

![plat](https://user-images.githubusercontent.com/80055816/236115267-4903969e-e424-476b-a373-18cfea25a440.PNG){: width="100%" height="100%"}{: .align-center}

![move](https://user-images.githubusercontent.com/80055816/236115286-4da4a638-0599-4c91-9586-0fe05bc0edb8.PNG){: width="100%" height="100%"}{: .align-center}

![diff](https://user-images.githubusercontent.com/80055816/236115320-7e4d7aac-e9e5-48f1-808e-975dd18da14d.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
AMovingPlatform::AMovingPlatform()
{
	PrimaryActorTick.bCanEverTick = true;

	// Now this actor can move
	// 주의할 점은 생성자에서 무언가를 변경해도
	// 월드에 배치되거나 블루프린트로 생성된 오브젝트는 변경되지 않는다
	SetMobility(EComponentMobility::Movable);
}

void AMovingPlatform::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	// The code that we write runs both on the server and the client in exactly the same way
	// The only difference being that there are some method calls that allow us to detect
	// Whether we are the client or whether we are the server
	// Namely that method call is has authority
	// If server returns true else return false
	if (HasAuthority())
	{
		FVector Location = GetActorLocation();
		Location += FVector(Speed * DeltaTime, 0, 0);
		SetActorLocation(Location);
	}
}
```

### 01-10 Authority and Replication

![replication](https://user-images.githubusercontent.com/80055816/236127710-3917f93c-4a6f-4f72-8eab-2440ec4ea2a6.PNG){: width="100%" height="100%"}{: .align-center}

![replication2](https://user-images.githubusercontent.com/80055816/236127795-4666b395-2894-4a06-87b1-964cfb464b28.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AMovingPlatform::BeginPlay()
{
	Super::BeginPlay();

	if (HasAuthority())
	{
		// 이 액터가 네트워크 클라이언트에 복제되는지 여부를 설정합니다
		SetReplicates(true);
		// 이 액터의 움직임이 네트워크 클라이언트에 복제되는지 여부를 설정합니다
		SetReplicateMovement(true);
	}
}
```

```cpp
void AMovingPlatform::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	//------------------------------------------------------------------------
	// 코드를 만약 이렇게 바꾸면 클라에서만 큐브가 움직인다
	// 하지만 서버가 기준이기 때문에 클라 캐릭터가 큐브가
	// 원래 있던 위치에 박치기 하면 충돌이 일어난다
	// 이 상태에서 만약 서버 캐릭터가 큐브위에 올라서면
	// 클라이언트 큐브가 서버 캐릭터를 옮길것이다 하지만
	// 진실은 서버 캐릭터와 서버 큐브는 안움직이고 있다
	// 본문에서는 이것을 아래와 같이 설명하고 있다
	// Unreal builds basically will move when something is attached to moves
	// Something when it's standing on top of something, then it moves
	//------------------------------------------------------------------------
	if (!HasAuthority())
	{
		FVector Location = GetActorLocation();
		Location += FVector(Speed * DeltaTime, 0, 0);
		SetActorLocation(Location);
	}
}
```

### 01-11 Widgets For FVector Properties

![edit](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/1be85207-afc7-4aab-9a16-94d7d7a8037c){: width="100%" height="100%"}{: .align-center}

```cpp
class PUZZLEPLATFORMS_API AMovingPlatform : public AStaticMeshActor
{
	GENERATED_BODY()

	//..
	
	// 위의 사진 처럼 Target 다이아 몬드를 생성하주려면 어떻게 해야 하는가?
	// MakeEditWidget = true로 세팅해준다
	UPROPERTY(EditAnywhere, Meta = (MakeEditWidget = true))
	FVector TargetLocation;
};
```

### 01-12 Sending The Platform Back

```cpp
if (HasAuthority())
{
	// Target Swap을 아래 코드처럼 거리기반으로 해주는 이유는 무엇인가?
	// Position 기반으로 코드를 작성하면 속도가 너무 빠를때 해당 Position을
	// 지나쳐 무한히 움직일 수 있기 때문이다
	FVector Location = GetActorLocation();
	float JourneyLength = (GlobalTargetLocation - GlobalStartLocation).Size();
	float JourneyTravelled = (Location - GlobalStartLocation).Size();

	if (JourneyTravelled >= JourneyLength)
	{
		// GlobalStartLocation은 Local 좌표인가 World 좌표인가?
		// World 좌표
		FVector Swap = GlobalStartLocation;
		GlobalStartLocation = GlobalTargetLocation;
		GlobalTargetLocation = Swap;
	}

	FVector Direction = (GlobalTargetLocation - GlobalStartLocation).GetSafeNormal();
	Location += Speed * DeltaTime * Direction;
	SetActorLocation(Location);
}
```

### 01-13 Set Up A Simple Puzzle

![open](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/903b178c-e35c-4570-8a4c-39924bf0f0bf){: width="100%" height="100%"}{: .align-center}

![movement](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/ccbb2915-d9ac-4749-b614-93d1126ce4a8){: width="100%" height="100%"}{: .align-center}

### 01-14 Playing Over The Internet

![net](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/4e0d7085-aef2-4cd0-b18f-86eb69317543){: width="100%" height="100%"}{: .align-center}

![hamachi](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/7f0bfe0d-3ebf-48b7-94ee-12008063b3bd){: width="100%" height="100%"}{: .align-center}

- C:\UE_4.17\Engine\Binaries\Win64\UE4Editor.exe D:\Puzzle_Platforms\PuzzlePlatforms\PuzzlePlatforms.uproject 000.000.000.000 -game -log

### 01-15 Set Up A Platform Trigger

![plat](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/74480ed8-0dfa-417a-98c9-1efc086e6efd){: width="100%" height="100%"}{: .align-center}

![bp](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/06b8c425-ebf8-46d8-9ca2-04f32ccbf779){: width="100%" height="100%"}{: .align-center}

![add](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/6cfabb44-b90c-408c-afe1-ca5f4a674219){: width="100%" height="100%"}{: .align-center}

![pad](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/39cd8ee6-6f7f-424a-a39a-340d5a5d2e57){: width="100%" height="100%"}{: .align-center}

```cpp
APlatformTrigger::APlatformTrigger()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

	// TriggerVolume 라는 이름으로 컴포넌트를 생성했다
	TriggerVolume = CreateDefaultSubobject<UBoxComponent>(FName("TriggerVolume"));
	if (!ensure(TriggerVolume != nullptr)) return;

	RootComponent = TriggerVolume;
}
```

### 01-16 Handling Overlap Events in C++

```cpp
// 충돌 이벤트를 생성하기 위한 주요 과정을 설명하면?

// 1. Dynamic 등록
TriggerVolume->OnComponentBeginOverlap.AddDynamic(this, &APlatformTrigger::OnOverlapBegin);
TriggerVolume->OnComponentEndOverlap.AddDynamic(this, &APlatformTrigger::OnOverlapEnd);

// 2. 충돌 함수 정의
void APlatformTrigger::OnOverlapBegin(UPrimitiveComponent* OverlappedComp, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	UE_LOG(LogTemp, Warning, TEXT("Activated"));
}

void APlatformTrigger::OnOverlapEnd(UPrimitiveComponent* OverlappedComp, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)
{
	UE_LOG(LogTemp, Warning, TEXT("Deactivated"));
}
```

![col](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/b268a431-9764-4159-b445-296f8682fe96){: width="100%" height="100%"}{: .align-center}

### 01-17 Activating Platforms From Triggers

![set](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/e78f4808-48ce-4644-bb8e-ad07d4866435){: width="100%" height="100%"}{: .align-center}

![click](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/68264525-64b2-438f-8f3d-a9620902bb0c){: width="100%" height="100%"}{: .align-center}

### 01-18 When To Use A GameInstance

![ins](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/14b332da-ab92-4cf9-92f8-018d46be60a7){: width="100%" height="100%"}{: .align-center}

![setins](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/2946cf93-692e-4b74-b84e-2a9ba2447310){: width="100%" height="100%"}{: .align-center}

![ini](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/f80369a2-8fa2-4ed3-b476-935af1f873dd){: width="100%" height="100%"}{: .align-center}

![window](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/42388599-f22a-45e9-8cfd-25092dd64362){: width="100%" height="100%"}{: .align-center}

```cpp
class PUZZLEPLATFORMS_API UPuzzlePlatformsGameInstance : public UGameInstance
{
	GENERATED_BODY()
	
public:
	UPuzzlePlatformsGameInstance(const FObjectInitializer& ObjectInitializer);

	// 이 함수에 대해 설명하면?
	// UGameInstance에 있는 함수를 오버라이딩한 함수이다
	virtual void Init();
};
```

### 01-19 Console Commands With Exec

![press](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/60418109-6090-4b88-ae37-17afcd6502c9){: width="100%" height="100%"}{: .align-center}

```cpp
class PUZZLEPLATFORMS_API UPuzzlePlatformsGameInstance : public UGameInstance
{
	GENERATED_BODY()
	
public:
	UPuzzlePlatformsGameInstance(const FObjectInitializer& ObjectInitializer);
	virtual void Init();
	
	// 이 함수의 용도는 무엇인가?
	// 콘솔 창에서 실행시킬 명령어를 정의한다
	UFUNCTION(Exec)
	void Host();

	UFUNCTION(Exec)
	void Join(const FString& Address);
};
```

- logging to the screen unreal 라고 구글에 검색하면 디버깅 메시지 출력 가이드를 볼 수 있다

### 01-20 Hosting Servers With ServerTravel

![level](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/9cb29746-e3e0-495c-9e0a-28b051ad5d5b){: width="100%" height="100%"}{: .align-center}

![actor](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/86a90d62-3a0a-45e8-ab9c-56c4a93e85b6){: width="100%" height="100%"}{: .align-center}

- (1) Player controllers will connect in to the map (2) We've got three players all playing around in the same map (3) Server can execute a command called server travel and say, oh, there's another map (4) All to move over their server, travel to map number two, in this case, Africa map (5) All the servers, one by one, have to reconnect to the new map and move themselves into the new map so the controllers don't get destroyed They just get moved into a new map

```cpp
void UPuzzlePlatformsGameInstance::Host()
{
	UEngine* Engine = GetEngine();
	if (!ensure(Engine != nullptr)) return;

	Engine->AddOnScreenDebugMessage(0, 2, FColor::Green, TEXT("Hosting"));

	UWorld* World = GetWorld();
	if (!ensure(World != nullptr)) return;

	// 이 함수의 역할은?
	// 월드 이동
	World->ServerTravel("/Game/ThirdPersonCPP/Maps/ThirdPersonExampleMap?listen"); // ?listen 구문이 없으면 어떻게 될까? 따로 공부하자
}
```

### 01-21 Joining Servers With ClientTravel

![default](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/cea0772b-2c46-4722-872d-eaa06bc384e5){: width="100%" height="100%"}{: .align-center}

- Understand the difference between client travel and server travel, Client travel that operates on a player controller level and it tells the individual player controller that it should move

```cpp
void UPuzzlePlatformsGameInstance::Join(const FString& Address)
{
	UEngine* Engine = GetEngine();
	if (!ensure(Engine != nullptr)) return;

	Engine->AddOnScreenDebugMessage(0, 5, FColor::Green, FString::Printf(TEXT("Joining %s"), *Address));

	APlayerController* PlayerController = GetFirstLocalPlayerController();
	if (!ensure(PlayerController != nullptr)) return;

	// 이 함수의 용도는 무엇인가?
	// Client travel을 하기위한 용도
	PlayerController->ClientTravel(Address, ETravelType::TRAVEL_Absolute);
}
```

### 01-22 Sharing Your Game On itch.io

![pack](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/d43e3d89-8497-4626-90dd-c4ba23c8acb3){: width="100%" height="100%"}{: .align-center}

- https://itch.io/ 이 사이트는 스팀과 달리 무료로 게임을 배포해 볼 수 있다

![itch](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/94ef0d9e-2dcf-4930-b4f1-48e6ac818397){: width="100%" height="100%"}{: .align-center}

![edit](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/5a7c6437-fcd1-4629-9faa-a0a83b54ff57){: width="100%" height="100%"}{: .align-center}

![zip](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/e1c8e214-9bc8-4dbd-a40c-6337f75c46e7){: width="100%" height="100%"}{: .align-center}

![upload](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/d27185d2-0b25-4dba-9673-1895430cf9f6){: width="100%" height="100%"}{: .align-center}

![up](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/fe0f7e9b-5030-4f2b-a41c-da499d42b2d1){: width="100%" height="100%"}{: .align-center}

![view](https://github.com/devMyeong/devMyeong.github.io/assets/80055816/167fb5df-1539-4c09-bf14-7639eb22577d){: width="100%" height="100%"}{: .align-center}

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
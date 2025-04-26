---
title:  "Dedicated Server"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2025-03-09
last_modified_at: 2025-03-09
---

## Chapter 1 Unreal Engine Dedicated Server

### 01-1 Setup Dedicated server and Client builds

![build](https://github.com/user-attachments/assets/b95d041e-af3e-49e0-8bca-0fc0e46dae43){: width="100%" height="100%"}{: .align-center}

![add](https://github.com/user-attachments/assets/1ab8b8f6-0c23-4026-9192-5fc7ae009541){: width="100%" height="100%"}{: .align-center}

- 우리가 만들어준 빌드 타겟을 로드하자

![add](https://github.com/user-attachments/assets/4f1cfd63-7965-4ace-97e3-3314dec5ed0d){: width="100%" height="100%"}{: .align-center}

- 빌드 타겟을 수정해주고 Generate Visual Studio project files 기능으로 프로젝트 파일을 재생성하자
- 이후 Development Editor 모드와 Development Server 모드를 각각 빌드해주자

![package](https://github.com/user-attachments/assets/8ec54a48-741d-4fbf-bd20-53d1a23f2496){: width="100%" height="100%"}{: .align-center}

![client](https://github.com/user-attachments/assets/e5c4e5d3-5fa5-4eba-a099-dcd242e6fecc){: width="100%" height="100%"}{: .align-center}

![server](https://github.com/user-attachments/assets/635ebcfa-30e5-4733-b0df-bb0536d5b655){: width="100%" height="100%"}{: .align-center}

![baro](https://github.com/user-attachments/assets/acae6166-1aac-4ab7-95ba-15a1d4c0e632){: width="100%" height="100%"}{: .align-center}

![connect](https://github.com/user-attachments/assets/ade40d3a-e55c-4597-a5e2-c2577d413b6e){: width="100%" height="100%"}{: .align-center}

### 01-2 Auto Connect to Server

![entrylevel](https://github.com/user-attachments/assets/cb3b75f5-9d68-4f03-b76c-a04c322f6f8a){: width="100%" height="100%"}{: .align-center}

![connect](https://github.com/user-attachments/assets/2b8fc48b-84df-4a59-94bf-bbeb2d1d7e84){: width="100%" height="100%"}{: .align-center}

### 01-3 Compile code out of client build

```cpp
void Akusogaki77_ProjectCharacter::BeginPlay()
{
	// Call the base class  
	Super::BeginPlay();

//-----------------------------------------
// 클라이언트 에서는 아래 로그가 출력되지 않는다
//-----------------------------------------
#if UE_SERVER
	UE_LOG(LogTemp, Warning, TEXT("This is running on the server"));
#endif
}
```

### 01-4 참고한 사이트
- [[**출처**](https://www.youtube.com/watch?v=ad5MZLSDAZk&list=PLnHeglBaPYu8GGRJrRu2nKVnPomyX1Xff)]

<br>

## Chapter 2 Multiplayer in Unreal Engine

### 02-1 How to Understand Network Replication
- 언리얼 엔진은 어떤 개념을 활용해 각 클라이언트의 월드를 동기화 시키는가? Replication ( 00 : 46 )
- NetMode는 무엇의 속성인가? World ( 01 : 28 )
- 게임을 플레이할 수 있고 GameInstance에 LocalPlayer가 있는 NetMode는? NM_Standalone, NM_ListenServer, NM_Client ( 01 : 35 )
- GameInstance에 GameMode 액터가 포함된 World의 표준적이고 신뢰할 수 있는 사본이 있는 NetMode는? NM_Standalone, NM_DedicatedServer, NM_ListenServer ( 01 : 43 )
- 원격 연결 시도에 열려 있는 NetMode는? NM_DedicatedServer, NM_ListenServer ( 01 : 52 )
- 게임 인스턴스가 시작된 방식에 따라 무엇이 달라 지는가? 월드의 NetMode ( 02 : 13 )
- 위에서 말하는 방식 중에서 게임 인스턴스가 원격 서버에 연결된 경우 해당 월드는 어떤 NetMode를 갖는가? NM_Client ( 02 : 18 )
- NM_Client의 특징은? 로컬 플레이어도 플레이 할 수 있지만, 서버의 요청에 따라 월드가 업데이트 된다 ( 02 : 23 )
- 게임 인스턴스가 로컬 맵을 로드하는 경우 어떤 NetMode를 갖는가? NM_Standalone ( 02 : 29 )
- 로컬에서 맵을 로드하고 ?Listen 옵션을 추가하면 어떤 NetMode를 갖는가? NM_ListenServer ( 02 : 43 )
- LocalPlayer도 없고 뷰포트도 없는 NetMode는? NM_DedicatedServer ( 02 : 57 )
- Replication 실현을 위해 필요한 클래스는? NetDriver, NetConnection, Channel ( 03 : 48 )
- 각 프로세스마다 고유의 NetDriver를 갖는가? Yes 이때 서버 프로세스의 NetDriver는 메시지 수신 기능을 담당하고 클라이언트 프로세스의 NetDriver는 서버에 연결 요청을 보냄 ( 04 : 05 )
- 서버와 클라이언트 NetDriver가 접촉하면 어떤일이 일어 나는가? 각각의 NetDriver 내에 NetConnection이 설정된다 이때 서버는 연결된 플레이어의 수만큼 NetConnection을 갖고 각 클라이언트는 서버와의 연결을 나타내는 단일 NetConnection을 갖는다 ( 04 : 17 )
- NetConnection 내에 어떤 채널들이 있는가? UControlChannel, UVoiceChannel, UActorChannel ( 04 : 33 )
- UActorChannel의 특징은? 현재 연결을 통해 Replication 되고 있는 액터 수만큼 생성된다 주의할 점은 클라이언트 수만큼이 아니라 액터 수만큼 이라는 것이다 ( 04 : 41 )

```cpp
// 아래 코드에 대해 설명하면? ( 04 : 58 )
// 복제에 적합한 액터가 특정 플레이어와 관련이 있다고 간주되어
// 서버는 해당 플레이어의 NetConnection 에서 ActorChannel을 열고
// 서버와 클라이언트는 해당 채널을 사용하여 해당 액터에 대한 정보를 교환한다
bReplicates = true;
IsNetRelevantFor(P0) = true;
```

- 액터가 클라이언트에 복제되는 경우 발생할 수 있는 중요한 세가지 사항은? Actor의 수명이 서버와 클라 사이에 동기화 된다, Property 또한 동기화 된다, RPC 또한 동기화 된다 ( 05 : 13 )
- 서버와 액터를 소유한 단일 클라이언트 간에 메시지를 주고 받는 방법은? 클라이언트 또는 서버 RPC를 선언한다 ( 05 : 59 )
- NetConnection은 보통 무엇을 나타내는가? 플레이어를 나타내며 플레이어가 게임에 완전히 로그인하면 해당 플레이어와 연관된 PlayerController 액터를 갖게 된다 ( 06 : 29 )
- 서버 관점에서 소유에 대해 설명하면? UNetConnection은 해당 PlayerController를 소유하며 PlayerController가 소유중인 자식들까지 소유한다 ( 06 : 38 )
- PlayerController가 소유중인 대표적인 자식 클래스들을 말해보면? APlayerState, APawn 이다 이때 APawn이 자신이 소유한 AWeapon을 액터를 생성했다고 가정하면 AWeapon의 부모를 추적해 어떤 클라이언트에 속해있는지 알 수 있다 ( 06 : 53 )
- Relevancy에 대해 설명하면? 서버의 NetDriver는 해당 액터가 각 클라이언트와 Relevancy 즉 관련이 있는지 확인한다 ( 07 : 51 )
- 항상 Relevancy 상태인 액터는? GameState, PlayerState ( 08 : 01 )
- 특정 플레이어가 A 액터를 소유했다면 어떤일이 일어나는가? A 액터는 항상 해당 클라이언트와 Relevancy 하다고 간주된다 ( 08 : 14 )
- PlayerController와 같은 일부 액터의 특징은? 소유자에게만 관련이 있도록 구성 되어있다 때문에 소유하지 않은 클라이언트에게 복제되지 않는다 ( 08 : 24 )
- 소유자로부터 관련성을 상속하도록 Actor를 구성하는것이 가능한가? Yes ( 08 : 33 )
- 소유되지 않은 Actor가 숨겨져 있고 충돌이 비활성화된 경우 어떤일이 일어나는가? Relevancy 하지 않는 것으로 간주된다 ( 08 : 38 )
- 위의 경우에 해당하지 않으면 Relevancy의 기준은 무엇인가? 플레이어와의 거리, IsNetRelevantFor 함수를 사용해 재정의 할 수 있음 ( 08 : 51 )
- 서버가 해당 액터와 관련된 업데이트를 클라이언트에게 동기화 하는 빈도는 어떤 요소에 의해 결정되는가? NetUpdateFrequency와 NetPriority ( 09 : 12 )
- 서버의 NetDriver는 대역폭 완화를 위해 무엇을 하는가? 우선순위에 따라 관련 행위자를 정렬한 다음 사용 가능한 대역폭을 모두 사용할 때까지 네트워크 업데이트를 실행한다 ( 09 : 42 )
- 우선순위의 기준은 무엇인가? 플레이어와 가까울수록, 한동안 업데이트가 되지 않을수록 우선순위가 올라간다 ( 09 : 56 )
- 우선순위 가중치에 해당하는 변수는? NetPriority ( 10 : 07 )
- 주기적이고 대역폭이 제한된 네트워크 업데이트 프로세스는 무엇을 활용하는게 좋은가? Property ( 10 : 20 )
- 네트워크로 즉시 전송하고 싶은 높은 우선순위 메시지가 있다면 무엇을 활용해야 하는가? RPCs ( 10 : 26 )
- 모든 UFUNCTION을 RPC로 만들 수 있는가? Yes ( 10 : 34 )

```cpp
// 서버에서 클라이언트 RPC를 호출하면 어떻게 되는가? 아래 코드처럼 소유 클라이언트 에서 실행된다 ( 10 : 41 )

// GameServer.exe
SomePawn->Client_DoSomething();

// Game.exe
Client_DoSomething_Implementation();

// 클라이언트 에서 서버 RPC를 호출하면 어떻게 되는가? 아래 코드처럼 서버에서 실행된다 ( 10 : 48 )

// Game.exe
SomePawn->Server_DoSomething();

// GameServer.exe
Server_DoSomething_Implementation();

// 서버에서 멀티캐스트 RPC를 호출하면 어떻게 되는가? 아래 코드처럼 서버 및 모든 클라이언트 에서 실행된다 ( 10 : 54 )

// GameServer.exe
SomePawn->Multicast_DoSomething();
Multicast_DoSomething_Implementation();

// Game.exe - P0
Multicast_DoSomething_Implementation();

// Game.exe - P1
Multicast_DoSomething_Implementation();
```

- Multicast RPC 에서 중요한 요소는? Relevancy, 특정 클라이언트는 해당 액터에 대한 오픈 채널이 없을 수 있기 때문이다 ( 11 : 00 )
- 위에서 말한 경우에서 어떤일이 발생 하는가? 해당 Multicast RPC를 특정 클라이언트가 수신하지 못한다 ( 11 : 12 )
- 신뢰할 수 없는 RPC는 어떤 경우에 삭제될 수 있는가? 대역폭이 포화 상태일 때 ( 11 : 21 )
- 신뢰할 수 없는 RPC 에서 가장 유의해야 하는것은? 순서대로 도착할 것도 보장되지 않는다 ( 11 : 31 )
- 신뢰할 수 있는 RPC 함수를 과도하게 사용하면 어떤 문제가 발생 하는가? 대역폭 포화가 심화되고 패킷 손실이 발생할 경우 병목 현상이 발생할 수 있다 ( 11 : 43 )

```cpp
//--------------------------------------------------------------------------------------
// 아래 코드의 기능은? WithValidation 기능을 활용해 신뢰할 수 있는 값인지 검증한다 ( 12 : 09 )
// 참고로 검증에 실패할 경우 해당 클라이언트를 추방한다
//--------------------------------------------------------------------------------------
bool ASomePawn::Server_InitiateAttack_Validate(float ChargeStrength)
{
	return ChargeStrength <= MaxChargeStrength;
}
```

- 소유 연결을 통해 클라이언트 에서 서버로 데이터를 가져오는 유일한 방법은? 서버 RPC ( 12 : 44 )
- RPC는 보통 언제 사용 되는가? 우선 순위가 높고 시간이 중요한 네트워크 코드에 사용된다 예를들어 엔진의 캐릭터 이동 ( 12 : 50 )
- Property에 대한 복제를 활성화 하려면 어떻게 해야 하는가? Replicated 지정자를 추가 해준다 ( 14 : 06 )

```cpp
// 아래 함수의 기능은? 어떤 속성을 어떤 조건에서 복제해야 하는지 지정하는 기능이다 ( 14 : 24 )

void ASomeActor::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);

	DOREPLIFETIME(ASomeActor, SomeProperty);
}
```

- Property 속성이 업데이트 될 때 특정 함수를 실행해야 하는 경우 사용하는 키워드는? ReplicatedUsing ( 14 : 54 )
- ReplicatedUsing을 사용할 때 블루프린트와 CPP의 차이는 무엇인가? 블루프린트는 속성값이 변하면 서버에서 연관된 함수가 자동으로 호출된다 하지만 CPP 에서는 클라이언트 뿐만 아니라 서버에서도 연관된 함수가 호출되려면 HasAuthority를 활용해 수동으로 호출해야 한다 ( 15 : 10 )
- ActorRole의 종류는? ROLE_None, ROLE_SimulatedProxy, ROLE_AutonomousProxy, ROLE_Authority ( 15 : 28 )
- Actor가 HasAuthority 하지 않으면 보통 어떤 Role 상태일까? SimulatedProxy ( 16 : 11 )
- PlayerController는 소유 클라이언트에 리플리케이티드 되며 어떤 Role을 담당하는가? ROLE_AutonomousProxy 참고로 서버는 PlayerController에 대해 Authority 하다 ( 16 : 18 )
- PlayerController에 연관된 Pawn의 Role은? ROLE_AutonomousProxy, 연관 되지 않은 Pawn 들은 ROLE_SimulatedProxy ( 16 : 23 )
- Pawn이 IsLocalController에 대해 참이면 해당 플레이어는? 코드가 실행되는 GameInstance에 해당한다 ( 16 : 53 )
- Pawn이 IsLocalController에 대해 거짓이면 해당 플레이어는? 원격 클라이언트의 플레이어에 해당한다 ( 16 : 59 )
- 리플리케이티드 되고있는 Actor의 모든 부분이 네트워킹과 관련되어야 하는가? No ( 18 : 15 )
- 네트워크를 인식하지 못하는 부분들은 어떻게 동기화 하면 좋을까? 함수화 시켜놓고 리플리케이션을 적용한다 ( 18 : 22 )

```cpp
// 각 액터들의 특징을 말해보면? ( 18 : 51 )
// dev.epicgames.com/documentation/ko-kr/unreal-engine/remote-procedure-calls-in-unreal-engine
// 액터 소유권이 어느쪽에 있는지 명확하게 구분하자
// 서버에서 호출된 RPC 인지 클라이언트에서 호출된 RPC 인지 명확하게 구분하자

AGameModeBase - server only
AGameStateBase - replicated to all clients
AGameSession - server only

APlayerController - replicated to owning client
APlayerState - replicated to all clients
APawn - replicated to relevant clients

AActor - you decide
```

- 권한이 있어야만 호출되는 함수를 어떻게 체크할 수 있을까? checkf 함수를 활용한다 ( 19 : 04 )
- 프로세스가 클라이언트에서 시작되어 서버에서 종료되는 경우 보통 어떤 RPC를 사용하는가? 서버 RPC ( 19 : 37 )
- 프로세스가 서버에서 시작되어 결국 클라이언트에 영향을 미치는 경우 보통 어떤 RPC를 사용하는가?  ( 19 : 44 )

```cpp
// 멀티플레이와 싱글플레이에서 서버에서만 발생해야 하는 일이 있다면? ( 20 : 24 )
if (HasAuthority()) {}

// 클라이언트로 실행할 때만 필요하고 싱글플레이에서 조차 실행 안되게 하려면? ( 20 : 32 )
if (!HasAuthority()) {}

// 데디케이티드 서버를 특별히 제외 하려는 경우는? ( 20 : 39 )
if (!IsRunningDedicatedServer()) {}

// 순전히 클라이언트 측 기능이 필요한 경우는? ( 20 : 47 )
if (Pawn->IsLocalController()) {}
if (Controller->IsLocalController()) {}
```

- 더 세부적인 사항은? ( 21 : 07 )

- [[Networking and Multiplayer](https://dev.epicgames.com/documentation/en-us/unreal-engine/networking-and-multiplayer-in-unreal-engine)]

### 02-2 참고한 사이트
- [[**출처**](https://www.youtube.com/watch?v=JOJP0CvpB8w&list=WL&index=46&t=44s)]

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
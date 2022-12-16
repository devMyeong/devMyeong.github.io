---
title:  "Unreal Engine C++ The Ultimate Shooter Course"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2022-12-13
---

## Chapter 1 Introduction

### 01-1 Introduction
- C++ tutorial series on the Internet, and no other course will you cover such a wide variety of unreal

### 01-2 Install an IDE
- Visual studio installer에서 C++을 사용한 게임 개발 체크하자

![config](https://user-images.githubusercontent.com/80055816/207264533-ed045cee-689f-47dd-acf3-04d52c598713.PNG){: width="100%" height="100%"}{: .align-center}

![config2](https://user-images.githubusercontent.com/80055816/207264842-9f56de74-8a18-42fa-a1e7-17db41b57371.PNG){: width="100%" height="100%"}{: .align-center}

![config3](https://user-images.githubusercontent.com/80055816/207264883-e4205773-3ba8-47bb-8fca-4dcba6975ce0.PNG){: width="100%" height="100%"}{: .align-center}

![config4](https://user-images.githubusercontent.com/80055816/207264946-8aeeaabc-5057-4610-858b-5a53e23563bc.PNG){: width="100%" height="100%"}{: .align-center}

### 01-3 Install Unreal Engine

![showcase](https://user-images.githubusercontent.com/80055816/207268243-b690d3cb-62a1-430c-94ba-d416bf0c5cfd.PNG){: width="100%" height="100%"}{: .align-center}

- 맨 위에서 부터 내 언리얼 엔진, 내 프로젝트, 내 에셋 순으로 나열되어 있다

### 01-4 C++ Refresher and UE4 Hierarchy
- 언리얼 계층 구조에 대해 나열하면? Object - Actor - Pawn - Character
- Object에 대해 설명하면? An object is a very basic type of class, it can store data, but it does not have the capability of being placed in a level
- Actor에 대해 설명하면? An actor can have a visual representation
- Pawn에 대해 설명하면? An pawn can be possessed by a controller, a controller is a special type of actor class that allows you to take user input, such as a keyboard
- Character에 대해 설명하면? A character has its own character, movement component, and this character movement component has functionality
- 언리얼 HAS A 관계에 대해 나열하면? Package - World - Level - Actor - Actor Component

### 01-5 Reflection and Garbage Collection
- 리플렉션(Reflection)에 대해 설명하면? 프로그램이 런타임에 자기 자신을 조사하는 기능입니다 가비지 콜렉션, 네트워크 리플리케이션, 블루프린트/C++ 커뮤니케이션, 에디터의 디테일 패널, 시리얼라이제이션 등 다수의 시스템에 탑재되어 있다
- Now, as soon as that pointer variable goes out of scope, the pointer gets deleted
- Unreal Engines Garbage collection system keeps track of how many variables reference any given object
- For a class to participate in Unreal Engines garbage collection system, it must make use of special macros that allow the class to be recognized ([**참고**](https://www.unrealengine.com/ko/blog/unreal-property-system-reflection))

### 01-6 How to Get Help
- ([**참고**](https://forums.unrealengine.com/categories?tag=unreal-engine)) 여기서 언리얼에 대한 질문을 할 수 있다
- 강의 영상중 특정 부분이 컴파일이 안되거나 하는 문제가 생기면 해당 영상의 질의응답 탭을 확인하자, 이 탭에서 내 질문에 대한 답변을 찾을수 없다면 직접 질문을 올릴수도 있다
- 질문 올릴때 유의점 1, Tag this video when it posts your questions so I'll know which video you're talking about
- 질문 올릴때 유의점 2, Mentioned that point in time in the video, in your question
- 질문 올릴때 유의점 3, If your question involves code, it's really helpful if you show me your code
- 질문 올릴때 유의점 4, If you have a compiler error, take a screenshot of the compiler error and include that in your question
- 질문 올릴때 유의점 5, If you have a piece of code that you don't understand, highlight that particular piece of code
- 강사님이 운영하는 디스코드 방에서 질문이나 의견등을 자유롭게 공유할 수 있으며, 협업할 멤버도 구할 수 있다
- 이 단원 자료에 이 강좌에 필요한 리소스, 디스코드 주소 등이 기재되어 있다

## Chapter 2 Project Setup

### 02-7 Project Setup

![set](https://user-images.githubusercontent.com/80055816/207659317-3c933a2d-adbd-4ee1-919d-796bbfde7b15.PNG){: width="100%" height="100%"}{: .align-center}

- 기본 프로젝트 세팅은 위와 같이 하면 된다

![projectmap](https://user-images.githubusercontent.com/80055816/207659560-2bb05525-7699-4982-8840-8594edeedaac.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 화면에 대한 설명은 ([**참고**](https://lifeisforu.tistory.com/326)) 여기에서 확인하면 된다
- It's called shooter game mode base, the class name is called a shooter game mode base because this game mode inherits ultimately from actor in the inheritance hierarchy
- So we have this shooter game mode based class And since we have access to it in C++, we can create variables and functions and things here if we want to, in the Unreal Engine, Ed, we can create a blueprint based on this class and we're going to stick it in the game mode folder
- 월드 세팅에 대한 설명은 ([**참고**](https://velog.io/@jijang/%EC%9B%94%EB%93%9C-%EC%84%B8%ED%8C%85)) 여기에서 확인하면 된다

![projectmode](https://user-images.githubusercontent.com/80055816/207664691-17660953-3654-4c54-815d-dbebb4e7a575.PNG){: width="100%" height="100%"}{: .align-center}

- Right here for the default game mode, we can set this to shooter game mode based BP and that will ensure that our shooter game mode based BP will be used for our entire project

### 02-8 Character Class

![bp](https://user-images.githubusercontent.com/80055816/207807937-9ce1f1ef-a762-4260-aaeb-dd063e9ce5d3.PNG){: width="100%" height="100%"}{: .align-center}

- The capsule component is the root component You cannot assign any other component to be the root component So that's something to keep in mind when it comes to the character class
- We also have an arrow component, which is pretty handy to show us the forward direction for our character
- And we also have a mesh This is inherited as well, and it by default has nothing assigned to the skeletal mesh
- And we also have the character movement component and this has a whole bunch of things that we can edit from here in the details panel

![default](https://user-images.githubusercontent.com/80055816/207808796-77fe4ad0-7a46-471a-b9c7-f02ec287427c.PNG){: width="100%" height="100%"}{: .align-center}

- What is the Default Pawn class and what is used for in Unreal Engine? ([**참고**](https://gamedev.stackexchange.com/questions/141647/what-is-the-default-pawn-class-and-what-is-used-for-in-unreal-engine))

### 02-9 UE_LOG Format String - Int

```cpp
// UE_LOG 사용법 소개
void AShooterCharacter::BeginPlay()
{
	Super::BeginPlay();
	
	UE_LOG(LogTemp, Warning, TEXT("BeginPlay() called!"));

	// 유니폼 초기화
	int myInt{ 42 };
	UE_LOG(LogTemp, Warning, TEXT("int myInt : %d"), myInt);
}
```

### 02-10 UE_LOG Format Specifiers

```cpp
// UE_LOG 사용법 소개 2
void AShooterCharacter::BeginPlay()
{
	Super::BeginPlay();
	
	UE_LOG(LogTemp, Warning, TEXT("BeginPlay() called!"));

	// 유니폼 초기화
	int myInt{ 42 };
	UE_LOG(LogTemp, Warning, TEXT("int myInt : %d"), myInt);

	float myFloat{ 3.141592f };
	UE_LOG(LogTemp, Warning, TEXT("int myFloat : %f"), myFloat);

	double myDouble{ 0.000756 };
	UE_LOG(LogTemp, Warning, TEXT("double myDouble : %lf"), myDouble);

	char myChar{ 'J' };
	UE_LOG(LogTemp, Warning, TEXT("char myChar : %c"), myChar);

	// 문자 앞의 대문자 L은 컴파일러에게 이 문자열을 와이드 문자로 저장하라고 지시하는 것이다
	wchar_t wideChar{ L'J' };
	UE_LOG(LogTemp, Warning, TEXT("wchar_t wideChar : %lc"), wideChar);

	bool myBool{ true };
	UE_LOG(LogTemp, Warning, TEXT("bool myBool : %d"), myBool);

	UE_LOG(LogTemp, Warning, TEXT("int : %d, float : %f, bool : %d"), myInt, myFloat, myBool);
}
```

### 02-11 UE_LOG with FString

```cpp
// UE_LOG 사용법 소개 3
void AShooterCharacter::BeginPlay()
{
	Super::BeginPlay();
	
	UE_LOG(LogTemp, Warning, TEXT("BeginPlay() called!"));

	// 유니폼 초기화
	int myInt{ 42 };
	UE_LOG(LogTemp, Warning, TEXT("int myInt : %d"), myInt);

	float myFloat{ 3.141592f };
	UE_LOG(LogTemp, Warning, TEXT("int myFloat : %f"), myFloat);

	double myDouble{ 0.000756 };
	UE_LOG(LogTemp, Warning, TEXT("double myDouble : %lf"), myDouble);

	char myChar{ 'J' };
	UE_LOG(LogTemp, Warning, TEXT("char myChar : %c"), myChar);

	// 문자 앞의 대문자 L은 컴파일러에게 이 문자열을 와이드 문자로 저장하라고 지시하는 것이다
	wchar_t wideChar{ L'J' };
	UE_LOG(LogTemp, Warning, TEXT("wchar_t wideChar : %lc"), wideChar);

	bool myBool{ true };
	UE_LOG(LogTemp, Warning, TEXT("bool myBool : %d"), myBool);

	UE_LOG(LogTemp, Warning, TEXT("int : %d, float : %f, bool : %d"), myInt, myFloat, myBool);

	//----------------------------------------------------------------------------
	// This looks like a pointer dereference referencing, but it's not a pointer,
	// we're actually using this asterisk overload on the string
	// And what this returns is you can see a constant char array
	//----------------------------------------------------------------------------
	FString myString{ TEXT("My String!!!!") };
	UE_LOG(LogTemp, Warning, TEXT("FString myString : %s"), *myString);

	//----------------------------------------------------------------------------
	// So that's why I kept the asterisk here, because the asterisk is going to
	// get this F string and return a constant T char
	//----------------------------------------------------------------------------
	UE_LOG(LogTemp, Warning, TEXT("Name of instance : %s"), *GetName());
}
```

### 02-12 Camera Spring Arm
- Chpater 1-6에 소개된 깃허브 저장소에서 각 챕터별 소스를 다운받을 수 있다
- UPROPERTY에 대해 설명하면? UPROPERTY의 역할은 언리얼 리플렉션 시스템에 해당 프로퍼티가 있음을 알리는 것입니다 ([**참고**](https://lifeisforu.tistory.com/326))

```cpp
private:
	// Camera boom positioning the camera behind the character
	// Let's give it a category of camera, this will give it its own category in the details panel
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = Camera, meta = (AllowPrivateAccess = "true"))
	class USpringArmComponent* CameraBoom;
```

- 특정 컴포넌트의 헤더파일 이름은 ([**참고**](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/GameFramework/USpringArmComponent/)) 여기에서 확인 가능하다

```cpp
AShooterCharacter::AShooterCharacter()
{
	// Set this character to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

	// Create a camera boom (pulls in towards the character if there is a collision)
	CameraBoom = CreateDefaultSubobject<USpringArmComponent>(TEXT("CameraBoom"));
	CameraBoom->SetupAttachment(RootComponent);
	CameraBoom->TargetArmLength = 300.f; // The camera follows at this distance behind the character
	CameraBoom->bUsePawnControlRotation = true; // Rotate the arm based on the controller
}
```

![boom](https://user-images.githubusercontent.com/80055816/207856156-e0d3c649-5e0a-4777-a7bb-d86411c79da7.PNG){: width="100%" height="100%"}{: .align-center}

- The red line is our camera boom and our camera is going to be attached to the end of it

### 02-13 Follow Camera

```cpp
// Create a follow camera
FollowCamera = CreateDefaultSubobject<UCameraComponent>(TEXT("FollowCamera"));

//----------------------------------------------------------------------------------------
// We're going to specify the socket on the camera boom, a spring arm component contains
// A socket at the end of it that we can attach the camera to and springform component
// Has a static variable that contains the name of that socket it's called socket name
//----------------------------------------------------------------------------------------
FollowCamera->SetupAttachment(CameraBoom, USpringArmComponent::SocketName); // Attach camera to end of boom
FollowCamera->bUsePawnControlRotation = false; // Camera does not rotate relative to arm
```

![boom](https://user-images.githubusercontent.com/80055816/207886347-96dc5a82-80de-4457-b32a-5306b5e902df.PNG){: width="100%" height="100%"}{: .align-center}

- It's important to remember that if we ever want to adjust the camera relative to the player, we should adjust the camera boom
- If we select the camera boom and we change the target arm length

### 02-14 Controllers and Input
- The controller is also invisible you can think of it as this invisible entity following the character around, but it doesn't have its own scale or location it's just a C++ class object associated with the character
- The controller doesn't have its own location per say, it does have an orientation, that is to say a rotation, and there are functions to get and set the controller rotation
- User의 역할에 대해 설명하면? The user provides input to the game via devices like the mouse and the keyboard
- Controller의 역할에 대해 설명하면? The controller takes user input data and uses that data to move the character, but first it passes it over to the movement component
- Movement Component에 대해 설명하면? The movement component takes input data and performs calculations and makes sure that before the character is moved, it's moved in the right ways it has to obey laws such as gravity but once those calculations are performed, then the movement component can move the character

### 02-15 Move Forward and Right
- Project Setting의 Input탭에서 Action Mapping과 Axis Mapping의 차이는? Action mappings are for one off key presses Axis mappings happen every frame, so they're continuously receiving data about those key presses, each frame of the game
- The controller is what possesses the pawn and the controller is facing a particular direction
- Yaw, Pitch, Roll에 대해서 각각 설명하면? Yaw는 Z축(수직축)을 중심으로 회전하는 것, Pitch는 Y축(횡축)을 중심으로 회전하는 것, Roll은 X축(종축)을 중심으로 회전하는 것 ([**참고**](https://happy8earth.tistory.com/492))

```cpp
// MoveForward를 구현하면?
void AShooterCharacter::MoveForward(float Value)
{
	if ((Controller != nullptr) && (Value != 0.0f))
	{
		// find out which way is forward
		const FRotator Rotation{ Controller->GetControlRotation() };
		const FRotator YawRotation{ 0, Rotation.Yaw, 0 };

		// 회전 행렬에서 방향벡터를 뽑아낸다
		const FVector Direction{ FRotationMatrix{YawRotation}.GetUnitAxis(EAxis::X) };

		// Add movement input along the given world direction vector(usually normalized) scaled by "ScaleValue"
		AddMovementInput(Direction, Value);
	}
}
```

```cpp
void AShooterCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	check(PlayerInputComponent);

	// MoveForward와 바인딩 되어 있는 부분을 설명하면? Project Setting의 Input탭에 등록되어 있는 Value
	PlayerInputComponent->BindAxis("MoveForward", this, &AShooterCharacter::MoveForward);
	PlayerInputComponent->BindAxis("MoveRight", this, &AShooterCharacter::MoveRight);
}
```

### 02-16 Delta Time
- What you should do instead is take your desired movement rate and multiply it by Delta time this will scale the movement rate by exactly the amount that you need to move your actor by the desired

### 02-17 Turn at Rate

```cpp
void AShooterCharacter::TurnAtRate(float Rate)
{
	// calculate delta for this frame from the rate information
	// AddControllerYawInput에 대해 설명하면? AddControllerYawInput은 APawn의 멤버함수로써,
	// Value값이 들어오면 Yaw를 컨트롤 할 수 있게끔한다
	AddControllerYawInput(Rate * BaseTurnRate * GetWorld()->GetDeltaSeconds()); // deg/sec * sec/frame
}
```

- BindAxis() 함수에 대해 설명하면? 축 매핑에 처리 함수를 바인딩하는 함수다 ([**참고**](https://wergia.tistory.com/127))

### 02-18 Mouse Turning and Jumping
- BindAction() 함수에 대해 설명하면? 액션 매핑에 처리 함수를 바인딩하는 함수다 ([**참고**](https://wergia.tistory.com/127))

### 02-19 Adding a Mesh
- 

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
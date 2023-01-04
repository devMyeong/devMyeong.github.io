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
last_modified_at: 2023-01-04
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
- Actor에 대해 설명하면? An actor can have a visual representation such as a 3D mesh
- Pawn에 대해 설명하면? An pawn is an actor, but it has additional functionality, such as the capability of being possessed by a controller ( A controller is a special type of actor class that allows you to take user input, such as a keyboard )
- Character에 대해 설명하면? A character has its own character movement component, and this character movement component has functionality that's appropriate for a character Things like jumping, swimming, flying or whatever else characters typically do are characteristics of the character movement component
- 언리얼 HAS A 관계에 대해 나열하면? Package - World - Level - Actor - Actor Component

### 01-5 Reflection and Garbage Collection
- 리플렉션(Reflection)에 대해 설명하면? 프로그램이 런타임에 자기 자신을 조사하는 기능입니다 가비지 콜렉션, 네트워크 리플리케이션, 블루프린트/C++ 커뮤니케이션, 에디터의 디테일 패널, 시리얼라이제이션 등 다수의 시스템에 탑재되어 있습니다 ([**참고**](https://mm5-gnap.tistory.com/356))
- 리플렉션(Reflection)과 C++ RTTI의 차이는? 리플렉션은 런타임에 객체 정보를 확인할 수 있고 RTTI는 클래스의 타입만 확인할 수 있다 ([**참고**](https://mm5-gnap.tistory.com/356))
- 리플렉션(Reflection)의 사용예시를 설명하면? 멤버 변수 앞에 UPROPERTY 매크로를 붙여 언리얼 리플렉션 시스템에 등록할 수 있다 ([**참고**](https://mm5-gnap.tistory.com/356))
- Garbage collection is when a program manages memory associated with the project and automatically deletes as soon as that pointer variable goes out of scope, the pointer gets deleted
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

<br>

## Chapter 2 Project Setup

### 02-7 Project Setup

![set](https://user-images.githubusercontent.com/80055816/207659317-3c933a2d-adbd-4ee1-919d-796bbfde7b15.PNG){: width="100%" height="100%"}{: .align-center}

- 기본 프로젝트 세팅은 위와 같이 하면 된다

![projectmap](https://user-images.githubusercontent.com/80055816/207659560-2bb05525-7699-4982-8840-8594edeedaac.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 화면에 대한 설명은 ([**참고**](https://lifeisforu.tistory.com/326)) 여기에서 확인하면 된다
- It's called ShooterGameModeBase(C++ File), the class name is called AShooterGameModeBase because this game mode inherits ultimately from actor in the inheritance hierarchy
- So we have this shooter game mode based class And since we have access to it in C++, we can create variables and functions and things here if we want to, in the Unreal Engine, Ed, we can create a blueprint based on this class and we're going to stick it in the game mode folder
- 월드 세팅 탭에서는 무엇을 할 수 있는지 설명하면? 기본 게임 모드나 중력같은 레벨 전용 세팅을 설정할 수 있다 ([**참고**](https://docs.unrealengine.com/4.26/ko/Basics/Levels/WorldSettings/)), ([**참고**](https://velog.io/@jijang/%EC%9B%94%EB%93%9C-%EC%84%B8%ED%8C%85))

![projectmode](https://user-images.githubusercontent.com/80055816/207664691-17660953-3654-4c54-815d-dbebb4e7a575.PNG){: width="100%" height="100%"}{: .align-center}

- Right here for the default game mode, we can set this to shooter game mode based BP and that will ensure that our shooter game mode based BP will be used for our entire project

### 02-8 Character Class

![bp](https://user-images.githubusercontent.com/80055816/207807937-9ce1f1ef-a762-4260-aaeb-dd063e9ce5d3.PNG){: width="100%" height="100%"}{: .align-center}

- The capsule component is the root component you cannot assign any other component to be the root component so that's something to keep in mind when it comes to the character class
- We also have an arrow component, which is pretty handy to show us the forward direction for our character
- And we also have a mesh This is inherited as well, and it by default has nothing assigned to the skeletal mesh
- And we also have the character movement component and this has a whole bunch of things that we can edit from here in the details panel

![default](https://user-images.githubusercontent.com/80055816/207808796-77fe4ad0-7a46-471a-b9c7-f02ec287427c.PNG){: width="100%" height="100%"}{: .align-center}

- What is the Default Pawn class? ([**참고**](https://gamedev.stackexchange.com/questions/141647/what-is-the-default-pawn-class-and-what-is-used-for-in-unreal-engine))

### 02-9 UE_LOG Format String - Int

```cpp
// UE_LOG 사용법 소개
void AShooterCharacter::BeginPlay()
{
	Super::BeginPlay();
	
	UE_LOG(LogTemp, Warning, TEXT("BeginPlay() called!"));

	// 유니폼 초기화, 유니폼 초기화를 하면 int 값에 42.54와 같이 double형을 입력하려 할 때 컴파일 에러를 띄워준다
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

	// 유니폼 초기화, 유니폼 초기화를 하면 int 값에 42.54와 같이 double형을 입력하려 할 때 컴파일 에러를 띄워준다
	int myInt{ 42 };
	UE_LOG(LogTemp, Warning, TEXT("int myInt : %d"), myInt);

	float myFloat{ 3.141592f };
	UE_LOG(LogTemp, Warning, TEXT("int myFloat : %f"), myFloat);

	double myDouble{ 0.000756 };
	UE_LOG(LogTemp, Warning, TEXT("double myDouble : %lf"), myDouble);

	char myChar{ 'J' };
	UE_LOG(LogTemp, Warning, TEXT("char myChar : %c"), myChar);

	// 문자 앞의 대문자 L은 컴파일러에게 이 문자를 와이드 문자로 인식하라고 지시하는 것이다
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

	// 유니폼 초기화, 유니폼 초기화를 하면 int 값에 42.54와 같이 double형을 입력하려 할 때 컴파일 에러를 띄워준다
	int myInt{ 42 };
	UE_LOG(LogTemp, Warning, TEXT("int myInt : %d"), myInt);

	float myFloat{ 3.141592f };
	UE_LOG(LogTemp, Warning, TEXT("int myFloat : %f"), myFloat);

	double myDouble{ 0.000756 };
	UE_LOG(LogTemp, Warning, TEXT("double myDouble : %lf"), myDouble);

	char myChar{ 'J' };
	UE_LOG(LogTemp, Warning, TEXT("char myChar : %c"), myChar);

	// 문자 앞의 대문자 L은 컴파일러에게 이 문자를 와이드 문자로 인식하라고 지시하는 것이다
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
		// Direction is pointing in the direction that corresponds to our controllers orientation
		const FVector Direction{ FRotationMatrix{YawRotation}.GetUnitAxis(EAxis::X) };

		// Add movement input along the given world direction vector(usually normalized) scaled by "Value"
		AddMovementInput(Direction, Value);
	}
}
```

```cpp
// SetupPlayerInputComponent() has access to something called the player input component
void AShooterCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	// PlayerInputComponent is what we use to bind functions to axis
	Super::SetupPlayerInputComponent(PlayerInputComponent);

	// Now we can perform a check to make sure that the input component is valid by using the checked macro
	// This performs an assert that will actually halt execution if the PlayerInputComponent is not valid
	check(PlayerInputComponent);

	// BindAxis() 함수에 대해 설명하면? 축 매핑에 처리 함수를 바인딩하는 함수다
	// MoveForward와 바인딩 되어 있는 부분을 설명하면? Project Setting의 Input탭에 등록되어 있는 Value
	PlayerInputComponent->BindAxis("MoveForward", this, &AShooterCharacter::MoveForward);
	PlayerInputComponent->BindAxis("MoveRight", this, &AShooterCharacter::MoveRight);
}
```

### 02-16 Delta Time
- What is Delta time? the time between frames
- What is Frame(Tick)? a single image updated to the screen
- What is Frame rate? the number of frames updated per second we often hear the term FPS
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

![compile](https://user-images.githubusercontent.com/80055816/208701914-ba7ca635-fb62-4566-b0ba-7e95d57a410e.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 이미지 처럼 Compile 버튼을 눌렀을 때 우측 하단에 C++ Compile Failed!가 뜨면 Show Log 버튼이 생기는데 해당 버튼을 누르면 컴파일 에러 원인을 확인할 수 있다

### 02-18 Mouse Turning and Jumping
- 마우스로 시야를 조정할 때 상하 개념을 전환 하라면 어떻게 하면 되는가? Project Setting -> Input -> Bindings -> Axis Mappings에서 해당 Scale 값을 양수 또는 음수로 변환한다
- BindAction() 함수에 대해 설명하면? 액션 매핑에 처리 함수를 바인딩하는 함수다 ([**참고**](https://wergia.tistory.com/127))

### 02-19 Adding a Mesh
- 언리얼 런처에서 내 프로젝트에 투입할 리소스를 선택하는 섹션은 무엇인가? Library 탭의 VAULT 섹션
- Skeletal Mesh가 바라보는 방향을 바꾸려면 어떻게 하면 되는가? 블루프린트의 Viewport 탭에서 해당 Mesh를 클릭한후 E 키를 눌러 원하는 방향으로 회전시키면 된다
- CapsulComponent와 Mesh의 높이를 어떻게 맞출수 있는가? CapsulComponent의 Details 탭에서 Capsule Half Height를 확인한 후 Mesh의 Details 패널에서 Transform Location Z에 해당 값을 음수로 전환해 기입한다

<br>

## Chapter 3 Animations

### 03-20 The AnimInstance
- The animation blueprint is based on a C++ class called something so what is something? Anime Instance

```cpp
UCLASS()
class SHOOTER_API UShooterAnimInstance : public UAnimInstance
{
	//..

	UFUNCTION(BlueprintCallable)

	// This function will behave much like our tick function
	// We can call this from the event graph in the animation blueprint thanks to UFUNCTION(BlueprintCallable)
	void UpdateAnimationProperties(float DeltaTime);

	// Now, this function is kind of like begin play for actors only, it's for the instant class
	// This is where we can initialize our member variables
	virtual void NativeInitializeAnimation() override;

	//..
}
```

- UFUNCTION()에 대해 설명하면? UPROPERTY는 C++의 변수를 블루프린트와 연동한다 UFUNCTION은 C++의 함수를 블루프린트와 연동한다 ([**참고**](https://velog.io/@ezhun/UFUNCTION-%EB%A7%A4%ED%81%AC%EB%A1%9C))

```cpp
void UShooterAnimInstance::UpdateAnimationProperties(float DeltaTime)
{
	if (ShooterCharacter == nullptr)
	{
		// Character 클래스의 포인터에 어떤 클래스를 형변환 해서 대입해야 할까? Pawn 클래스
		// 현재 AnimInstance를 소유중인 Pawn을 가져오는 것을 시도함
		ShooterCharacter = Cast<AShooterCharacter>(TryGetPawnOwner());
	}
	if (ShooterCharacter)
	{
		// Get the lateral speed of the character from velocity
		FVector Velocity{ ShooterCharacter->GetVelocity() };

		// This speed value does not take the Z component of the velocity into a count
		// So if the character is flying upward or falling downward, that will not affect this speed variable
		Velocity.Z = 0;
		Speed = Velocity.Size();

		//..
	}

	//..
}
```

- CharacterMovementComponent는 어떤 함수를 사용해 불러올 수 있는가? GetCharacterMovement() 함수

### 03-21 Animation Blueprint

![bp](https://user-images.githubusercontent.com/80055816/208726268-185ecc97-fa9f-463a-9d78-64a10c0dd4ca.PNG){: width="100%" height="100%"}{: .align-center}

- We have a ShooterAnimBP, we can assign this to the animation in our shooter character blueprint
- Something is where we can have blueprint logic so what is something? EventGraph
- Something is where we can have animation logic and state machines so what is somthing? AnimGraph
- EventGraph 탭의 Event Blueprint Update Animation 노드에 대해 설명하면? This is kind of like the tick function
- How we can pass Delta Time to UpdateAnimationProperties() function? because we have Event Blueprint Update Animation

### 03-22 Run Animation
![motion](https://user-images.githubusercontent.com/80055816/208732533-480b44f6-44e7-4326-8770-c4bdd5bca426.PNG){: width="100%" height="100%"}{: .align-center}
- Every frame, whatever is returned by the state machine will be fed into the Output Pose which will ultimately drive the animation
- StateMachine에서 Automatic Rule Based on Sequence Player in State 옵션의 의미는? 애니메이션이 종료되었을때 바로 Transition이 되도록 설정하는 것 ([**참고**](https://gosnem93.tistory.com/12))

### 03-23 Trimming Animations
- We created copies so we didn't trim the originals and we tweaked those until they looked good for our game so that it looks a little more natural when she starts running and when she stops running

### 03-24 Rotate Character to Movement

```cpp
AShooterCharacter::AShooterCharacter() :
	BaseTurnRate(45.f),
	BaseLookUpRate(45.f)
{
	//..

	// Don't rotate when the controller rotates
	// Let the controller only affect the camera
	bUseControllerRotationPitch = false;
	bUseControllerRotationYaw = false;
	bUseControllerRotationRoll = false;

	//--------------------------------
	// Configure character movement
	//--------------------------------
	// Character moves in the direction of input...
	GetCharacterMovement()->bOrientRotationToMovement = true;
	// RotationRate의 y값이 올라가면 어떻게 되는가? 빠르게 회전한다
	GetCharacterMovement()->RotationRate = FRotator(0.f, 540.f, 0.f); // ... at this rotation rate
	GetCharacterMovement()->JumpZVelocity = 600.f;
	// AirControl의 역할은 무엇인가? How much control we can have while in the air
	GetCharacterMovement()->AirControl = 0.2f;
}
```
- bOrientRotationToMovement에 대해 설명하면? 현재 캐릭터가 가속값을 가지고 있다면 현재 가속되고있는값 방향으로 캐릭터 메쉬를 회전시켜줍니다 ([**참고**](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=khy19702&logNo=221451673504)) 

![problem](https://user-images.githubusercontent.com/80055816/209083505-252b6fd9-facf-464a-89ad-2d39f7d2f99a.png){: width="100%" height="100%"}{: .align-center}

- Now, the value here in the blueprint will override the value that you set in the constructor so you need to make sure to change it here as well

### 03-25 Firing the Weapon
- BindAction() 함수에 대해 설명하면? 액션 매핑에 처리 함수를 바인딩하는 함수다 ([**참고**](https://wergia.tistory.com/127))

### 03-26 Shooting Sound Effects

![cue](https://user-images.githubusercontent.com/80055816/209095683-7beea9a1-6463-4fff-b0ad-940275c9cb77.PNG){: width="100%" height="100%"}{: .align-center}

- What is Sound Cue Editor? setting up and editing SoundCue node-based audio assets

![move](https://user-images.githubusercontent.com/80055816/209097652-2ecf860d-73d0-452a-a2ca-c97a654a1b90.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/209101648-ab383b09-3f3a-445f-a835-df6c20462c82.PNG){: width="100%" height="100%"}{: .align-center}

### 03-27 Shooting Particles
- Explain the relationship between a skeletal mesh and a skeleton? a skeleton is applied to a skeletal mesh for animation ([**참고**](https://forums.unrealengine.com/t/skeletal-mesh-vs-skeleton/504342))

![particle](https://user-images.githubusercontent.com/80055816/209128838-bb3f956c-25bd-4817-9c60-b7b3acafbfbf.PNG){: width="100%" height="100%"}{: .align-center}

- Add Socket 버튼을 눌러 weapon의 자식 노드를 만든뒤 이 노드에 점화 파티클을 spawn 하자
- Just like when we altered the animations, we're going to create a copy of Particle System so that if we mess it up, we don't mess up the original

![parcode](https://user-images.githubusercontent.com/80055816/209133829-20b5685c-9e92-429e-89e9-190dc5aa4fb5.PNG){: width="100%" height="100%"}{: .align-center}

![spawn](https://user-images.githubusercontent.com/80055816/209133898-d98d7d61-31fd-4a45-a424-30c7a355275e.PNG){: width="100%" height="100%"}{: .align-center}

### 03-28 Shooting Animation

![montage](https://user-images.githubusercontent.com/80055816/209170273-11ade971-db2f-475d-b9df-3f296e0feb97.PNG){: width="100%" height="100%"}{: .align-center}

- Animation Montage와 Animation Graph의 차이점은? 애니메이션 몽타주는 애니메이션들을 이어붙여 하나의 기능적인 애니메이션 시퀀스를 구성하기 위한 애셋이고, 애니메이션 그래프는 그것을 관리하며 애니메이션의 최종적인 로직을 구성하기 위해 사용된다 ([**참고**](https://bbagwang.com/unreal-engine/ue4-%EC%97%90%EC%84%9C%EC%9D%98-animation-montage%EC%99%80-animation-graph%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90/))

![montage2](https://user-images.githubusercontent.com/80055816/209175780-23f77390-309d-4f4c-a587-0d6f2ebc3d32.PNG){: width="100%" height="100%"}{: .align-center}

![slot](https://user-images.githubusercontent.com/80055816/209189204-4f67e4c1-d2a6-4f2a-8aad-e2632c2dfe28.PNG){: width="100%" height="100%"}{: .align-center}

- Now we're using the WeaponFire slot for this montage

![cache](https://user-images.githubusercontent.com/80055816/209182462-a746af17-ab35-4f09-824d-28a36ab26654.PNG){: width="100%" height="100%"}{: .align-center}

- New Save Cached Pose what does it do? You should use a cached poses because you can’t plug in the Output Pose into two different inputs ([**참고**](https://forums.unrealengine.com/t/new-save-cached-pose-what-does-it-do/407200))

![link](https://user-images.githubusercontent.com/80055816/209187079-2725131a-6a1d-4383-a008-50dde378b97b.PNG){: width="100%" height="100%"}{: .align-center}

- UAnimInstance 객체의 포인터를 얻어오는 방법은? GetMesh()->GetAnimInstance() 함수를 호출한다

![hip](https://user-images.githubusercontent.com/80055816/209188179-77c171e3-5170-491b-ac69-ee722ec39ab6.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/209188602-ee696b87-13dd-46b7-b5a9-b21c8b6e79a7.PNG){: width="100%" height="100%"}{: .align-center}

### 03-29 Blending Shooting Animations

![blend](https://user-images.githubusercontent.com/80055816/209201843-31525181-c501-40fe-91f6-d656be6a4d17.PNG){: width="100%" height="100%"}{: .align-center}

- In our case, we want to blend in such a way that the hip fire animation is only playing for the top half of the body and the regular ground locomotion is only playing for the bottom half

### 03-30 Line Tracing for Bullet Hits

```cpp
	//..

	if (BarrelSocket)
	{
		const FTransform SocketTransform = BarrelSocket->GetSocketTransform(GetMesh());

		if (MuzzleFlash)
		{
			UGameplayStatics::SpawnEmitterAtLocation(GetWorld(), MuzzleFlash, SocketTransform);
		}

		// Fill them in with data about the hit result
		FHitResult FireHit;
		const FVector Start{ SocketTransform.GetLocation() };
		// 여기서 Quat는 Quaternion을 의미한다
		const FQuat Rotation{ SocketTransform.GetRotation() };
		const FVector RotationAxis{ Rotation.GetAxisX() };
		const FVector End{ Start + RotationAxis * 50'000.f };

		// https://wergia.tistory.com/139
		// ECollisionChannel::ECC_Visibility는 화면에 보이는 객체가 맞으면 무조건 맞았다고 판정한다는 의미이다
		GetWorld()->LineTraceSingleByChannel(FireHit, Start, End, ECollisionChannel::ECC_Visibility);
		if (FireHit.bBlockingHit)
		{
			DrawDebugLine(GetWorld(), Start, End, FColor::Red, false, 2.f);
			DrawDebugPoint(GetWorld(), FireHit.Location, 5.f, FColor::Red, false, 2.f);
		}
	}

	//..
```

### 03-31 Impact Particles

```cpp
void AShooterCharacter::FireWeapon()
{
	//..

	if (ImpactParticles)
	{
		// FireHit 객체를 활용해 파티클이 생성될 위치를 지정해 줄 수 있다
		UGameplayStatics::SpawnEmitterAtLocation(GetWorld(), ImpactParticles, FireHit.Location);
	}

	//..
}
```

### 03-32 Beam Particles

![beam](https://user-images.githubusercontent.com/80055816/209317459-390c5b24-7ea3-4193-903d-e4c864709c0f.PNG){: width="100%" height="100%"}{: .align-center}

- M_Beam is material and Beam is texture

![particle](https://user-images.githubusercontent.com/80055816/209319442-e05c0355-426a-49dc-899e-a423dbf04c16.PNG){: width="100%" height="100%"}{: .align-center}

![target](https://user-images.githubusercontent.com/80055816/209330269-6cd33c01-5241-4c0e-a32a-46fc90674093.PNG){: width="100%" height="100%"}{: .align-center}

- This is what our particle system uses for the beam behavior

### 03-33 Socket Offset

```cpp
AShooterCharacter::AShooterCharacter() :
	BaseTurnRate(45.f),
	BaseLookUpRate(45.f)
{
	//..

	// SocketOffset을 조정하면 어떻게 되는가?
	// 카메라 위치가 바뀐다
	CameraBoom->SocketOffset = FVector(0.f, 50.f, 50.f);

	//..
}
```

### 03-34 HUD Class and Crosshairs

![ui](https://user-images.githubusercontent.com/80055816/209374423-2e356140-6d71-46c1-8c86-eec386015da8.PNG){: width="100%" height="100%"}{: .align-center}

![hud](https://user-images.githubusercontent.com/80055816/209374504-dcb0591f-6aa2-4cff-972e-b4cf428bdbd1.PNG){: width="100%" height="100%"}{: .align-center}

![default](https://user-images.githubusercontent.com/80055816/209374574-8b7001bb-75b6-42c8-ab3b-ce9124dd73f6.PNG){: width="100%" height="100%"}{: .align-center}

![event](https://user-images.githubusercontent.com/80055816/209374642-29a9748a-183f-4579-8037-9cf79c8bf552.PNG){: width="100%" height="100%"}{: .align-center}

![eventend](https://user-images.githubusercontent.com/80055816/209374818-8cb56439-f7e4-450b-ae43-2d0685e6dc2a.PNG){: width="100%" height="100%"}{: .align-center}

![center](https://user-images.githubusercontent.com/80055816/209374702-2087ca35-8e82-44dc-ba0b-498d7e039fdb.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 현상 때문에 필요한 노드는? Crosshair Half Width

### 03-35 Directing Rifle Shots

```cpp
void AShooterCharacter::FireWeapon()
{
	//..

	// GetViewportSize() 함수에 대해 설명하면?
	// Retrieve the size of the main viewport
	FVector2D ViewportSize;
	if (GEngine && GEngine->GameViewport)
	{
		GEngine->GameViewport->GetViewportSize(ViewportSize);
	}

	// Get screen space location of crosshairs
	FVector2D CrosshairLocation(ViewportSize.X / 2.f, ViewportSize.Y / 2.f);
	CrosshairLocation.Y -= 50.f;
	FVector CrosshairWorldPosition;
	FVector CrosshairWorldDirection;

	// UGameplayStatics::GetPlayerController() 함수에 대해 설명하면?
	// Returns the player controller at the specified player index
	bool bScreenToWorld = UGameplayStatics::DeprojectScreenToWorld(
		UGameplayStatics::GetPlayerController(this, 0),
		CrosshairLocation,
		CrosshairWorldPosition,
		CrosshairWorldDirection);

	if (bScreenToWorld) // was deprojection successful?
	{
		FHitResult ScreenTraceHit;
		const FVector Start{ CrosshairWorldPosition };
		const FVector End{ CrosshairWorldPosition + CrosshairWorldDirection * 50'000.f };

		// Set beam end point to line trace end point
		FVector BeamEndPoint{ End };

		// UWorld::LineTraceSingleByChannel에 대해 설명하면?
		// Trace a ray against the world using a specific channel and return the first blocking hit
		GetWorld()->LineTraceSingleByChannel(
			ScreenTraceHit,
			Start,
			End,
			ECollisionChannel::ECC_Visibility);
		if (ScreenTraceHit.bBlockingHit) // was there a trace hit?
		{
			// Beam end point is now trace hit location
			BeamEndPoint = ScreenTraceHit.Location;
			if (ImpactParticles)
			{
				UGameplayStatics::SpawnEmitterAtLocation(
					GetWorld(),
					ImpactParticles,
					ScreenTraceHit.Location);
			}
		}
		if (BeamParticles)
		{
			UParticleSystemComponent* Beam = UGameplayStatics::SpawnEmitterAtLocation(
				GetWorld(),
				BeamParticles,
				SocketTransform);
			if (Beam)
			{
				Beam->SetVectorParameter(FName("Target"), BeamEndPoint);
			}
		}
	}

	//..
}
```

### 03-36 Trace from Gun Barrel
- Look at the beam particles they're going through the first cube they're not actually stopping at the first cube why is this happening? This is because we're tracing from the screen location of the crosshairs instead of from the gun barrel
- Why we need a second trace? We also need to check to see if there's anything in between the gun barrel and that hit point

### 03-37 Refactor Beam End Code
- In summary, we refactored our code for getting the beam in location all into one neat, tidy function

### 03-38 Movement Offset Yaw

```cpp
//..

	// GetBaseAimRotation() 함수에 대해 설명하면?
	// Return the aim rotation for the Pawn
	FRotator AimRotation = ShooterCharacter->GetBaseAimRotation();

	// UKismetMathLibrary::MakeRotFromX() 함수에 대해 설명하면?
	// Builds a rotator given only a XAxis
	FRotator MovementRotation =
		UKismetMathLibrary::MakeRotFromX(
			ShooterCharacter->GetVelocity());

	MovementOffsetYaw = UKismetMathLibrary::NormalizedDeltaRotator(
		MovementRotation,
		AimRotation).Yaw;

//..
```

- FRotator 클래스에 대해 설명하면? Implements a container for rotation information All rotation values are stored in degrees

### 03-39 Strafing Blendspace

![blend](https://user-images.githubusercontent.com/80055816/209563105-6683ae78-76b1-480c-9ba1-1df0eeb04cfa.PNG){: width="100%" height="100%"}{: .align-center}

![jog](https://user-images.githubusercontent.com/80055816/209563145-a092150b-d1ee-483b-8cca-c2551c4c82fb.PNG){: width="100%" height="100%"}{: .align-center}

![adapt](https://user-images.githubusercontent.com/80055816/209563159-261e5160-eae0-4356-a235-db5099a59c03.PNG){: width="100%" height="100%"}{: .align-center}

### 03-40 Strafing Blendspace

![1d](https://user-images.githubusercontent.com/80055816/209568050-a79217e6-de97-4f0f-b3a0-c7f6c09d8dae.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/209568117-13a1606d-c3c2-4efd-b621-bf2ae4b1f7de.PNG){: width="100%" height="100%"}{: .align-center}

![jogstart](https://user-images.githubusercontent.com/80055816/209568138-99d571bf-b628-4330-b25a-acbc910f2c03.PNG){: width="100%" height="100%"}{: .align-center}

### 03-41 Jog Stop Blendspace

![stop](https://user-images.githubusercontent.com/80055816/209574769-e20f2fb6-cfdf-4a98-ba00-9e99e19bdec8.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
class SHOOTER_API UShooterAnimInstance : public UAnimInstance
{
	// Why we need this value? As soon as we stop moving, then our velocity is no longer to the right
	// Our velocity vector goes to zero
	// So there's no longer a delta rotation between our velocity and our Aim rotation
	// So what we need to do is we need to save this movement offset before we stop moving
	float LastMovementOffsetYaw;
};
```

![ratescale](https://user-images.githubusercontent.com/80055816/209575079-4e80e231-d3df-4ab1-a3e0-93db329312ed.PNG){: width="100%" height="100%"}{: .align-center}

![loop](https://user-images.githubusercontent.com/80055816/209574826-3ebca411-eae9-4719-b635-917ae575b80c.PNG){: width="100%" height="100%"}{: .align-center}

### 03-42 Smoothing Character Movement

![interpol](https://user-images.githubusercontent.com/80055816/209576613-558c6da5-15b8-4f2e-9f77-d7ea881786ab.PNG){: width="100%" height="100%"}{: .align-center}

![option](https://user-images.githubusercontent.com/80055816/209576719-f8766a59-0b32-4a60-a694-6ca06f9f3fb4.PNG){: width="100%" height="100%"}{: .align-center}

<br>

## Chapter 4 Aiming and Crosshairs

### 04-43 Zoom Field of View
- UPROPERTY의 Category에 대해 설명하면? 디테일패널에서 보일 항목 이름을 명칭 할 수 있습니다 ([**참고**](https://darkcatgame.tistory.com/62))
- FOV에 대해 설명하면? How much of the world is the camera showing
- When is the time Our follow camera will be constructed? At the BeginPlay() function

### 04-44 Aiming Zoom Interpolation
- SetFieldOfView() 함수에 대해 설명하면? Set Field Of View ([**참고**](https://docs.unrealengine.com/4.26/en-US/BlueprintAPI/Camera/SetFieldOfView/))
- FMath::FInterpTo() 함수에 대해 설명하면? Interpolate float from Current to Target ([**참고**](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Core/Math/FMath/FInterpTo/))

### 04-45 Aiming Pose

![cache](https://user-images.githubusercontent.com/80055816/209630729-50bce418-3f29-4bcd-9a17-c196da4a3201.PNG){: width="100%" height="100%"}{: .align-center}

![additive](https://user-images.githubusercontent.com/80055816/209630782-33770ee8-2cdd-4557-a60d-3288fe8ccfe4.PNG){: width="100%" height="100%"}{: .align-center}

- 애디티브 애니메이션에 대해 설명하면? 그 자체로는 유용하지 않고 애디티브 적용 처리를 해주어야 작동한다 위의 화면 처럼 No additive를 적용하면 애디티브 설정이 해제된다 ([**참고**](https://velog.io/@myverytinybrain/%EC%96%B8%EB%A6%AC%EC%96%BC-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-%EB%B8%94%EB%A3%A8%ED%94%84%EB%A6%B0%ED%8A%B83)), ([**참고**](https://velog.io/@cedongne/UE5-Unreal-Engine-5-%EA%B8%B8%EB%9D%BC%EC%9E%A1%EC%9D%B4-10.-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-%EB%B8%94%EB%A3%A8%ED%94%84%EB%A6%B0%ED%8A%B8-%EC%82%BC%EC%9D%B8%EC%B9%AD-%EC%83%98%ED%94%8C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%B6%84%EC%84%9D))

![weight](https://user-images.githubusercontent.com/80055816/209634722-b7902bcc-51e6-41c9-8711-7d9c2e03a24f.PNG){: width="100%" height="100%"}{: .align-center}

- Blend Weights에 대해 설명하면? 애디티브 포즈가 끼칠 영향력을 결정할 알파 값으로 사용할 (0.0, 1.0) 범위의 float (실수)값입니다 값이 0.0 이면 Additive 포즈는 Base 인풋 포즈에 전혀 더해지지 않음을, 1.0 이면 Additive 포즈를 Base 인풋 포즈에 온전히 더해버림을 뜻합니다 ([**참고**](https://docs.unrealengine.com/5.0/ko/animation-blueprint-blend-nodes-in-unreal-engine/#layeredblendperbone))

### 04-46 Aiming State Machine

```cpp
void UShooterAnimInstance::UpdateAnimationProperties(float DeltaTime)
{
	//..

	// How we get ShooterCharacter class variable?
	bAiming = ShooterCharacter->GetAiming();

	//..
}
```

![hip](https://user-images.githubusercontent.com/80055816/209670395-1d6642b7-d511-4d3f-822d-4db913b52666.PNG){: width="100%" height="100%"}{: .align-center}

![duration](https://user-images.githubusercontent.com/80055816/209670472-ceda03ef-ca92-4aa1-a56b-ec1092feefc5.PNG){: width="100%" height="100%"}{: .align-center}

![state](https://user-images.githubusercontent.com/80055816/209670526-aa5ce0a5-74eb-4f9e-9ef5-82c091b7b059.PNG){: width="100%" height="100%"}{: .align-center}

### 04-47 Aiming Look Sensitivity
- UPROPERTY(EditDefaultsOnly) means? Property can be changed only for Blurprints, Defaults means the Blueprint ([**참고**](https://forums.unrealengine.com/t/uproperty-editdefaultsonly-means/292728))
- UPROPERTY 에서 UIMin, UIMax의 역할은 무엇인가? 에디터에서 조정할 수 있는 숫자 범위를 지정하는 명령어입니다 ([**참고**](https://darkcatgame.tistory.com/62))

### 04-48 Crosshair Spread Velocity

```cpp
void AShooterCharacter::CalculateCrosshairSpread(float DeltaTime)
{
	FVector2D WalkSpeedRange{ 0.f, 600.f };
	FVector2D VelocityMultiplierRange{ 0.f, 1.f };
	FVector Velocity{ GetVelocity() };
	Velocity.Z = 0.f;

	// GetMappedRangeValueClamped() 함수의 기능에 대해 설명하면?
	// 특정 범위를 입력받아 정규화된 범위를 출력한다
	CrosshairVelocityFactor = FMath::GetMappedRangeValueClamped(
		WalkSpeedRange,
		VelocityMultiplierRange,
		Velocity.Size());

	CrosshairSpreadMultiplier = 0.5f + CrosshairVelocityFactor;
}
```

### 04-49 Spreading the Crosshairs

![bp](https://user-images.githubusercontent.com/80055816/209703910-d97cbf05-e692-43fb-99b4-a74f70068d53.PNG){: width="100%" height="100%"}{: .align-center}

![nodeone](https://user-images.githubusercontent.com/80055816/209703977-d7b8c8e0-e4cc-45c4-8121-3f72a605e501.PNG){: width="100%" height="100%"}{: .align-center}

- Get a referenceto Shooter Character 코멘트에 대해 설명하면? Shooter Character 변수를 세팅한다
- Set Screen Center and Crosshair Location 코멘트에 대해 설명하면? Crosshair의 위치를 세팅한다
- Raise screen center by 50 코멘트에 대해 설명하면? Crosshair의 위치를 y축으로 50만큼 올린다
- Make sure Shooter Character is valid 코멘트에 대해 설명하면? Shooter Character 변수가 세팅 되었는지 확인한다

![nodetwo](https://user-images.githubusercontent.com/80055816/209704016-87dbc0d4-6a47-4a51-8102-c13ea3ac1283.PNG){: width="100%" height="100%"}{: .align-center}

- Get a reference to Shooter Character 코멘트에 대해 설명하면? CrosshairSpreadMultiplier를 세팅한다
- Move X by half-width and Y by half-height 코멘트에 대해 설명하면? Crosshair가 중앙에서 오른쪽 하단으로 살짝 벗어나는 문제를 해결한다
- Center of crosshair location 코멘트에 대해 설명하면? CrosshairBaseCenter 변수를 세팅한다
- 노드를 연결하는 선중 일부분을 더블클릭 하면 해당 부분에 관절을 만들 수 있다
- Our conclusion is that spreading our crosshairs based on our character's velocity

### 04-50 Crosshair In Air Factor

```cpp
void AShooterCharacter::CalculateCrosshairSpread(float DeltaTime)
{
	//..

	// CrosshairInAirFactor 값이 증가하면 어떻게 되는가?
	// Crosshair가 더욱 확장된다
	CrosshairSpreadMultiplier = 0.5f + CrosshairVelocityFactor + CrosshairInAirFactor;

	//..
}
```

### 04-51 Crosshair Aim Factor

```cpp
void AShooterCharacter::CalculateCrosshairSpread(float DeltaTime)
{
	//..

		// CrosshairAimFactor는 더해야 할까 빼야 할까?
		// 정답은 빼야한다
		CrosshairSpreadMultiplier =
		0.5f +
		CrosshairVelocityFactor +
		CrosshairInAirFactor -
		CrosshairAimFactor;
}
```

### 04-52 Bullet Fire Aim Factor
- Remember, functions have addresses and memory just like variables do, which means you can take the address of a function

```cpp
void AShooterCharacter::StartCrosshairBulletFire()
{
	bFiringBullet = true;

	// SetTimer 함수에 대해 설명하면?
	// 하나의 함수를 Tick()과 다른 타임라인으로 지정된 시간 마다 호출 할 수 있다 ( https://midason.tistory.com/442 )
	GetWorldTimerManager().SetTimer(
		CrosshairShootTimer,
		this,
		&AShooterCharacter::FinishCrosshairBulletFire,
		ShootTimeDuration);
}
```

![diff](https://user-images.githubusercontent.com/80055816/209787317-d3c4fd8c-4d0f-43f0-b745-030b11bfef0a.PNG){: width="100%" height="100%"}{: .align-center}

- 마이너스 노드를 제거한 이유는? Zoom In 일때 50 픽셀과 Zoom Out 일때 50 픽셀은 다르기 때문에 Zoom In 일때 크로스 헤어 위치와 Zoom Out 일때 크로스 헤어 위치가 다른 현상이 있어서이다

### 04-53 New Level Assets

![demon](https://user-images.githubusercontent.com/80055816/209819963-1e4636ee-6212-4026-8368-1b20fc9877d5.PNG){: width="100%" height="100%"}{: .align-center}

![start](https://user-images.githubusercontent.com/80055816/209820055-7e81445e-fdee-44d4-a9b6-1ab829984b3d.PNG){: width="100%" height="100%"}{: .align-center}

### 04-54 Automatic Fire

```cpp
void AShooterCharacter::StartFireTimer()
{
	// bShouldFire 변수는 언제 어떤 함수에서 true로 리셋되는가?
	// AutomaticFireRate가 지난후 AutoFireReset() 함수에서 리셋된다
	if (bShouldFire)
	{
		FireWeapon();
		bShouldFire = false;
		GetWorldTimerManager().SetTimer(
			AutoFireTimer,
			this,
			&AShooterCharacter::AutoFireReset,
			AutomaticFireRate);
	}
}
```

- So just keep in mind that if you increase automatic fire rate, you have to increase that interpolation time for the gunfire spread factor

### 04-55 Jump Animations

![rule](https://user-images.githubusercontent.com/80055816/209841139-bab517d0-0629-44da-8ac5-7056672068b1.PNG){: width="100%" height="100%"}{: .align-center}

![rule2](https://user-images.githubusercontent.com/80055816/209841174-d581d52b-f283-4edf-bea2-ed66bdd6b2b3.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 상황에서 1번 트랜지션 룰은 어떤 결과를 만드는가? 결론적으로 2번 트랜지션 룰이 실행되게 된다

![in](https://user-images.githubusercontent.com/80055816/209841375-aa244658-8c4b-49ed-a9c4-2be957da312d.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/209841412-d478bf93-13a5-4907-98c4-2ba88fcb1e4f.PNG){: width="100%" height="100%"}{: .align-center}

![playrate](https://user-images.githubusercontent.com/80055816/209841453-41a0e118-a738-45d7-bf56-9e7ef5a471c2.PNG){: width="100%" height="100%"}{: .align-center}

![loop](https://user-images.githubusercontent.com/80055816/209841490-b0ca5a03-2e72-4b51-9a37-0e349178d8e0.PNG){: width="100%" height="100%"}{: .align-center}

<br>

## Chapter 5 The Weapon

### 05-56 Item Class

![actor](https://user-images.githubusercontent.com/80055816/209851528-d0d00dc4-3406-41e4-99ee-6416bc1dae31.PNG){: width="100%" height="100%"}{: .align-center}

- ctrl + k + o 키를 누르면 현재 헤더 파일의 cpp 파일이 나타난다

```cpp
AItem::AItem()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

	// 아래 구문의 실행 결과를 설명하면?
	// ItemMesh 이름을 지닌 USkeletalMeshComponent가 생성된다
	ItemMesh = CreateDefaultSubobject<USkeletalMeshComponent>(TEXT("ItemMesh"));
	SetRootComponent(ItemMesh);

	// 아래 구문의 실행 결과를 설명하면?
	// CollisionBox 이름을 지닌 UBoxComponent가 생성된다
	CollisionBox = CreateDefaultSubobject<UBoxComponent>(TEXT("CollisionBox"));
	CollisionBox->SetupAttachment(ItemMesh);
}
```

### 05-57 Weapon Class
- We haven't even created a blueprint based on item class because we're planning on using this as a parent class for other items such as a weapon

![itemclass](https://user-images.githubusercontent.com/80055816/209858762-8cf2d2e1-7528-45a6-b959-494cd10f158c.PNG){: width="100%" height="100%"}{: .align-center}

![duplicate](https://user-images.githubusercontent.com/80055816/209858805-0afe265e-fbe3-4ad0-bd3c-d3026deab8a2.PNG){: width="100%" height="100%"}{: .align-center}

![throw](https://user-images.githubusercontent.com/80055816/209858824-da8dcf31-0755-4d8c-8444-e816c16ea5d5.PNG){: width="100%" height="100%"}{: .align-center}

### 05-58 UMG Intro Lesson
- We're going to be creating a pick up widget, which is basically a pop up window that will show information on the screen in our game
- We're going to use UMG, which is the Unreal Motion graphics editor
- The canvas panel allows widgets to be laid out arbitrarily on some area
- Which means distance is measured from the anchor
- An overlay allows UI elements to stack on top of each other
- We also have horizontal and vertical boxes and these allow us to arrange items in a widget
- The hierarchy allows us to arrange items in our UMG

### 05-59 PickupWidget Blueprint
- ProjectName.Build.cs 파일에 있는 PublicDependencyModuleNames에 대해 설명하면? List of public dependency module names ([**참고**](https://docs.unrealengine.com/4.27/en-US/ProductionPipelines/BuildTools/UnrealBuildTool/ModuleFiles/))
- dependency에 대해 설명하면? 코드를 생성하는 프로젝트는 코드를 사용하는 프로젝트의 프로젝트 종속성이라고 합니다 ([**참고**](https://learn.microsoft.com/ko-kr/visualstudio/ide/how-to-create-and-remove-project-dependencies?view=vs-2022))

![widget](https://user-images.githubusercontent.com/80055816/210127948-e0bed3dc-4c4c-4462-ab41-1f9588cc13c4.PNG){: width="100%" height="100%"}{: .align-center}

![hierarchy](https://user-images.githubusercontent.com/80055816/210127959-4820e229-a081-446b-a6fd-5411938e6909.PNG){: width="100%" height="100%"}{: .align-center}

### 05-60 Finishing the PickupWidget

![anchor](https://user-images.githubusercontent.com/80055816/210133822-ed1f4ffd-bafc-4443-af41-7a845de8a193.PNG){: width="100%" height="100%"}{: .align-center}

- Anchor 설정시 어떤 효과가 있는지 설명하면? If we squeeze in the size of the widget, we're not going to squish that icon

![2d](https://user-images.githubusercontent.com/80055816/210133829-0b4cd511-924f-493f-a1c9-f6fefe5e265d.PNG){: width="100%" height="100%"}{: .align-center}

![font](https://user-images.githubusercontent.com/80055816/210133871-f0121b4d-f2d6-4f57-9126-2fbd766aac0b.PNG){: width="100%" height="100%"}{: .align-center}

### 05-61 Adding the Pickup Widget

![widget](https://user-images.githubusercontent.com/80055816/210137381-ecd41687-15c1-4539-973c-4cb23df1d443.PNG){: width="100%" height="100%"}{: .align-center}

### 05-62 Trace for Widget

```cpp
bool AShooterCharacter::TraceUnderCrosshairs(FHitResult& OutHitResult)
{
	//..

	// Crosshair의 위치를 찾는 방법은?
	// 뷰포트의 중심 위치를 구해 사용하면 된다
	FVector2D CrosshairLocation(ViewportSize.X / 2.f, ViewportSize.Y / 2.f);

	//..
```

```cpp
AItem::AItem()
{
	//..

	// https://forums.unrealengine.com/t/what-is-the-meaning-of-each-ecollisionresponse-enum-ecr-ignore-ecr-overlap-ecr-block-ecr-max/397826
	// 각 옵션들에 대해 이해하지 못했다 위의 사이트를 보고 다시 공부하자
	CollisionBox->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	CollisionBox->SetCollisionResponseToChannel(
		ECollisionChannel::ECC_Visibility,
		ECollisionResponse::ECR_Block);

	//..
}
```

### 05-63 Refactor Trace Under Crosshairs

```cpp
bool AShooterCharacter::GetBeamEndLocation(
	const FVector& MuzzleSocketLocation,
	FVector& OutBeamLocation)
{
	//..

	// 아래의 수식이 필요한 이유는?
	// 연기 파티클이 목표물을 좀더 확실하게 맞췄다는 느낌을 주기위해
	const FVector WeaponTraceEnd{ MuzzleSocketLocation + StartToEnd * 1.25f };

	// ..
}
```

### 05-64 Widget Trace When Close
- 델리게이트에 대해 설명하면? 등록된 함수에게 알려주는 기능 ([**참고**](https://3dmpengines.tistory.com/2057))
- OnComponentBeginOverlap() 함수에 대해 설명하면? Event called when something starts to overlaps this component, for example a player walking into a trigger ([**참고**](https://docs.unrealengine.com/5.0/en-US/API/Runtime/Engine/Components/UPrimitiveComponent/OnComponentBeginOverlap/))

### 05-65 Hide Widget

```cpp
void AShooterCharacter::TraceForItems()
{
	//..

	// 아래 코드의 결과는?
	// 이전 프레임의 아이템과 현재 프레임의 아이템이 다르면 Widget을 안보이게 처리한다
	if (TraceHitItemLastFrame)
	{
		if (HitItem != TraceHitItemLastFrame)
		{
			// We are hitting a different AItem this frame from last frame
			// Or AItem is null.
			TraceHitItemLastFrame->GetPickupWidget()->SetVisibility(false);
		}
	}

	//..
}
```

### 05-66 Bind Item Name

![bind](https://user-images.githubusercontent.com/80055816/210186302-d4fbadff-ec14-4f1e-a6cb-7e6fef906680.PNG){: width="100%" height="100%"}{: .align-center}

![item](https://user-images.githubusercontent.com/80055816/210186309-e936371a-85cc-4a49-900c-ce6958e3d3ae.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정을 설명하면? Item Reference가 유효하면 Item Name을 String 형으로 Return 한다

![widget](https://user-images.githubusercontent.com/80055816/210186314-ad2cfaec-7885-4aee-bf70-ecc377223c3f.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정이 왜 필요한가? Item Reference를 할당 해주기 위해

![conclude](https://user-images.githubusercontent.com/80055816/210186326-bb62382d-f226-40b6-bd72-328425e5668a.PNG){: width="100%" height="100%"}{: .align-center}

![view](https://user-images.githubusercontent.com/80055816/210186337-6e3aeee2-aa5f-4c81-86d7-50192d71613f.PNG){: width="100%" height="100%"}{: .align-center}

### 05-67 Bind Item Count

![bind](https://user-images.githubusercontent.com/80055816/210222558-9189c698-8689-4004-bdf9-2ed3d1fe1994.PNG){: width="100%" height="100%"}{: .align-center}

![item](https://user-images.githubusercontent.com/80055816/210196495-9f968f8c-7149-4449-8a9e-fabf085bcc62.PNG){: width="100%" height="100%"}{: .align-center}

### 05-68 Bind Star Opacity

![bindstar](https://user-images.githubusercontent.com/80055816/210220330-a38681ed-93fa-465b-85b6-a1607cac4073.PNG){: width="100%" height="100%"}{: .align-center}

![return](https://user-images.githubusercontent.com/80055816/210220387-d70afcd6-e732-48d8-924a-ac559f32b4fa.PNG){: width="100%" height="100%"}{: .align-center}

![one](https://user-images.githubusercontent.com/80055816/210220554-6935c617-6e10-4b63-8e5e-664feeae5b0f.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정에서 Get 노드는 무엇을 얻어오는 것인지 설명하면? Item.h 파일의 ActiveStars 변수의 1번 인덱스 상태
- Visible Alpha와 Hidden Alpha 노드의 RGBA 값을 각각 설명하면? (1.0, 1.0, 1.0, 1.0)과 (0.0, 0.0, 0.0, 0.0)

![collapse](https://user-images.githubusercontent.com/80055816/210220591-71aadc63-c38d-43ea-bb3d-ebfb2cfc3d03.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 기능으로 함수를 하나의 덩어리로 묶을수 있다

![gfunction](https://user-images.githubusercontent.com/80055816/210220638-cabb1107-8231-435a-b10c-f5e390980825.PNG){: width="100%" height="100%"}{: .align-center}

- local variables는 지역변수 variables는 멤버변수를 의미한다

![opacity](https://user-images.githubusercontent.com/80055816/210220696-43f6923b-ae83-4a00-bd40-c1df3eb6adb5.PNG){: width="100%" height="100%"}{: .align-center}

![final](https://user-images.githubusercontent.com/80055816/210220728-538d9540-12db-4985-ad39-f8d72332ac38.PNG){: width="100%" height="100%"}{: .align-center}

### 05-69 Spawn Default Weapon
- `TSubclassOf<AWeapon>`에 대해 설명하면? 블루프린트 창의 디테일 패널 드롭다운 메뉴에 AWeapon의 파생 클래스만 표시된다 ([**참고**](https://wergia.tistory.com/136))

![socket](https://user-images.githubusercontent.com/80055816/210248575-dddfd384-02a5-46c4-a052-88220baf3964.PNG){: width="100%" height="100%"}{: .align-center}

![position](https://user-images.githubusercontent.com/80055816/210248614-83d26798-0d3d-4373-9440-df8d9108724d.PNG){: width="100%" height="100%"}{: .align-center}

![base](https://user-images.githubusercontent.com/80055816/210248645-5eeb3e71-d388-462e-8ca1-1a0e65230e0d.PNG){: width="100%" height="100%"}{: .align-center}

![class](https://user-images.githubusercontent.com/80055816/210248669-f3749df6-c8fe-4623-bf55-12583ad4ccac.PNG){: width="100%" height="100%"}{: .align-center}

### 05-70 Equip Function

```cpp
AWeapon* AShooterCharacter::SpawnDefaultWeapon()
{
	if (DefaultWeaponClass)
	{
		// Spawn the Weapon
		return GetWorld()->SpawnActor<AWeapon>(DefaultWeaponClass);
	}

	return nullptr;
}
```

- SpawnActor() 함수에 대해 설명하면? 지정된 클래스의 새 인스턴스를 생성한 다음 새로 생성된 Actor 로의 포인터를 반환합니다 ([**참고**](https://pros2.tistory.com/129))

### 05-71 Item State Lesson
- Item States are Equipped, Pickup, Equipinterping, PickedUp, Falling

### 05-72 Item State

```cpp
void AShooterCharacter::EquipWeapon(AWeapon* WeaponToEquip)
{
	//..

	// 아래 함수가 실행되면 어떤 일이 일어나는지 설명하면?
	// Weapon 상태가 EItemState::EIS_Equipped로 바뀐다
	EquippedWeapon->SetItemState(EItemState::EIS_Equipped);

	//..
}
```

### 05-73 Set Item Properties

```cpp
void AItem::SetItemProperties(EItemState State)
{
	// EIS_Pickup 스테이트에 대해 설명하면?
	// The item Mesh to ignore all collision channels
	// The Area Sphere to Overlap all collision channels
	// The Collision Box to ignore all channels except for Visibility, which it will block
	switch (State)
	{
	case EItemState::EIS_Pickup:
		// Set mesh properties
		ItemMesh->SetSimulatePhysics(false);
		ItemMesh->SetEnableGravity(false);
		ItemMesh->SetVisibility(true);
		ItemMesh->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);

		// SetCollisionEnabled에 대해 설명하면?
		// Controls what kind of collision is enabled for this body
		// https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/Components/UPrimitiveComponent/SetCollisionEnabled/ 참고
		ItemMesh->SetCollisionEnabled(ECollisionEnabled::NoCollision);
		// Set AreaSphere properties
		AreaSphere->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Overlap);
		AreaSphere->SetCollisionEnabled(ECollisionEnabled::QueryOnly);
		// Set CollisionBox properties
		CollisionBox->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
		CollisionBox->SetCollisionResponseToChannel(
			ECollisionChannel::ECC_Visibility,
			ECollisionResponse::ECR_Block);
		
		// QueryAndPhysics에 대해 설명하면?
		// Can be used for both spatial queries (raycasts, sweeps, overlaps)
		// And simulation (rigid body, constraints)
		// https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/Engine/ECollisionEnabled__Type/ 참고
		CollisionBox->SetCollisionEnabled(ECollisionEnabled::QueryAndPhysics);
		break;

		//..
	}
}
```

### 05-74 Detach Weapon

```cpp
void AShooterCharacter::DropWeapon()
{
	if (EquippedWeapon)
	{
		// 아래 코드의 실행 결과를 예측하면?
		// 무기가 손에서 분리되어 월드 공간에 놓인다 ( 더 공부 해야됨 )
		FDetachmentTransformRules DetachmentTransformRules(EDetachmentRule::KeepWorld, true);
		EquippedWeapon->GetItemMesh()->DetachFromComponent(DetachmentTransformRules);

		EquippedWeapon->SetItemState(EItemState::EIS_Falling);
	}
}
```

### 05-75 Item Falling State
- Physics Asset Editor에 대해 설명하면? The editor used to set up physics bodies and constraints for physical simulation and collision for Skeletal Meshes ([**참고**](https://docs.unrealengine.com/4.26/en-US/InteractiveExperiences/Physics/PhysicsAssetEditor/))

![phy](https://user-images.githubusercontent.com/80055816/210300800-49ef8011-cb00-4be5-9ca9-52d63aaa3238.PNG){: width="100%" height="100%"}{: .align-center}

![connect](https://user-images.githubusercontent.com/80055816/210300812-00c1faee-768b-497a-b425-f0c3cc54f7c4.PNG){: width="100%" height="100%"}{: .align-center}

### 05-76 Throw Weapon
- FRotator() 생성자의 매개변수를 순서데로 나열하면? Pitch, Yaw, Roll ([**참고**](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Core/Math/FRotator/__ctor/5/))

```cpp
void AWeapon::ThrowWeapon()
{
	//..

	// ETeleportType::TeleportPhysics에 대해 설명하면?
	// Teleport physics body so that velocity remains the same and no collision occurs
	// https://docs.unrealengine.com/4.26/en-US/API/Runtime/Engine/Engine/ETeleportType/ 참고
	GetItemMesh()->SetWorldRotation(MeshRotation, false, nullptr, ETeleportType::TeleportPhysics);

	// ImpulseDirection 변수에 대입연산 하고 있는데 어떻게 MeshRight.RotateAngleAxis() 함수와
	// ImpulseDirection.RotateAngleAxis() 함수가 둘다 적용되고 있는거지?
	FVector ImpulseDirection = MeshRight.RotateAngleAxis(-20.f, MeshForward);
	float RandomRotation{ 30.f };
	ImpulseDirection = ImpulseDirection.RotateAngleAxis(RandomRotation, FVector(0.f, 0.f, 1.f));
	ImpulseDirection *= 20'000.f;

	// 아래 함수에 대해 설명하면?
	// Add an impulse to a single rigid body Good for one time instant burst
	// https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/Components/UPrimitiveComponent/AddImpulse/ 참고
	GetItemMesh()->AddImpulse(ImpulseDirection);

	//..
}
```

```cpp
AWeapon::AWeapon() :
	ThrowWeaponTime(0.7f),
	bFalling(false)
{
	// Now, if you ever find that your tick function isn't being called
	// Make sure you have this in your constructor because an actor will not tick
	PrimaryActorTick.bCanEverTick = true;
}
```

```cpp
void AWeapon::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	// 아래 내용은 왜 필요한가? Keep the Weapon upright
	if (GetItemState() == EItemState::EIS_Falling && bFalling)
	{
		const FRotator MeshRotation{ 0.f, GetItemMesh()->GetComponentRotation().Yaw, 0.f };
		GetItemMesh()->SetWorldRotation(MeshRotation, false, nullptr, ETeleportType::TeleportPhysics);
	}
}
```

### 05-77 Swap Weapon

![item](https://user-images.githubusercontent.com/80055816/210419936-8a0aee2f-7cfb-49e4-a4bd-8ef4d6c06ece.PNG){: width="100%" height="100%"}{: .align-center}

- Swap이 잘되는지 확인하기 위해 위의 사진처럼 머티리얼과 이름을 바꿔보자

```cpp
void AShooterCharacter::SwapWeapon(AWeapon* WeaponToSwap)
{
	DropWeapon();
	EquipWeapon(WeaponToSwap);

	// 아래 코드가 필요한 이유는?
	// 무기가 스와핑 되고난 이후에 아래 변수들이 초기화 되지 않는다
	TraceHitItem = nullptr;
	TraceHitItemLastFrame = nullptr;
}
```

```cpp
void AItem::SetItemProperties(EItemState State)
{
	//..

	// 아래 코드가 필요한 이유는?
	// 착용 상태에서는 Widget이 출력되면 안되기 때문이다
	case EItemState::EIS_Equipped:
	PickupWidget->SetVisibility(false);

	//..
}
```

<br>

## Chapter 6 Item Interpolation

### 06-78 Item Interping Slide
- we will use linear interpolation
- Here's a curve where the vertical axis represents our items Z position in space and the horizontal axis represents time

### 06-79 Camera Interp Location

```cpp
class SHOOTER_API AShooterCharacter : public ACharacter
{
	// CameraInterpDistance 변수의 생성 목적은 무엇인지 설명하면?
	// Distance outward from the camera for the interp destination
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Items, meta = (AllowPrivateAccess = "true"))
		float CameraInterpDistance;

	// CameraInterpElevation 변수의 생성 목적은 무엇인지 설명하면?
	// Distance upward from the camera for the interp destination
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Items, meta = (AllowPrivateAccess = "true"))
		float CameraInterpElevation;
}
```

### 06-80 Get Pickup Item

```cpp
void AShooterCharacter::GetPickupItem(AItem* Item)
{
	auto Weapon = Cast<AWeapon>(Item);
	if (Weapon)
	{
		SwapWeapon(Weapon);
	}
}
```

### 06-81 Item Z Curve

![curve](https://user-images.githubusercontent.com/80055816/210357742-c9c2a7a1-2282-441b-a8bd-05d8935f5923.PNG){: width="100%" height="100%"}{: .align-center}

![auto](https://user-images.githubusercontent.com/80055816/210360752-137d29da-7e96-4c6d-87e4-5bbef9c4c7bd.PNG){: width="100%" height="100%"}{: .align-center}

### 06-82 Item Interp Variables

```cpp
// 아래 함수는 누가 호출할 예정인지 설명하면?
// 플레이어가 호출할 예정이다
void AItem::StartItemCurve(AShooterCharacter* Char)
{
	// Store a handle to the Character
	Character = Char;
	// Store initial location of the Item
	ItemInterpStartLocation = GetActorLocation();
	bInterping = true;
	SetItemState(EItemState::EIS_EquipInterping);

	GetWorldTimerManager().SetTimer(
		ItemInterpTimer,
		this,
		&AItem::FinishInterping,
		ZCurveTime);
}
```

### 06-83 Interping State Properties

```cpp
void AItem::SetItemProperties(EItemState State)
{
	//..

	case EItemState::EIS_EquipInterping:
	PickupWidget->SetVisibility(false);
	// Set mesh properties
	ItemMesh->SetSimulatePhysics(false);
	ItemMesh->SetEnableGravity(false);
	ItemMesh->SetVisibility(true);
	ItemMesh->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	ItemMesh->SetCollisionEnabled(ECollisionEnabled::NoCollision);
	// Set AreaSphere properties
	AreaSphere->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	AreaSphere->SetCollisionEnabled(ECollisionEnabled::NoCollision);
	// Set CollisionBox properties
	CollisionBox->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	CollisionBox->SetCollisionEnabled(ECollisionEnabled::NoCollision);
	break;

	//..
}
```

### 06-84 Following the Z Curve

![diff](https://user-images.githubusercontent.com/80055816/210382801-4afee055-96d7-41ba-8f23-983583d2e68f.png){: width="100%" height="100%"}{: .align-center}

### 06-85 Interp Item X and Y

![interp](https://user-images.githubusercontent.com/80055816/210393380-8a7af87c-5144-4927-a7bd-68d3ca0d46b4.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
FVector AShooterCharacter::GetCameraInterpLocation()
{
	const FVector CameraWorldLocation{ FollowCamera->GetComponentLocation() };
	const FVector CameraForward{ FollowCamera->GetForwardVector() };
	// Desired = CameraWorldLocation + Forward * A + Up * B
	return CameraWorldLocation + CameraForward * CameraInterpDistance
		+ FVector(0.f, 0.f, CameraInterpElevation);
}
```

### 06-86 Interp Rotation
- Whenever the camera rotates, we would like our item to rotate exactly that same amount

```cpp
void AItem::ItemInterp(float DeltaTime)
{
	//..

	// 아래 코드의 효과는?
	// 카메라가 회전하면 아이템도 카메라가 회전한 만큼 회전한다
	const FRotator CameraRotation{ Character->GetFollowCamera()->GetComponentRotation() };
	FRotator ItemRotation{ 0.f, CameraRotation.Yaw + InterpInitialYawOffset, 0.f };
	SetActorRotation(ItemRotation, ETeleportType::TeleportPhysics);

	//..
}
```

### 06-87 Interp Scale

```cpp
void AItem::ItemInterp(float DeltaTime)
{
	//..

	if (ItemScaleCurve)
	{
		const float ScaleCurveValue = ItemScaleCurve->GetFloatValue(ElapsedTime);
		SetActorScale3D(FVector(ScaleCurveValue, ScaleCurveValue, ScaleCurveValue));
	}

	//..
}
```

<br>

## Chapter 7 Reloading

### 07-88 Retargeting Animations in Unreal Engine 5
- I cover the new way of retargeting animations in Unreal Engine 5 in a new YouTube video
- The link is in the resources for Lecture 88: Retargeting Animations

### 07-89 Retargeting Animations
- What does that mean, retarget? the process of retargeting and animation is taking that animation and assigning it to a different skeleton
- In order to retarget this animation to the Belka skeleton, we're going to use something called a rig

![rig](https://user-images.githubusercontent.com/80055816/210504496-28fc5007-b0c5-474f-9de0-71cb1d8df7c8.PNG){: width="100%" height="100%"}{: .align-center}

![match](https://user-images.githubusercontent.com/80055816/210504554-80497dd5-2a16-4cc3-b63c-6cfa0f87f3ee.PNG){: width="100%" height="100%"}{: .align-center}

![base](https://user-images.githubusercontent.com/80055816/210504595-140785cd-753b-435c-9479-00ffa549a22a.PNG){: width="100%" height="100%"}{: .align-center}

![reload](https://user-images.githubusercontent.com/80055816/210504645-1c845563-5033-4d90-ac25-522127163c7b.PNG){: width="100%" height="100%"}{: .align-center}

![retarget](https://user-images.githubusercontent.com/80055816/210504691-94b82be3-2a9a-49a6-b787-6f720f5a171f.PNG){: width="100%" height="100%"}{: .align-center}

![morphing](https://user-images.githubusercontent.com/80055816/210504764-15b10669-53b7-4f42-a3da-977c4147f129.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 이미지에 나타난 과정을 거치는 이유는? To fix morphing problem

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
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
last_modified_at: 2022-12-18
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

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
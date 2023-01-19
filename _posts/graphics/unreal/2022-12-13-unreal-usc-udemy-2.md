---
title:  "Unreal Engine C++ The Ultimate Shooter Course (2)"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2023-01-10
---

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

### 04-54 New Level Assets ( Warning )

![demon](https://user-images.githubusercontent.com/80055816/209819963-1e4636ee-6212-4026-8368-1b20fc9877d5.PNG){: width="100%" height="100%"}{: .align-center}

![start](https://user-images.githubusercontent.com/80055816/209820055-7e81445e-fdee-44d4-a9b6-1ab829984b3d.PNG){: width="100%" height="100%"}{: .align-center}

### 04-55 Automatic Fire

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

### 04-56 Jump Animations

![rule](https://user-images.githubusercontent.com/80055816/209841139-bab517d0-0629-44da-8ac5-7056672068b1.PNG){: width="100%" height="100%"}{: .align-center}

![rule2](https://user-images.githubusercontent.com/80055816/209841174-d581d52b-f283-4edf-bea2-ed66bdd6b2b3.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 상황에서 1번 트랜지션 룰은 어떤 결과를 만드는가? 결론적으로 2번 트랜지션 룰이 실행되게 된다

![in](https://user-images.githubusercontent.com/80055816/209841375-aa244658-8c4b-49ed-a9c4-2be957da312d.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/209841412-d478bf93-13a5-4907-98c4-2ba88fcb1e4f.PNG){: width="100%" height="100%"}{: .align-center}

![playrate](https://user-images.githubusercontent.com/80055816/209841453-41a0e118-a738-45d7-bf56-9e7ef5a471c2.PNG){: width="100%" height="100%"}{: .align-center}

![loop](https://user-images.githubusercontent.com/80055816/209841490-b0ca5a03-2e72-4b51-9a37-0e349178d8e0.PNG){: width="100%" height="100%"}{: .align-center}

<br>

## Chapter 5 The Weapon

### 05-57 Item Class

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

### 05-58 Weapon Class
- We haven't even created a blueprint based on item class because we're planning on using this as a parent class for other items such as a weapon

![itemclass](https://user-images.githubusercontent.com/80055816/209858762-8cf2d2e1-7528-45a6-b959-494cd10f158c.PNG){: width="100%" height="100%"}{: .align-center}

![duplicate](https://user-images.githubusercontent.com/80055816/209858805-0afe265e-fbe3-4ad0-bd3c-d3026deab8a2.PNG){: width="100%" height="100%"}{: .align-center}

![throw](https://user-images.githubusercontent.com/80055816/209858824-da8dcf31-0755-4d8c-8444-e816c16ea5d5.PNG){: width="100%" height="100%"}{: .align-center}

### 05-59 UMG Intro Lesson
- We're going to be creating a pick up widget, which is basically a pop up window that will show information on the screen in our game
- We're going to use UMG, which is the Unreal Motion graphics editor
- The canvas panel allows widgets to be laid out arbitrarily on some area
- Which means distance is measured from the anchor
- An overlay allows UI elements to stack on top of each other
- We also have horizontal and vertical boxes and these allow us to arrange items in a widget
- The hierarchy allows us to arrange items in our UMG

### 05-60 PickupWidget Blueprint
- ProjectName.Build.cs 파일에 있는 PublicDependencyModuleNames에 대해 설명하면? List of public dependency module names ([**참고**](https://docs.unrealengine.com/4.27/en-US/ProductionPipelines/BuildTools/UnrealBuildTool/ModuleFiles/))
- dependency에 대해 설명하면? 코드를 생성하는 프로젝트는 코드를 사용하는 프로젝트의 프로젝트 종속성이라고 합니다 ([**참고**](https://learn.microsoft.com/ko-kr/visualstudio/ide/how-to-create-and-remove-project-dependencies?view=vs-2022))

![widget](https://user-images.githubusercontent.com/80055816/210127948-e0bed3dc-4c4c-4462-ab41-1f9588cc13c4.PNG){: width="100%" height="100%"}{: .align-center}

![hierarchy](https://user-images.githubusercontent.com/80055816/210127959-4820e229-a081-446b-a6fd-5411938e6909.PNG){: width="100%" height="100%"}{: .align-center}

### 05-61 Finishing the PickupWidget

![anchor](https://user-images.githubusercontent.com/80055816/210133822-ed1f4ffd-bafc-4443-af41-7a845de8a193.PNG){: width="100%" height="100%"}{: .align-center}

- Anchor 설정시 어떤 효과가 있는지 설명하면? If we squeeze in the size of the widget, we're not going to squish that icon
- Anchor를 사용하려면 어떻게 해야 하는가? Canvas Panel 아래 계층에 포함되어야 한다

![2d](https://user-images.githubusercontent.com/80055816/210133829-0b4cd511-924f-493f-a1c9-f6fefe5e265d.PNG){: width="100%" height="100%"}{: .align-center}

![font](https://user-images.githubusercontent.com/80055816/210133871-f0121b4d-f2d6-4f57-9126-2fbd766aac0b.PNG){: width="100%" height="100%"}{: .align-center}

### 05-62 Adding the Pickup Widget

![widget](https://user-images.githubusercontent.com/80055816/210137381-ecd41687-15c1-4539-973c-4cb23df1d443.PNG){: width="100%" height="100%"}{: .align-center}

### 05-63 Trace for Widget

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

### 05-64 Refactor Trace Under Crosshairs

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

### 05-65 Widget Trace When Close
- 델리게이트에 대해 설명하면? 등록된 함수에게 알려주는 기능 ([**참고**](https://3dmpengines.tistory.com/2057))
- OnComponentBeginOverlap() 함수에 대해 설명하면? Event called when something starts to overlaps this component, for example a player walking into a trigger ([**참고**](https://docs.unrealengine.com/5.0/en-US/API/Runtime/Engine/Components/UPrimitiveComponent/OnComponentBeginOverlap/))

### 05-66 Hide Widget

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

### 05-67 Bind Item Name

![bind](https://user-images.githubusercontent.com/80055816/210186302-d4fbadff-ec14-4f1e-a6cb-7e6fef906680.PNG){: width="100%" height="100%"}{: .align-center}

![item](https://user-images.githubusercontent.com/80055816/210186309-e936371a-85cc-4a49-900c-ce6958e3d3ae.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정을 설명하면? Item Reference가 유효하면 Item Name을 String 형으로 Return 한다

![widget](https://user-images.githubusercontent.com/80055816/210186314-ad2cfaec-7885-4aee-bf70-ecc377223c3f.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정이 왜 필요한가? Item Reference를 할당 해주기 위해

![conclude](https://user-images.githubusercontent.com/80055816/210186326-bb62382d-f226-40b6-bd72-328425e5668a.PNG){: width="100%" height="100%"}{: .align-center}

![view](https://user-images.githubusercontent.com/80055816/210186337-6e3aeee2-aa5f-4c81-86d7-50192d71613f.PNG){: width="100%" height="100%"}{: .align-center}

### 05-68 Bind Item Count

![bind](https://user-images.githubusercontent.com/80055816/210222558-9189c698-8689-4004-bdf9-2ed3d1fe1994.PNG){: width="100%" height="100%"}{: .align-center}

![item](https://user-images.githubusercontent.com/80055816/210196495-9f968f8c-7149-4449-8a9e-fabf085bcc62.PNG){: width="100%" height="100%"}{: .align-center}

### 05-69 Bind Star Opacity

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

### 05-70 Spawn Default Weapon
- `TSubclassOf<AWeapon>`에 대해 설명하면? 블루프린트 창의 디테일 패널 드롭다운 메뉴에 AWeapon의 파생 클래스만 표시된다 ([**참고**](https://wergia.tistory.com/136))

![socket](https://user-images.githubusercontent.com/80055816/210248575-dddfd384-02a5-46c4-a052-88220baf3964.PNG){: width="100%" height="100%"}{: .align-center}

![position](https://user-images.githubusercontent.com/80055816/210248614-83d26798-0d3d-4373-9440-df8d9108724d.PNG){: width="100%" height="100%"}{: .align-center}

![base](https://user-images.githubusercontent.com/80055816/210248645-5eeb3e71-d388-462e-8ca1-1a0e65230e0d.PNG){: width="100%" height="100%"}{: .align-center}

![class](https://user-images.githubusercontent.com/80055816/210248669-f3749df6-c8fe-4623-bf55-12583ad4ccac.PNG){: width="100%" height="100%"}{: .align-center}

### 05-71 Equip Function

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

### 05-72 Item State Lesson
- Item States are Equipped, Pickup, Equipinterping, PickedUp, Falling

### 05-73 Item State

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

### 05-74 Set Item Properties

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

### 05-75 Detach Weapon

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

### 05-76 Item Falling State
- Physics Asset Editor에 대해 설명하면? The editor used to set up physics bodies and constraints for physical simulation and collision for Skeletal Meshes ([**참고**](https://docs.unrealengine.com/4.26/en-US/InteractiveExperiences/Physics/PhysicsAssetEditor/))

![phy](https://user-images.githubusercontent.com/80055816/210300800-49ef8011-cb00-4be5-9ca9-52d63aaa3238.PNG){: width="100%" height="100%"}{: .align-center}

![connect](https://user-images.githubusercontent.com/80055816/210300812-00c1faee-768b-497a-b425-f0c3cc54f7c4.PNG){: width="100%" height="100%"}{: .align-center}

### 05-77 Throw Weapon
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

### 05-78 Swap Weapon

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

### 06-79 Item Interping Slide
- we will use linear interpolation
- Here's a curve where the vertical axis represents our items Z position in space and the horizontal axis represents time

### 06-80 Camera Interp Location

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

### 06-81 Get Pickup Item

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

### 06-82 Item Z Curve

![curve](https://user-images.githubusercontent.com/80055816/210357742-c9c2a7a1-2282-441b-a8bd-05d8935f5923.PNG){: width="100%" height="100%"}{: .align-center}

![auto](https://user-images.githubusercontent.com/80055816/210360752-137d29da-7e96-4c6d-87e4-5bbef9c4c7bd.PNG){: width="100%" height="100%"}{: .align-center}

### 06-83 Item Interp Variables

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

### 06-84 Interping State Properties

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

### 06-85 Following the Z Curve

![diff](https://user-images.githubusercontent.com/80055816/210382801-4afee055-96d7-41ba-8f23-983583d2e68f.png){: width="100%" height="100%"}{: .align-center}

### 06-86 Interp Item X and Y

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

### 06-87 Interp Rotation
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

### 06-88 Interp Scale

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

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
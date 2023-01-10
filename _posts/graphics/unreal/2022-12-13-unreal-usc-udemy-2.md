---
title:  "Unreal Engine C++ The Ultimate Shooter Course (1)"

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

## Chapter 7 Reloading

### 07-89 Retargeting Animations in Unreal Engine 5
- I cover the new way of retargeting animations in Unreal Engine 5 in a new YouTube video
- The link is in the resources for Lecture 88: Retargeting Animations

### 07-90 Retargeting Animations
- What does that mean, retarget? the process of retargeting and animation is taking that animation and assigning it to a different skeleton
- In order to retarget this animation to the Belka skeleton, we're going to use something called a rig

![rig](https://user-images.githubusercontent.com/80055816/210504496-28fc5007-b0c5-474f-9de0-71cb1d8df7c8.PNG){: width="100%" height="100%"}{: .align-center}

![match](https://user-images.githubusercontent.com/80055816/210504554-80497dd5-2a16-4cc3-b63c-6cfa0f87f3ee.PNG){: width="100%" height="100%"}{: .align-center}

![base](https://user-images.githubusercontent.com/80055816/210504595-140785cd-753b-435c-9479-00ffa549a22a.PNG){: width="100%" height="100%"}{: .align-center}

![reload](https://user-images.githubusercontent.com/80055816/210504645-1c845563-5033-4d90-ac25-522127163c7b.PNG){: width="100%" height="100%"}{: .align-center}

![retarget](https://user-images.githubusercontent.com/80055816/210504691-94b82be3-2a9a-49a6-b787-6f720f5a171f.PNG){: width="100%" height="100%"}{: .align-center}

![morphing](https://user-images.githubusercontent.com/80055816/210504764-15b10669-53b7-4f42-a3da-977c4147f129.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 이미지에 나타난 과정을 거치는 이유는? To fix morphing problem

### 07-91 Edit Animations In Unreal
- we're going to make a duplicate of this animation because we're going to have a different version of this reload animation for each weapon

![adjust](https://user-images.githubusercontent.com/80055816/210524863-f0acfc48-d65e-4c4d-9bee-601b8a62db86.PNG){: width="100%" height="100%"}{: .align-center}

![key](https://user-images.githubusercontent.com/80055816/210601469-b2a833a2-84d0-4de3-abd1-16cd9a99cf70.PNG){: width="100%" height="100%"}{: .align-center}

### 07-92 Ammo
- 언리얼에서 열거형에 대해 설명하면? 일반적인 enum이 아닌 enum class로 만들어야 한다 그리고 UENUM은 uint8만을 지원한다 ([**참고**](https://wergia.tistory.com/150))

```cpp
// 언리얼에서 지원하는 Map 클래스
TMap<EAmmoType, int32> AmmoMap;
```

### 07-93 Ammo Count Widget

![widget](https://user-images.githubusercontent.com/80055816/210568546-458d2d02-2771-4b7f-8d9c-44bef1f166f0.PNG){: width="100%" height="100%"}{: .align-center}

![custom](https://user-images.githubusercontent.com/80055816/210568649-a1425022-cbb5-4d8a-9e15-7415ab1bce00.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/210568701-47a5c31e-3960-4dbf-a296-a19926d9546f.PNG){: width="100%" height="100%"}{: .align-center}

### 07-94 Draw Ammo Count To Screen

![pc](https://user-images.githubusercontent.com/80055816/210598818-fddce373-64e2-409f-9438-e8c5cab7966f.PNG){: width="100%" height="100%"}{: .align-center}

- We're going to draw this HUD to the screen using our player controller class and we're going to do this from C++

![over](https://user-images.githubusercontent.com/80055816/210599031-24318290-8f20-4691-93a2-2da0ea7c03ae.PNG){: width="100%" height="100%"}{: .align-center}

- ShooterHUDOverlay를 만드는 이유는? We want to position AmmoCountBP where we want it

![bp](https://user-images.githubusercontent.com/80055816/210599590-33bb8caf-7fe5-4ee1-a91d-2f3e770f227d.PNG){: width="100%" height="100%"}{: .align-center}

![cho](https://user-images.githubusercontent.com/80055816/210599662-8d6db63a-c5bc-4053-b8c4-9a4ab69e616a.PNG){: width="100%" height="100%"}{: .align-center}

![final](https://user-images.githubusercontent.com/80055816/210599732-9c73eac4-d84d-496e-bffe-6210de6bed09.PNG){: width="100%" height="100%"}{: .align-center}

![mode](https://user-images.githubusercontent.com/80055816/210599812-b5a39b7d-17dc-4b23-b8df-ea1a37893569.PNG){: width="100%" height="100%"}{: .align-center}

- 위의 과정은 무엇을 위함인지 설명하면? This right here will override what we have set in the project settings for every level

### 07-95 Weapon Ammo in C++

```cpp
bool AShooterCharacter::WeaponHasAmmo()
{
	if (EquippedWeapon == nullptr) return false;

	return EquippedWeapon->GetAmmo() > 0;
}
```

### 07-96 Bind Weapon Ammo

![select](https://user-images.githubusercontent.com/80055816/210627317-85c41cf1-a78a-4b32-9733-925cc285cfb5.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/210627373-ad6c0743-2441-4414-acee-1aff8021b345.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/210627419-bf234a52-a424-40e6-b873-eef1c35ebaae.PNG){: width="100%" height="100%"}{: .align-center}

![code](https://user-images.githubusercontent.com/80055816/210627463-f70932fd-6246-4b46-ad40-a5fd7c1bf093.PNG){: width="100%" height="100%"}{: .align-center}

### 07-97 Fixing Barrel Socket Location

![add](https://user-images.githubusercontent.com/80055816/210627524-ad8b653d-d27d-45f9-9e7a-c90fde2b8789.PNG){: width="100%" height="100%"}{: .align-center}

- So we don't actually want the mesh of our character We want the mesh of our equipped weapon

### 07-98 Improving Weapon Fire Code Lecture
- So we'll see you in the next video when we start to restructure this code

### 07-99 Improving Weapon Fire Code

```cpp
void AShooterCharacter::FireButtonPressed()
{
	bFireButtonPressed = true;
	FireWeapon();
}
```

```cpp
void AShooterCharacter::FireWeapon()
{
	if (EquippedWeapon == nullptr) return;
	if (CombatState != ECombatState::ECS_Unoccupied) return;

	if (WeaponHasAmmo())
	{
		PlayFireSound();
		SendBullet();
		PlayGunfireMontage();
		EquippedWeapon->DecrementAmmo();

		StartFireTimer();
	}
}
```

```cpp
void AShooterCharacter::StartFireTimer()
{
	CombatState = ECombatState::ECS_FireTimerInProgress;

	GetWorldTimerManager().SetTimer(
		AutoFireTimer,
		this,
		&AShooterCharacter::AutoFireReset,
		AutomaticFireRate);
}
```

```cpp
void AShooterCharacter::AutoFireReset()
{
	CombatState = ECombatState::ECS_Unoccupied;

	if (WeaponHasAmmo())
	{
		if (bFireButtonPressed)
		{
			FireWeapon();
		}
	}
	else
	{
		// Reload Weapon
	}
}
```

### 07-100 Reload Montage

![montage](https://user-images.githubusercontent.com/80055816/210723001-9bf4baed-ab97-4d70-b81c-4470089779c8.PNG){: width="100%" height="100%"}{: .align-center}

![drag](https://user-images.githubusercontent.com/80055816/210723197-f7f49a63-6655-488f-9f78-15b89b4fb910.PNG){: width="100%" height="100%"}{: .align-center}

![new](https://user-images.githubusercontent.com/80055816/210723784-285a4263-4e2c-457d-8fd6-6c476d334eb4.PNG){: width="100%" height="100%"}{: .align-center}

- The reason we're saying SMG is because this animation is specific to the SMG weapon and we're going to have different sections here with the different animations for the different weapons

![conclude](https://user-images.githubusercontent.com/80055816/210723832-9ec5549e-1a6a-4376-aa2e-f52ee33da8d2.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/210723866-4b7f6d65-8a10-4312-bf2a-a4c59d77d2a2.PNG){: width="100%" height="100%"}{: .align-center}

### 07-101 Reload Lecture
- Remember, if we're not in the unoccupied state, we cannot fire the weapon

### 07-102 The Weapon Type

```cpp
UCLASS()
class SHOOTER_API AWeapon : public AItem
{
	//..

	FORCEINLINE EWeaponType GetWeaponType() const { return WeaponType; }
	
	//..
}
```

### 07-103 Reload Continued

![class](https://user-images.githubusercontent.com/80055816/210818648-deef6cfd-5105-4eb4-9a42-2cb3738e9e27.PNG){: width="100%" height="100%"}{: .align-center}

![hfile](https://user-images.githubusercontent.com/80055816/210830218-3b70162b-d197-4a68-9274-7343e5d533c2.PNG){: width="100%" height="100%"}{: .align-center}

![delete](https://user-images.githubusercontent.com/80055816/210818932-3e4b74f3-dc4b-48a1-9e4c-2ced4a8466aa.PNG){: width="100%" height="100%"}{: .align-center}

### 07-104 Update AmmoMap

```cpp
// 표현식( Ammo + Amount <= MagazineCapacity )이 true가 아니면 에러메시지를 출력한다
void AWeapon::ReloadAmmo(int32 Amount)
{
	checkf(Ammo + Amount <= MagazineCapacity, TEXT("Attempted to reload with more than magazine capacity!"));
	Ammo += Amount;
}
```

```cpp
//..

// 언리얼 Map에서 Add는 Replace를 의미한다
AmmoMap.Add(AmmoType, CarriedAmmo);

//..
```

![notify](https://user-images.githubusercontent.com/80055816/210819133-2c881796-52d5-41b3-a35e-72acd63076b8.PNG){: width="100%" height="100%"}{: .align-center}

### 07-105 Bind Carried Ammo

![bind](https://user-images.githubusercontent.com/80055816/210819338-7123930b-1dfc-40dc-b90b-b03d4c5d068c.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/210819469-91f6cd51-4895-49bf-b0a6-0d39dbb75f77.PNG){: width="100%" height="100%"}{: .align-center}

### 07-106 Bind Weapon Name

![item](https://user-images.githubusercontent.com/80055816/210852062-2ad27b4e-9198-401c-bae1-77105394d228.PNG){: width="100%" height="100%"}{: .align-center}

### 07-107 Move Clip Lecture
- The scene component can be used as a reference point for its transform
- So what we're going to do with this scene component is attach it to our hand
- Then we can simply use that scene component for its transform its location, rotation and scale

### 07-108 Grab and Release Clip

![notify](https://user-images.githubusercontent.com/80055816/210935695-cd0b4f71-9419-40a3-a61a-24fa781569b6.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/210935758-c2d41261-653f-4b89-85af-284b23f3e50d.PNG){: width="100%" height="100%"}{: .align-center}

![track](https://user-images.githubusercontent.com/80055816/210935808-b3bfe638-0789-43ab-a77e-038a76f63541.PNG){: width="100%" height="100%"}{: .align-center}

![bone](https://user-images.githubusercontent.com/80055816/210935848-c4a196b5-a299-4ab9-8a86-b750d66b267d.PNG){: width="100%" height="100%"}{: .align-center}

![hand](https://user-images.githubusercontent.com/80055816/210935882-6608860a-affb-47a9-b87f-20a0536dfcd4.PNG){: width="100%" height="100%"}{: .align-center}

![final](https://user-images.githubusercontent.com/80055816/210935919-aaaef0db-5a8a-4781-bdd6-ab76a77b64e2.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AShooterCharacter::GrabClip()
{
	//..

	//------------------------------------------------------------------------------------------
	// EAttachmentRule::KeepRelative에 대해 설명하면?
	// Keeps current relative transform as the relative transform to the new parent
	// https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/Engine/EAttachmentRule/ 참고
	// -----------------------------------------------------------------------------------------
	FAttachmentTransformRules AttachmentRules(EAttachmentRule::KeepRelative, true);
	HandSceneComponent->AttachToComponent(GetMesh(), AttachmentRules, FName(TEXT("Hand_L")));
	HandSceneComponent->SetWorldTransform(ClipTransform);

	EquippedWeapon->SetMovingClip(true);

	//..
}
```

### 07-109 Weapon AnimBP

![bp](https://user-images.githubusercontent.com/80055816/210947736-58d0ca5b-4e53-4513-95f9-63b15dfd3b45.PNG){: width="100%" height="100%"}{: .align-center}

![create](https://user-images.githubusercontent.com/80055816/210947781-1a884d9c-eaf3-4e3c-845b-7cca5be6a349.PNG){: width="100%" height="100%"}{: .align-center}

![ref](https://user-images.githubusercontent.com/80055816/210947852-df8964b2-c0f7-4e1b-960c-c90689ed3669.PNG){: width="100%" height="100%"}{: .align-center}

![graph](https://user-images.githubusercontent.com/80055816/210949650-6534a504-7380-4124-b822-fa316b7f976a.PNG){: width="100%" height="100%"}{: .align-center}

- Get owning actor is a node that returns an actor That is the actor that owns this animation blueprint

### 07-110 Moving the Clip

![inside](https://user-images.githubusercontent.com/80055816/210959454-57ddf63e-6fe8-4367-974b-bf6051fb233f.PNG){: width="100%" height="100%"}{: .align-center}

![clip](https://user-images.githubusercontent.com/80055816/210959499-e300ec51-b613-4efa-a6ff-573daa44bd46.PNG){: width="100%" height="100%"}{: .align-center}

![remember](https://user-images.githubusercontent.com/80055816/210959558-f0c4c262-fab0-4caa-aec2-826d62b59b88.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/210959602-6ca766cd-0af0-46d8-992f-012887c8a9a4.PNG){: width="100%" height="100%"}{: .align-center}

![selbp](https://user-images.githubusercontent.com/80055816/210959629-b3376202-2ee9-49b0-8856-bd452d1cd407.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
AShooterCharacter::AShooterCharacter() :
{
	//..

	// Now this is a scene component and really we don't even need to attach it to anything
	// Because Grap clip is going to handle the attachment
	// We're going to attach it to our handbone
	HandSceneComponent = CreateDefaultSubobject<USceneComponent>(TEXT("HandSceneComp"));

	//..
}
```

### 07-111 Clip Sounds

![cue](https://user-images.githubusercontent.com/80055816/211035557-ac497055-c52c-402b-b20f-9a956bb2f26c.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/211035640-c0e4134e-8cf6-4ba5-9286-7f7dc65d6e06.PNG){: width="100%" height="100%"}{: .align-center}

![select](https://user-images.githubusercontent.com/80055816/211035712-12c4e3c0-dd5d-44e1-9345-717585df3823.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/211035777-63ebccd4-beb1-46a6-aa43-03a73e863e67.PNG){: width="100%" height="100%"}{: .align-center}

### 07-112 Pickup Sounds

```cpp
//..

// 사운드 재생 함수
UGameplayStatics::PlaySound2D(this, Item->GetEquipSound());

//..
```

<br>

## Chapter 8 Advanced Movement

### 08-113 Rotate Root Bone
- The root is the parent of the skeleton hierarchy So if we rotate the root, the body will follow

```cpp
void UShooterAnimInstance::TurnInPlace()
{
	if (ShooterCharacter == nullptr) return;
	if (Speed > 0)
	{
		// Don't want to turn in place; Character is moving
	}
	else
	{
		CharacterYawLastFrame = CharacterYaw;
		CharacterYaw = ShooterCharacter->GetActorRotation().Yaw;
		const float YawDelta{ CharacterYaw - CharacterYawLastFrame };

		RootYawOffset -= YawDelta;

		// If we use a different number for each message for the key, 
		// Then we can print multiple messages on the screen at the same time
		if (GEngine) GEngine->AddOnScreenDebugMessage(
			1,
			-1,
			FColor::Blue,
			FString::Printf(TEXT("CharacterYaw: %f"), CharacterYaw));
		if (GEngine) GEngine->AddOnScreenDebugMessage(
			2,
			-1,
			FColor::Red,
			FString::Printf(TEXT("RootYawOffset: %f"), RootYawOffset));
	}
}
```

- And so we're going to use Yaw Offset to rotate our bone back toward the forward direction

![expose](https://user-images.githubusercontent.com/80055816/211035866-67be219c-207f-4bc2-a4ff-1890589417b4.PNG){: width="100%" height="100%"}{: .align-center}

![conclude](https://user-images.githubusercontent.com/80055816/211038604-e5f76322-31fd-4f79-92c6-79068b107666.PNG){: width="100%" height="100%"}{: .align-center}

- So this is how we use our route, yaw offset to rotate the bone back once we've rotated our camera for each frame

### 08-114 Turn In Place Animations

![copy](https://user-images.githubusercontent.com/80055816/211054454-27af812c-f82d-45ac-9bff-65fb13691eb5.PNG){: width="100%" height="100%"}{: .align-center}

![remove](https://user-images.githubusercontent.com/80055816/211054510-36f66d3b-3ca9-48d9-90e9-5bcbfe7a51d8.PNG){: width="100%" height="100%"}{: .align-center}

![upper](https://user-images.githubusercontent.com/80055816/211054552-b6fb2023-06ac-41be-bcab-43644140e6c7.PNG){: width="100%" height="100%"}{: .align-center}

![rule](https://user-images.githubusercontent.com/80055816/211054610-cef7a87e-3ca1-4290-81e2-db9c951e369c.PNG){: width="100%" height="100%"}{: .align-center}

![rrule](https://user-images.githubusercontent.com/80055816/211054660-036bdc06-7620-4fd1-8b40-2b65691c6187.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/211054706-ef434b16-d8f3-421f-9229-c2f186aea375.PNG){: width="100%" height="100%"}{: .align-center}

![loop](https://user-images.githubusercontent.com/80055816/211054773-6c9e875e-fc86-4e2f-9adc-ed5c3447bc53.PNG){: width="100%" height="100%"}{: .align-center}

- We actually haven't programmed the ability for the bone to rotate back towards our direction of movement All we're doing is playing the animation

### 08-115 Animation Curves

![curve](https://user-images.githubusercontent.com/80055816/211069552-134ac7ec-8285-4b52-ac31-3451dc1fa9d5.PNG){: width="100%" height="100%"}{: .align-center}

![graph](https://user-images.githubusercontent.com/80055816/211069654-f6d40fcd-59c9-4680-afba-e996dc160b43.PNG){: width="100%" height="100%"}{: .align-center}

![meta](https://user-images.githubusercontent.com/80055816/211069703-928eaf1c-549d-4588-b91d-5664ba2c16a6.PNG){: width="100%" height="100%"}{: .align-center}

![turn](https://user-images.githubusercontent.com/80055816/211069759-41497a74-2db5-4b60-9417-4e6babce27ed.PNG){: width="100%" height="100%"}{: .align-center}

![reuse](https://user-images.githubusercontent.com/80055816/211069804-56fb1d1c-7dc9-46ea-afe1-898ebe71f6df.PNG){: width="100%" height="100%"}{: .align-center}

### 08-116 Turn In Place Using Curve Values

```cpp
void UShooterAnimInstance::TurnInPlace()
{
	if (ShooterCharacter == nullptr) return;
	if (Speed > 0)
	{
		// Don't want to turn in place; Character is moving
		RootYawOffset = 0.f;
		CharacterYaw = ShooterCharacter->GetActorRotation().Yaw;
		CharacterYawLastFrame = CharacterYaw;
		RotationCurveLastFrame = 0.f;
		RotationCurve = 0.f;
	}
	else
	{
		CharacterYawLastFrame = CharacterYaw;
		CharacterYaw = ShooterCharacter->GetActorRotation().Yaw;
		const float YawDelta{ CharacterYaw - CharacterYawLastFrame };

		// Root Yaw Offset, updated and clamped to [-180, 180]
		RootYawOffset = UKismetMathLibrary::NormalizeAxis(RootYawOffset - YawDelta);

		// if turning than? the value is 1.0
		// if not than? the value is 0.0
		const float Turning{ GetCurveValue(TEXT("Turning")) };
		if (Turning > 0)
		{
			RotationCurveLastFrame = RotationCurve;
			RotationCurve = GetCurveValue(TEXT("Rotation"));
			const float DeltaRotation{ RotationCurve - RotationCurveLastFrame };

			// if RootYawOffset > 0 than? Turning Left
			// if RootYawOffset < 0 than? Turning Right
			RootYawOffset > 0 ? RootYawOffset -= DeltaRotation : RootYawOffset += DeltaRotation;

			const float ABSRootYawOffset{ FMath::Abs(RootYawOffset) };
			if (ABSRootYawOffset > 90.f)
			{
				const float YawExcess{ ABSRootYawOffset - 90.f };
				RootYawOffset > 0 ? RootYawOffset -= YawExcess : RootYawOffset += YawExcess;
			}
		}

		if (GEngine) GEngine->AddOnScreenDebugMessage(1, -1, FColor::Cyan, FString::Printf(TEXT("RootYawOffset: %f"), RootYawOffset));
	}
}
```

### 08-117 Hip Aim Offset

![offset](https://user-images.githubusercontent.com/80055816/211155215-689a6ba8-7707-425e-be71-081168b7b02c.PNG){: width="100%" height="100%"}{: .align-center}

![ao](https://user-images.githubusercontent.com/80055816/211163870-35168590-a3c4-423d-82dc-5fbea12c8be5.PNG){: width="100%" height="100%"}{: .align-center}

![down](https://user-images.githubusercontent.com/80055816/211155233-1eed43f6-2559-4321-84ed-4ef7daa63adc.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/211155246-70174cfd-856d-4686-ba79-314af49ea8af.PNG){: width="100%" height="100%"}{: .align-center}

![connect](https://user-images.githubusercontent.com/80055816/211155265-7437778d-6979-4126-a0e0-2d497429deb2.PNG){: width="100%" height="100%"}{: .align-center}

![pitch](https://user-images.githubusercontent.com/80055816/211155271-4159324d-69c9-4026-881a-4dee1feae3d1.PNG){: width="100%" height="100%"}{: .align-center}

![reload](https://user-images.githubusercontent.com/80055816/211155281-61537562-0806-4e38-bdcf-90a2f6cd50e0.PNG){: width="100%" height="100%"}{: .align-center}

![clear](https://user-images.githubusercontent.com/80055816/211155295-cf229d2c-495d-4e65-ba0a-51e716741d26.PNG){: width="100%" height="100%"}{: .align-center}

### 08-118 Aiming Aim Offset

![one](https://user-images.githubusercontent.com/80055816/211163286-f83adc76-f53e-4717-ab48-4b0d9897f0c7.PNG){: width="100%" height="100%"}{: .align-center}

![aim](https://user-images.githubusercontent.com/80055816/211163296-2f381267-d428-4b25-98f5-ca0a90c0607f.PNG){: width="100%" height="100%"}{: .align-center}

![add](https://user-images.githubusercontent.com/80055816/211163307-bce3b149-2579-41ef-919b-ca24519185fb.PNG){: width="100%" height="100%"}{: .align-center}

![many](https://user-images.githubusercontent.com/80055816/211163348-7aaaee07-7c0e-4a68-befb-7d0f0639c474.PNG){: width="100%" height="100%"}{: .align-center}

![air](https://user-images.githubusercontent.com/80055816/211163358-819e5845-d648-4745-a6ef-a1aa3dea9a69.PNG){: width="100%" height="100%"}{: .align-center}

### 08-119 Lean

```cpp
void UShooterAnimInstance::Lean(float DeltaTime)
{
	//..

	// Explain below code
	// This gives us a measure of how quickly we're turning
	// And this is going to handle any sin(math) changes
	const FRotator Delta{ UKismetMathLibrary::NormalizedDeltaRotator(CharacterRotation, CharacterRotationLastFrame) };
	const float Target{ Delta.Yaw / DeltaTime };

	//..
}
```

### 08-120 Lean Blendspace

![space](https://user-images.githubusercontent.com/80055816/211194907-1951d564-3b57-47ac-afa5-a41dc1f0c855.PNG){: width="100%" height="100%"}{: .align-center}

![input](https://user-images.githubusercontent.com/80055816/211194930-05d0e4e4-6970-4504-aec4-4603f99f2f4a.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/211194942-bfcf61bd-ff28-49e4-90f8-f7265983daca.PNG){: width="100%" height="100%"}{: .align-center}

### 08-121 Crouching Setup

![crouch](https://user-images.githubusercontent.com/80055816/211194876-4a33c942-746a-4593-a0ce-021c46270650.PNG){: width="100%" height="100%"}{: .align-center}

### 08-122 Crouching Animations

![dup](https://user-images.githubusercontent.com/80055816/211197423-c35b84d2-f61a-4723-9eb1-1b76cfc193d8.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/211197438-ceb7dc7f-e321-45d7-b134-20f15e0bc520.PNG){: width="100%" height="100%"}{: .align-center}

![upper](https://user-images.githubusercontent.com/80055816/211197458-f1a9e701-b3d6-4e4f-9472-02c38d49bd7b.PNG){: width="100%" height="100%"}{: .align-center}

### 08-123 Crouching AnimBP

![start](https://user-images.githubusercontent.com/80055816/211200848-1582b280-7813-45ea-a137-5d1e7f558997.PNG){: width="100%" height="100%"}{: .align-center}

![insert](https://user-images.githubusercontent.com/80055816/211200862-a1128c8b-29e4-4f43-af50-503a8da85d90.PNG){: width="100%" height="100%"}{: .align-center}

![rule](https://user-images.githubusercontent.com/80055816/211200886-d1cfd419-3544-4312-85ac-71e7bab091c9.PNG){: width="100%" height="100%"}{: .align-center}

![check](https://user-images.githubusercontent.com/80055816/211200904-20713d50-e807-47dc-aebe-a88255dbbda4.PNG){: width="100%" height="100%"}{: .align-center}

![rulee](https://user-images.githubusercontent.com/80055816/211200919-f2f8ffa5-358e-4b80-8f21-3e140189e003.PNG){: width="100%" height="100%"}{: .align-center}

![final](https://user-images.githubusercontent.com/80055816/211200932-c138b732-b946-414c-a942-0fe54bc539eb.PNG){: width="100%" height="100%"}{: .align-center}

### 08-124 Crouching Turn Animations

![look](https://user-images.githubusercontent.com/80055816/211208175-d4ab2c47-624b-4d75-8ae0-a3b751962b8e.PNG){: width="100%" height="100%"}{: .align-center}

![down](https://user-images.githubusercontent.com/80055816/211208191-b277f173-94d0-412d-9f46-2f27a9c4678a.PNG){: width="100%" height="100%"}{: .align-center}

![noskin](https://user-images.githubusercontent.com/80055816/211208201-2846acae-e728-4d65-841d-0368153faaa0.PNG){: width="100%" height="100%"}{: .align-center}

![first](https://user-images.githubusercontent.com/80055816/211208677-e9608fdb-127a-4714-b394-b661ba16506d.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/211208213-15464b68-b592-4bd6-973c-32058769a7b8.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/211208235-681a0fe4-ba8c-4fa3-8c30-1f719764c5eb.PNG){: width="100%" height="100%"}{: .align-center}

### 08-125 Retargeting Anims with Different Skeletons

![match](https://user-images.githubusercontent.com/80055816/211208252-5f69b304-6a62-4ad9-9f11-ce62bea3ed84.PNG){: width="100%" height="100%"}{: .align-center}

![view](https://user-images.githubusercontent.com/80055816/211208261-c93d18b5-0759-4d1b-ac28-89ef4b75dcc3.PNG){: width="100%" height="100%"}{: .align-center}

![too](https://user-images.githubusercontent.com/80055816/211208273-a7839a23-8e3d-4cbd-b2e7-8718aeb7d0ba.PNG){: width="100%" height="100%"}{: .align-center}

![use](https://user-images.githubusercontent.com/80055816/211208284-7a69c903-edd0-42a3-aa27-a75b8fc0d312.PNG){: width="100%" height="100%"}{: .align-center}

![apply](https://user-images.githubusercontent.com/80055816/211208300-f14a4d7d-a1cb-447c-8181-505223bac7c1.PNG){: width="100%" height="100%"}{: .align-center}

![re](https://user-images.githubusercontent.com/80055816/211208322-c8161164-fb3a-4f1d-bc2f-f939f526dcff.PNG){: width="100%" height="100%"}{: .align-center}

![temp](https://user-images.githubusercontent.com/80055816/211208334-f1dcc422-c0dd-4a10-a79c-e8df235957a9.PNG){: width="100%" height="100%"}{: .align-center}

### 08-126 Crouch Turn In Place AnimBP

![match](https://user-images.githubusercontent.com/80055816/211268589-5df7df03-b573-4d91-8014-2162e9439490.PNG){: width="100%" height="100%"}{: .align-center}

![spin](https://user-images.githubusercontent.com/80055816/211270630-96904a1a-d74b-4a96-89fa-a6edc28f493b.PNG){: width="100%" height="100%"}{: .align-center}

![upper](https://user-images.githubusercontent.com/80055816/211269151-0539499b-a0b3-4edd-9b05-c4a41916308b.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/211269195-e9a4cf36-e932-4d86-a766-e9c6495570fa.PNG){: width="100%" height="100%"}{: .align-center}

![ani](https://user-images.githubusercontent.com/80055816/211269237-7005e56a-e227-4491-bbb1-34e5d2aa7535.PNG){: width="100%" height="100%"}{: .align-center}

![curve](https://user-images.githubusercontent.com/80055816/211269277-baacfdc1-8fa2-4b9a-8776-7f36675d3e63.PNG){: width="100%" height="100%"}{: .align-center}

![curvegood](https://user-images.githubusercontent.com/80055816/211269319-7ec58322-94a9-434d-be96-44fefbd67cb4.PNG){: width="100%" height="100%"}{: .align-center}

![again](https://user-images.githubusercontent.com/80055816/211269370-1da99c5e-753a-4e20-a38f-54585bdc4e23.PNG){: width="100%" height="100%"}{: .align-center}

![speed](https://user-images.githubusercontent.com/80055816/211269410-885e9040-437b-42be-83bd-bc57dde678b6.PNG){: width="100%" height="100%"}{: .align-center}

- layered blend per bone에 대해 설명하면? A value of 0.0 means the Additive pose is not added to the Base input pose at all, while a value of 1.0 means the Additive pose is added fully to the Base input pose ([**참고**](https://docs.unrealengine.com/4.27/en-US/AnimatingObjects/SkeletalMeshAnimation/NodeReference/Blend/))

### 08-127 Crouch Recoil Weight

![weight](https://user-images.githubusercontent.com/80055816/211308406-8f0d672b-8e98-4c02-a63a-571c4d4511a0.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void UShooterAnimInstance::TurnInPlace()
{
	if (bTurningInPlace)
	{
		if (bReloading)
		{
			RecoilWeight = 1.f;
		}
		else
		{
			RecoilWeight = 0.f;
		}
	}
}
```

### 08-128 Crouch Walking Blendspace

![crouch](https://user-images.githubusercontent.com/80055816/211314245-580e8052-830f-4241-8048-207023bf91e9.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/211314309-f85bae5d-bd2f-40a4-9112-af31500786cb.PNG){: width="100%" height="100%"}{: .align-center}

![insert](https://user-images.githubusercontent.com/80055816/211314422-a1fc7e90-7c54-4f32-90c3-10c57d7861fd.PNG){: width="100%" height="100%"}{: .align-center}

### 08-129 Crouch Walking

![bs](https://user-images.githubusercontent.com/80055816/211319248-e8d6e1cd-92df-4e75-ad28-835254a99eeb.PNG){: width="100%" height="100%"}{: .align-center}

![rule](https://user-images.githubusercontent.com/80055816/211319298-42194331-b68d-4ea9-87c5-4e188693f3bb.PNG){: width="100%" height="100%"}{: .align-center}

![nrule](https://user-images.githubusercontent.com/80055816/211319338-649fad54-8b8a-444a-9408-cdaf564a37b1.PNG){: width="100%" height="100%"}{: .align-center}

![time](https://user-images.githubusercontent.com/80055816/211319379-b1eadb4f-c387-4499-bb59-7d78b15219b0.PNG){: width="100%" height="100%"}{: .align-center}

![pick](https://user-images.githubusercontent.com/80055816/211341989-1328a3fb-c38c-4a60-9022-fd533bf1c4ce.PNG){: width="100%" height="100%"}{: .align-center}

### 08-130 Crouch Movement Speed and Jump

```cpp
void AShooterCharacter::CrouchButtonPressed()
{
	if (!GetCharacterMovement()->IsFalling())
	{
		bCrouching = !bCrouching;
	}
	if (bCrouching)
	{
		GetCharacterMovement()->MaxWalkSpeed = CrouchMovementSpeed;
	}
	else
	{
		GetCharacterMovement()->MaxWalkSpeed = BaseMovementSpeed;
	}
}
```

![remove](https://user-images.githubusercontent.com/80055816/211329317-404db5df-07af-4035-82be-4ee479a1888a.PNG){: width="100%" height="100%"}{: .align-center}

### 08-131 Interp Capsule Half Height

![pause](https://user-images.githubusercontent.com/80055816/211576680-d31b56a3-b4c2-4c0f-a530-35f272634375.PNG){: width="100%" height="100%"}{: .align-center}

![low](https://user-images.githubusercontent.com/80055816/211576761-ab4d7c9f-7058-4011-913a-9a41d6395100.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AShooterCharacter::InterpCapsuleHalfHeight(float DeltaTime)
{
	// 아래 코드가 필요한 이유는?
	// 캡슐 크기가 작아지는 만큼 메시를 들어올리기 위해
	const float DeltaCapsuleHalfHeight{ InterpHalfHeight - GetCapsuleComponent()->GetScaledCapsuleHalfHeight() };
	const FVector MeshOffset{ 0.f, 0.f, -DeltaCapsuleHalfHeight };
	GetMesh()->AddLocalOffset(MeshOffset);
}
```

```cpp
void AShooterCharacter::CrouchButtonPressed()
{
	// 아래 코드는 무엇을 하는 코드인가?
	// 상태에 따라 마찰계수를 조절하는 코드
	if (!GetCharacterMovement()->IsFalling())
	{
		bCrouching = !bCrouching;
	}
	if (bCrouching)
	{
		GetCharacterMovement()->MaxWalkSpeed = CrouchMovementSpeed;
		GetCharacterMovement()->GroundFriction = CrouchingGroundFriction;
	}
	else
	{
		GetCharacterMovement()->MaxWalkSpeed = BaseMovementSpeed;
		GetCharacterMovement()->GroundFriction = BaseGroundFriction;
	}
}
```

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
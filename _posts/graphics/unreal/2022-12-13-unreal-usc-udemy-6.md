---
title:  "Unreal Engine C++ The Ultimate Shooter Course (6)"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2023-02-02
---

## Chapter 15 AI and Behavior Trees

### 15-272 Unreal Engine AI

- AI Character, Use AI Controller
- Behavior Tree, Makes decisions
- Blackboard, Stores data for the Behavior Tree
- AI Controller, Moves the Character
- Navigation Mesh, Enables AI navigation
- Now, remember, the sequence( In Behavior Tree Nodes ) will execute the child tasks until one of them fails

### 15-273 Adding AI Modules

```cpp
// Shooter.Build.cs

public class Shooter : ModuleRules
{
    public Shooter(ReadOnlyTargetRules Target) : base(Target)
    {
        PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;

        // Add AI Module
        PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "UMG", "PhysicsCore", "NavigationSystem", "AIModule" });

        PrivateDependencyModuleNames.AddRange(new string[] { });

        // Uncomment if you are using Slate UI
        // PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });

        // Uncomment if you are using online features
        // PrivateDependencyModuleNames.Add("OnlineSubsystem");

        // To include OnlineSubsystemSteam, add it to the plugins section in your uproject file with the Enabled attribute set to true
    }
}
```

```cpp
class SHOOTER_API AEnemy : public ACharacter, public IBulletHitInterface
{
    /** Behavior tree for the AI Character */
	UPROPERTY(EditAnywhere, Category = "Behavior Tree", meta = (AllowPrivateAccess = "true"))
	class UBehaviorTree* BehaviorTree;
}
```

### 15-274 AI Controller

![ai](https://user-images.githubusercontent.com/80055816/216330534-1d7403ef-83a9-4b93-ab85-3c629c310713.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
class SHOOTER_API AEnemy : public ACharacter, public IBulletHitInterface
{
    //..

    FORCEINLINE UBehaviorTree* GetBehaviorTree() const { return BehaviorTree; }

    //..
}
```

```cpp
void AEnemyController::OnPossess(APawn* InPawn)
{
	Super::OnPossess(InPawn);
	if (InPawn == nullptr) return;

	AEnemy* Enemy = Cast<AEnemy>(InPawn);
	if (Enemy)
	{
		if (Enemy->GetBehaviorTree())
		{
			BlackboardComponent->InitializeBlackboard(*(Enemy->GetBehaviorTree()->BlackboardAsset));
		}
	}
}
```

### 15-275 Creating a Blackboard and Behavior Tree

![bp](https://user-images.githubusercontent.com/80055816/216347229-d5fab43c-4ee7-4372-8c46-55bde29c295f.PNG){: width="100%" height="100%"}{: .align-center}

![board](https://user-images.githubusercontent.com/80055816/216347298-feed2c0f-0bbb-4499-9e06-f51a2105b22b.PNG){: width="100%" height="100%"}{: .align-center}

![explain](https://user-images.githubusercontent.com/80055816/216347383-5f4fa98d-9aba-44a0-b14a-9c97fb6e0097.PNG){: width="100%" height="100%"}{: .align-center}

![btree](https://user-images.githubusercontent.com/80055816/216347485-613bbf70-8194-4f01-83ff-174fd496141e.PNG){: width="100%" height="100%"}{: .align-center}

![default](https://user-images.githubusercontent.com/80055816/216347554-66183a64-e69d-42d6-bd78-b44422844806.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/216347659-3c1bba21-4131-4678-b3db-e82671ff0389.PNG){: width="100%" height="100%"}{: .align-center}

### 15-276 Patrol Point

```cpp
void AEnemy::BeginPlay()
{
	Super::BeginPlay();

	GetMesh()->SetCollisionResponseToChannel(ECollisionChannel::ECC_Visibility, ECollisionResponse::ECR_Block);
	
	// Converted from local space to worldspace
	const FVector WorldPatrolPoint = UKismetMathLibrary::TransformLocation(
		GetActorTransform(),
		PatrolPoint);

	DrawDebugSphere(
		GetWorld(),
		WorldPatrolPoint,
		25.f,
		12,
		FColor::Red,
		true
	);
}
```

![pat](https://user-images.githubusercontent.com/80055816/216379095-63ad9990-e2ab-4cb3-b2a1-26afa4fe632a.PNG){: width="100%" height="100%"}{: .align-center}

### 15-277 Move to Task

![add](https://user-images.githubusercontent.com/80055816/216398444-bc317113-e9bc-4458-ac62-81dfb5d39e8a.PNG){: width="100%" height="100%"}{: .align-center}

![save](https://user-images.githubusercontent.com/80055816/216398506-cd7d00e0-dc84-4ecd-b3f9-661ac528cf63.PNG){: width="100%" height="100%"}{: .align-center}

![cont](https://user-images.githubusercontent.com/80055816/216398573-2f807424-45cb-4122-8579-3817d15bcbd3.PNG){: width="100%" height="100%"}{: .align-center}

![pkey](https://user-images.githubusercontent.com/80055816/216398658-856c671c-1210-476f-9892-0ea62d411959.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::BeginPlay()
{
	//..

	if (EnemyController)
	{
		EnemyController->GetBlackboardComponent()->SetValueAsVector(
			TEXT("PatrolPoint"),
			WorldPatrolPoint);

		EnemyController->RunBehaviorTree(BehaviorTree);
	}
}
```

### 15-278 Enemy Run Animation

![blend](https://user-images.githubusercontent.com/80055816/216407348-85e21d7f-30f1-4648-9a72-698d2b5f6263.PNG){: width="100%" height="100%"}{: .align-center}

![update](https://user-images.githubusercontent.com/80055816/216407457-eef4eb35-3511-43b5-9e70-fc92f8d0b084.PNG){: width="100%" height="100%"}{: .align-center}

![space](https://user-images.githubusercontent.com/80055816/216407591-dd67c856-6168-471a-a6ed-9f2cc333fa75.PNG){: width="100%" height="100%"}{: .align-center}

### 15-279 Second Patrol Point

![next](https://user-images.githubusercontent.com/80055816/216429534-937f4ccb-4fdc-4123-844d-027fa7842037.PNG){: width="100%" height="100%"}{: .align-center}

![two](https://user-images.githubusercontent.com/80055816/216429591-2e92906b-128c-4c96-80c9-573ee7a8ff5f.PNG){: width="100%" height="100%"}{: .align-center}

### 15-280 Wait Task

![wait](https://user-images.githubusercontent.com/80055816/216525273-a682fd72-abe3-4008-b81c-b64d5083fc17.PNG){: width="100%" height="100%"}{: .align-center}

### 15-281 Agro Sphere

![obj](https://user-images.githubusercontent.com/80055816/216538124-64360edd-dc2d-4790-a34c-2b0d4f949ad7.PNG){: width="100%" height="100%"}{: .align-center}

![agro](https://user-images.githubusercontent.com/80055816/216538239-2646394f-add7-40d3-b443-eb1119a8df8e.PNG){: width="100%" height="100%"}{: .align-center}

![display](https://user-images.githubusercontent.com/80055816/216538283-d5c03eeb-01db-437b-b00b-2b4c6a1d7410.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::AgroSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	if (OtherActor == nullptr) return;

	auto Character = Cast<AShooterCharacter>(OtherActor);
	if (Character)
	{
		// Set the value of the Target Blackboard Key
		EnemyController->GetBlackboardComponent()->SetValueAsObject(
			TEXT("Target"),
			Character);
	}
}
```

### 15-282 Move To Actor

![black](https://user-images.githubusercontent.com/80055816/216542337-79dd16bb-4e39-420c-a8f7-4a83783f5006.PNG){: width="100%" height="100%"}{: .align-center}

### 15-283 Stun Enemy

![finish](https://user-images.githubusercontent.com/80055816/216571276-4fcc2ee1-91fa-4dbc-b7bf-2411d6c7db73.PNG){: width="100%" height="100%"}{: .align-center}

![anim](https://user-images.githubusercontent.com/80055816/216572092-0b520139-1894-45dd-9054-2128efa6dadd.PNG){: width="100%" height="100%"}{: .align-center}

![stun](https://user-images.githubusercontent.com/80055816/216572142-42097943-5a0d-466c-921e-ae0f1d80e025.PNG){: width="100%" height="100%"}{: .align-center}

### 15-284 Stunned Blackboard Decorator

![save](https://user-images.githubusercontent.com/80055816/216592044-dc129ee3-e244-49f4-8bf8-c02a84d7233a.PNG){: width="100%" height="100%"}{: .align-center}

![check](https://user-images.githubusercontent.com/80055816/216592120-da97069c-7eda-4ae8-be44-617184192a12.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::SetStunned(bool Stunned)
{
	bStunned = Stunned;

	if (EnemyController)
	{
		EnemyController->GetBlackboardComponent()->SetValueAsBool(
			TEXT("Stunned"),
			Stunned);
	}
}
```

![fun](https://user-images.githubusercontent.com/80055816/216625678-336a7f53-2c64-4282-9c30-5c4c70a5382d.PNG){: width="100%" height="100%"}{: .align-center}

![board](https://user-images.githubusercontent.com/80055816/216592167-7630d5b8-87d4-4cf1-b177-616dfeb2c638.PNG){: width="100%" height="100%"}{: .align-center}

![value](https://user-images.githubusercontent.com/80055816/216592212-a4f86305-ba0d-47a1-b1d4-4730a7ade56e.PNG){: width="100%" height="100%"}{: .align-center}

- A decorator node behaves like a conditional It's like an F check that says if a particular condition is true, then allow the node to execute, otherwise prevent the node from executing

### 15-285 Selector Node
- The selector will continue executing children until one of them succeeds

![blackboard](https://user-images.githubusercontent.com/80055816/216632379-6644cba6-5844-44c5-8604-98d525389379.PNG){: width="100%" height="100%"}{: .align-center}

![chose](https://user-images.githubusercontent.com/80055816/216632707-23ee4fff-2d38-46e6-bc5f-196c0bfc71db.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/216632769-e6249592-b452-4bde-894c-a4a4761da853.PNG){: width="100%" height="100%"}{: .align-center}

### 15-286 In Attack Range

![range](https://user-images.githubusercontent.com/80055816/216649263-61286824-56e4-4c5d-9f72-648e52bf94ab.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::CombatRangeOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	if (OtherActor == nullptr) return;
	auto ShooterCharacter = Cast<AShooterCharacter>(OtherActor);
	if (ShooterCharacter)
	{
		bInAttackRange = true;
		if (EnemyController)
		{
			EnemyController->GetBlackboardComponent()->SetValueAsBool(
				TEXT("InAttackRange"),
				true
			);
		}
	}
}
```

```cpp
void AEnemy::CombatRangeEndOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)
{
	if (OtherActor == nullptr) return;
	auto ShooterCharacter = Cast<AShooterCharacter>(OtherActor);
	if (ShooterCharacter)
	{
		bInAttackRange = false;
		if (EnemyController)
		{
			EnemyController->GetBlackboardComponent()->SetValueAsBool(
				TEXT("InAttackRange"),
				false
			);
		}
	}
}
```

![attack](https://user-images.githubusercontent.com/80055816/216649551-bd050d89-c73c-42ff-be73-2c7d2d722e38.PNG){: width="100%" height="100%"}{: .align-center}

![true](https://user-images.githubusercontent.com/80055816/216649613-5dae00f1-7719-47a9-8e91-429697e3d792.PNG){: width="100%" height="100%"}{: .align-center}

### 15-287 Enemy Attack Montage

![mon](https://user-images.githubusercontent.com/80055816/216663064-a0231f6d-3048-4cb8-9e2b-5a8241d3ddef.PNG){: width="100%" height="100%"}{: .align-center}

![slot](https://user-images.githubusercontent.com/80055816/216663135-9040f881-19f0-451f-b573-ab435d7a9ba2.PNG){: width="100%" height="100%"}{: .align-center}

![nodegood](https://user-images.githubusercontent.com/80055816/216663190-6c0b9121-042a-4f93-9164-a691237790bc.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::PlayAttackMontage(FName Section, float PlayRate)
{
	UAnimInstance* AnimInstance = GetMesh()->GetAnimInstance();
	if (AnimInstance && AttackMontage)
	{
		AnimInstance->Montage_Play(AttackMontage);
		AnimInstance->Montage_JumpToSection(Section, AttackMontage);
	}
}
```

### 15-288 Get Attack Section Name

```cpp
FName AEnemy::GetAttackSectionName()
{
	FName SectionName;
	const int32 Section{ FMath::RandRange(1, 4) };
	switch (Section)
	{
	case 1:
		SectionName = AttackLFast;
		break;
	case 2:
		SectionName = AttackRFast;
		break;
	case 3:
		SectionName = AttackL;
		break;
	case 4:
		SectionName = AttackR;
		break;
	}
	return SectionName;
}
```

### 15-289 Custom Behavior Tree Task
- Now, remember, a sequence node will execute its children from left to right until one of them fails

![task](https://user-images.githubusercontent.com/80055816/216775594-ed19241d-e139-4f0a-9348-2910b549d3ff.PNG){: width="100%" height="100%"}{: .align-center}

![section](https://user-images.githubusercontent.com/80055816/216775616-54f9a480-7984-4571-86dc-077faad3871b.PNG){: width="100%" height="100%"}{: .align-center}

![sel](https://user-images.githubusercontent.com/80055816/216775642-0165dfea-8d0b-4fed-952e-047f250c923e.PNG){: width="100%" height="100%"}{: .align-center}

![know](https://user-images.githubusercontent.com/80055816/216775674-b7744286-67af-43ce-95fc-7fe76a1ff35f.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::BeginPlay()
{
	//..

	// Ignore the camera for Mesh and Capsule
	GetMesh()->SetCollisionResponseToChannel(
		ECollisionChannel::ECC_Camera,
		ECollisionResponse::ECR_Ignore);
	GetCapsuleComponent()->SetCollisionResponseToChannel(
		ECollisionChannel::ECC_Camera,
		ECollisionResponse::ECR_Ignore
	);

	//..
}
```

### 15-290 Weapon Collision Volumes

![socket](https://user-images.githubusercontent.com/80055816/216778593-5a4bc4ab-608b-4ac0-8cfa-128fa9873620.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::BeginPlay()
{
	//..

	// Bind functions to overlap events for weapon boxes
	LeftWeaponCollision->OnComponentBeginOverlap.AddDynamic(
		this,
		&AEnemy::OnLeftWeaponOverlap);
	RightWeaponCollision->OnComponentBeginOverlap.AddDynamic(
		this,
		&AEnemy::OnRightWeaponOverlap);

	// Set collision presets for weapon boxes
	LeftWeaponCollision->SetCollisionEnabled(ECollisionEnabled::NoCollision);
	LeftWeaponCollision->SetCollisionObjectType(ECollisionChannel::ECC_WorldDynamic);
	LeftWeaponCollision->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	LeftWeaponCollision->SetCollisionResponseToChannel(
		ECollisionChannel::ECC_Pawn,
		ECollisionResponse::ECR_Overlap);
	RightWeaponCollision->SetCollisionEnabled(ECollisionEnabled::NoCollision);
	RightWeaponCollision->SetCollisionObjectType(ECollisionChannel::ECC_WorldDynamic);
	RightWeaponCollision->SetCollisionResponseToAllChannels(ECollisionResponse::ECR_Ignore);
	RightWeaponCollision->SetCollisionResponseToChannel(
		ECollisionChannel::ECC_Pawn,
		ECollisionResponse::ECR_Overlap);

	//..
}
```

![pause](https://user-images.githubusercontent.com/80055816/216778621-763f2d4b-5e5e-4377-ad7e-8178b3d18b2a.PNG){: width="100%" height="100%"}{: .align-center}

### 15-291 Activate and Deactivate Collision

![active](https://user-images.githubusercontent.com/80055816/216780769-ddd99fa8-b838-4aff-88a4-5390d3391be1.PNG){: width="100%" height="100%"}{: .align-center}

![de](https://user-images.githubusercontent.com/80055816/216780782-af2007a7-f6b5-4609-a370-e141f892a97f.PNG){: width="100%" height="100%"}{: .align-center}

### 15-292 Enemy Damage

```cpp
void AEnemy::DoDamage(AActor* Victim)
{
	if (Victim == nullptr) return;
	auto Character = Cast<AShooterCharacter>(Victim);
	if (Character)
	{
		UGameplayStatics::ApplyDamage(
			Character,
			BaseDamage,
			EnemyController,
			this,
			UDamageType::StaticClass()
		);
	}
}
```

```cpp
float AShooterCharacter::TakeDamage(float DamageAmount, FDamageEvent const& DamageEvent, AController* EventInstigator, AActor* DamageCauser)
{
	if (Health - DamageAmount <= 0.f)
	{
		Health = 0.f;
	}
	else
	{
		Health -= DamageAmount;
	}
	return DamageAmount;
}
```

### 15-293 Character Health Bar

![bp](https://user-images.githubusercontent.com/80055816/216835469-fd899790-8bb9-4a94-b786-af648cf0d04c.PNG){: width="100%" height="100%"}{: .align-center}

![bind](https://user-images.githubusercontent.com/80055816/216835499-0f3dd1de-40f1-4169-8531-6b3e8b7edc65.PNG){: width="100%" height="100%"}{: .align-center}

![vari](https://user-images.githubusercontent.com/80055816/216835509-f1d828bf-316a-4572-bfb8-96c970f76289.PNG){: width="100%" height="100%"}{: .align-center}

![hp](https://user-images.githubusercontent.com/80055816/216835517-07166244-e841-47b4-8442-f05f43794704.PNG){: width="100%" height="100%"}{: .align-center}

![health](https://user-images.githubusercontent.com/80055816/216835533-f5c249b2-c9b4-4114-9d62-7d57cde0c055.PNG){: width="100%" height="100%"}{: .align-center}

![tint](https://user-images.githubusercontent.com/80055816/216835543-017afd6f-4ce4-4c53-94fd-15dc08126475.PNG){: width="100%" height="100%"}{: .align-center}

![radi](https://user-images.githubusercontent.com/80055816/216835556-623cfb75-7fe4-4ed5-8f57-af7ba5792f68.PNG){: width="100%" height="100%"}{: .align-center}

### 15-294 Enemy Weapon Attack Sounds

![swing](https://user-images.githubusercontent.com/80055816/216895176-12e61386-6ad9-4d19-b1d3-ae7985fb4065.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/216895209-9da8642b-8dc0-4f1b-9b1e-261e1a9e312f.PNG){: width="100%" height="100%"}{: .align-center}

### 15-295 Melee Impact Sound

```cpp
void AEnemy::DoDamage(AActor* Victim)
{
	if (Victim == nullptr) return;
	auto Character = Cast<AShooterCharacter>(Victim);
	if (Character)
	{
		UGameplayStatics::ApplyDamage(
			Character,
			BaseDamage,
			EnemyController,
			this,
			UDamageType::StaticClass()
		);

		if (Character->GetMeleeImpactSound())
		{
			UGameplayStatics::PlaySoundAtLocation(
				this,
				Character->GetMeleeImpactSound(),
				GetActorLocation());
		}
	}
}
```

![hit](https://user-images.githubusercontent.com/80055816/216898005-5f5a8c70-859e-40b0-80fa-9cec1e586ee4.PNG){: width="100%" height="100%"}{: .align-center}

### 15-296 Enemy Vocal Attack Sounds

![grux](https://user-images.githubusercontent.com/80055816/216914494-569e7d6c-f8d1-46e0-88c9-530f5c6102a2.PNG){: width="100%" height="100%"}{: .align-center}

![pain](https://user-images.githubusercontent.com/80055816/216914541-65e5c526-de8d-4e82-8e14-a938b409d5fc.PNG){: width="100%" height="100%"}{: .align-center}

### 15-297 Weapon Trails

![trail](https://user-images.githubusercontent.com/80055816/216955860-88ba9265-3b12-4972-b39a-d2d406eda02b.PNG){: width="100%" height="100%"}{: .align-center}

### 15-298 Blood Particles

```cpp
class SHOOTER_API AShooterCharacter : public ACharacter
{
	//..

	/** Blood splatter particles for melee hit */
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Combat, meta = (AllowPrivateAccess = "true"))
	UParticleSystem* BloodParticles;

	//..
}
```

```cpp
void AEnemy::SpawnBlood(AShooterCharacter* Victim, FName SocketName)
{
	const USkeletalMeshSocket* TipSocket{ GetMesh()->GetSocketByName(SocketName) };
	if (TipSocket)
	{
		const FTransform SocketTransform{ TipSocket->GetSocketTransform(GetMesh()) };
		if (Victim->GetBloodParticles())
		{
			UGameplayStatics::SpawnEmitterAtLocation(
				GetWorld(),
				Victim->GetBloodParticles(),
				SocketTransform
			);
		}
	}
}
```

![parti](https://user-images.githubusercontent.com/80055816/216969126-94578f5f-f66d-4dde-8e86-a2772907b489.PNG){: width="100%" height="100%"}{: .align-center}

### 15-299 Character Stun State

```cpp
UENUM(BlueprintType)
enum class ECombatState : uint8
{
	ECS_Unoccupied UMETA(DisplayName = "Unoccupied"),
	ECS_FireTimerInProgress UMETA(DisplayName = "FireTimerInProgress"),
	ECS_Reloading UMETA(DisplayName = "Reloading"),
	ECS_Equipping UMETA(DisplayName = "Equipping"),

	// Stunned 상태 추가
	ECS_Stunned UMETA(DisplayName = "Stunned"),

	ECS_MAX UMETA(DisplayName = "DefaultMAX")
};
```

### 15-300 End Stun Anim Notify

![mon](https://user-images.githubusercontent.com/80055816/217007799-a410c50b-28c3-49fc-b883-20fd651a52f2.PNG){: width="100%" height="100%"}{: .align-center}

![front](https://user-images.githubusercontent.com/80055816/217007857-89c5c43a-046f-4668-9ba3-49fccb102a01.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/217007930-a4052c65-1a0f-469a-8639-2582cfb2d497.PNG){: width="100%" height="100%"}{: .align-center}

![stun](https://user-images.githubusercontent.com/80055816/217007976-88a82d25-f278-4818-9612-b59c6258b9f5.PNG){: width="100%" height="100%"}{: .align-center}

### 15-301 Stun Character on Hit

```cpp
void AEnemy::StunCharacter(AShooterCharacter* Victim)
{
	if (Victim)
	{
		const float Stun{ FMath::FRandRange(0.f, 1.f) };
		if (Stun <= Victim->GetStunChance())
		{
			Victim->Stun();
		}
	}
}
```

![tage](https://user-images.githubusercontent.com/80055816/217024484-d8ef08b3-293f-41ec-89e7-39c51c0aa5cd.PNG){: width="100%" height="100%"}{: .align-center}

![react](https://user-images.githubusercontent.com/80055816/217024562-975ec0ac-b239-4d29-a6f6-2c6f0e90111d.PNG){: width="100%" height="100%"}{: .align-center}

![slot](https://user-images.githubusercontent.com/80055816/217024819-653ae0bd-7c3a-4ce5-b2d9-7b15ab1fc277.PNG){: width="100%" height="100%"}{: .align-center}

### 15-302 Limit Enemy Attacks

```cpp
void AEnemy::PlayAttackMontage(FName Section, float PlayRate)
{
	//..

	bCanAttack = false;
	GetWorldTimerManager().SetTimer(
		AttackWaitTimer,
		this,
		&AEnemy::ResetCanAttack,
		AttackWaitTime
	);
	if (EnemyController)
	{
		EnemyController->GetBlackboardComponent()->SetValueAsBool(
			FName("CanAttack"),
			false);
	}
}
```

![can](https://user-images.githubusercontent.com/80055816/217040148-cf78d2ac-165d-4931-858f-5175d6d2fff2.PNG){: width="100%" height="100%"}{: .align-center}

![black](https://user-images.githubusercontent.com/80055816/217040215-76739b21-7db9-490d-b12d-660897478c90.PNG){: width="100%" height="100%"}{: .align-center}

### 15-303 Delay Before Chasing Player

![mov](https://user-images.githubusercontent.com/80055816/217060671-26231c3d-75b2-469b-8739-85e3cb83422b.PNG){: width="100%" height="100%"}{: .align-center}

![motion](https://user-images.githubusercontent.com/80055816/217048327-36d6ba5a-e224-4da7-ba50-17411bcf2d34.PNG){: width="100%" height="100%"}{: .align-center}

### 15-304 Agro Enemy when Shot

```cpp
float AEnemy::TakeDamage(float DamageAmount, FDamageEvent const& DamageEvent, AController* EventInstigator, AActor* DamageCauser)
{
	// Set the Target Blackboard Key to agro the Character
	if (EnemyController)
	{
		EnemyController->GetBlackboardComponent()->SetValueAsObject(
			FName("Target"),
			DamageCauser);
	}

	if (Health - DamageAmount <= 0.f)
	{
		Health = 0.f;
		Die();
	}
	else
	{
		Health -= DamageAmount;
	}

	return DamageAmount;
}
```

### 15-305 Enemy Death Montage

![death](https://user-images.githubusercontent.com/80055816/217060177-f28bccfc-6ae9-40f7-81c9-f2454579953b.PNG){: width="100%" height="100%"}{: .align-center}

![enemy](https://user-images.githubusercontent.com/80055816/217060270-f517773c-b55a-41a1-bf67-c81bbc5d3869.PNG){: width="100%" height="100%"}{: .align-center}

![anim](https://user-images.githubusercontent.com/80055816/217060308-8d913644-eb8b-49c5-8421-c65ae01f3de0.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AEnemy::Die()
{
	if (bDying) return;

	bDying = true;

	HideHealthBar();

	UAnimInstance* AnimInstance = GetMesh()->GetAnimInstance();
	if (AnimInstance && DeathMontage)
	{
		AnimInstance->Montage_Play(DeathMontage);
	}

	if (EnemyController)
	{
		EnemyController->GetBlackboardComponent()->SetValueAsBool(
			FName("Dead"),
			true
		);
		EnemyController->StopMovement();
	}
}
```

![option](https://user-images.githubusercontent.com/80055816/217060374-3db05222-ed56-4afe-983d-6c0ab688992f.PNG){: width="100%" height="100%"}{: .align-center}

![sel](https://user-images.githubusercontent.com/80055816/217060427-ef453f2f-17f2-42e9-ba1a-af4d4247f89e.PNG){: width="100%" height="100%"}{: .align-center}

### 15-306 Destroy Enemy

![end](https://user-images.githubusercontent.com/80055816/217157245-dfc03076-9377-4d59-abf2-75d36440eb6b.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/217157295-43c11f2f-341f-4c06-a199-562324df1599.PNG){: width="100%" height="100%"}{: .align-center}

### 15-307 Polish Up Enemy Death

```cpp
void AEnemy::BulletHit_Implementation(FHitResult HitResult)
{
	if (ImpactSound)
	{
		UGameplayStatics::PlaySoundAtLocation(this, ImpactSound, GetActorLocation());
	}
	if (ImpactParticles)
	{
		UGameplayStatics::SpawnEmitterAtLocation(GetWorld(), ImpactParticles, HitResult.Location, FRotator(0.f), true);
	}

	// 죽었는데 스턴되는것 방지
	if (bDying) return;

	ShowHealthBar();	

	// Determine whether bullet hit stuns
	const float Stunned = FMath::FRandRange(0.f, 1.f);
	if (Stunned <= StunChance)
	{
		// Stun the Enemy
		PlayHitMontage(FName("HitReactFront"));
		SetStunned(true);
	}
}
```

```cpp
// 몬스터가 죽고나서 잠시뒤 사라진다
void AEnemy::FinishDeath()
{
	GetMesh()->bPauseAnims = true;

	GetWorldTimerManager().SetTimer(
		DeathTimer,
		this,
		&AEnemy::DestroyEnemy,
		DeathTime
	);
}
```

### 15-308 Character Death

![mon](https://user-images.githubusercontent.com/80055816/217207664-6a385848-43eb-49e0-971f-ef1fe77dbaea.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/217208867-fae56a95-896f-4f23-a823-b007af3f6e89.PNG){: width="100%" height="100%"}{: .align-center}

![setthis](https://user-images.githubusercontent.com/80055816/217208977-7a2016d8-58c6-4c54-8a79-c481360d5aa5.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/217209061-104bb7f7-3204-4263-8ebf-dfe76cc116e2.PNG){: width="100%" height="100%"}{: .align-center}

![finish](https://user-images.githubusercontent.com/80055816/217209195-07087b70-28f0-41df-807b-9c7eb2107d27.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AShooterCharacter::Die()
{
	UAnimInstance* AnimInstance = GetMesh()->GetAnimInstance();
	if (AnimInstance && DeathMontage)
	{
		AnimInstance->Montage_Play(DeathMontage);
	}
}
```

```cpp
void AShooterCharacter::FinishDeath()
{
	GetMesh()->bPauseAnims = true;
	APlayerController* PC = UGameplayStatics::GetPlayerController(this, 0);
	if (PC)
	{
		DisableInput(PC);
	}
}
```

![end](https://user-images.githubusercontent.com/80055816/217209341-a98ae462-df5e-4433-86b3-61bf2c51aa67.PNG){: width="100%" height="100%"}{: .align-center}

![dup](https://user-images.githubusercontent.com/80055816/217209416-e13e7d93-2954-4309-a750-efea214e43d8.PNG){: width="100%" height="100%"}{: .align-center}

![remove](https://user-images.githubusercontent.com/80055816/217209515-de927c77-3033-47a9-b680-4f7a2be366e6.PNG){: width="100%" height="100%"}{: .align-center}

![frame](https://user-images.githubusercontent.com/80055816/217209580-4ba21c64-f7f6-4a75-be7b-334f1ae86705.PNG){: width="100%" height="100%"}{: .align-center}

![wow](https://user-images.githubusercontent.com/80055816/217209644-098f520e-07cd-4b15-83a8-bd3c48bbf85e.PNG){: width="100%" height="100%"}{: .align-center}

### 15-309 Stop Enemy Attack on Death

![dead](https://user-images.githubusercontent.com/80055816/217251572-34223543-8c17-430d-87e6-bb1869535af3.PNG){: width="100%" height="100%"}{: .align-center}

![check](https://user-images.githubusercontent.com/80055816/217251655-1b712e1e-0320-4077-87ac-0d12d3b7196c.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
float AShooterCharacter::TakeDamage(float DamageAmount, FDamageEvent const& DamageEvent, AController* EventInstigator, AActor* DamageCauser)
{
	if (Health - DamageAmount <= 0.f)
	{
		Health = 0.f;
		Die();

		auto EnemyController = Cast<AEnemyController>(EventInstigator);
		if (EnemyController)
		{
			EnemyController->GetBlackboardComponent()->SetValueAsBool(
				FName(TEXT("CharacterDead")),
				true
			);
		}
	}
	else
	{
		Health -= DamageAmount;
	}
	return DamageAmount;
}
```

### 15-310 Retargeting New Montages

![health](https://user-images.githubusercontent.com/80055816/217251749-c20d085a-56a1-4c20-8278-fd42cfe6405d.PNG){: width="100%" height="100%"}{: .align-center}

![dupin](https://user-images.githubusercontent.com/80055816/217251827-aa4f47be-71f7-43ab-b900-ae467601ba36.PNG){: width="100%" height="100%"}{: .align-center}

![twin](https://user-images.githubusercontent.com/80055816/217251880-b199c32b-1b62-4e68-b1c9-7c93cf0c6b3a.PNG){: width="100%" height="100%"}{: .align-center}

### 15-311 New Type of Enemies

```cpp
class SHOOTER_API AEnemy : public ACharacter, public IBulletHitInterface
{
	//.. 

	// EditAnywhere 으로 설정해 Blueprint에서 컨트롤 가능하게 하자
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Combat, meta = (AllowPrivateAccess = "true"))
	float Health;

	//..
}
```

![dupbp](https://user-images.githubusercontent.com/80055816/217269982-7c95dcf9-d3ed-4ffe-a88c-93ce2ad9e1ac.PNG){: width="100%" height="100%"}{: .align-center}

![change](https://user-images.githubusercontent.com/80055816/217270046-e3353794-cc70-4151-8b92-7d2d5f68659f.PNG){: width="100%" height="100%"}{: .align-center}

![del](https://user-images.githubusercontent.com/80055816/217270113-46567fbf-c145-4e65-bcf3-35239880705a.PNG){: width="100%" height="100%"}{: .align-center}

![new](https://user-images.githubusercontent.com/80055816/217270189-734be3fa-1c25-47ab-aafb-24b8bdfb2210.PNG){: width="100%" height="100%"}{: .align-center}

![chief](https://user-images.githubusercontent.com/80055816/217270253-5129840a-0d3f-4c73-b5fd-9b6f656d5a2d.PNG){: width="100%" height="100%"}{: .align-center}

![value](https://user-images.githubusercontent.com/80055816/217270325-41d66fb9-578a-4310-88a4-a79ccccde5df.PNG){: width="100%" height="100%"}{: .align-center}

### 15-312 Showcasing Different Grux Enemies

![walk](https://user-images.githubusercontent.com/80055816/217284173-31a9c9c4-4993-41cf-ab66-82834a4a9ae0.PNG){: width="100%" height="100%"}{: .align-center}

![draw](https://user-images.githubusercontent.com/80055816/217284270-962fc1a2-1069-429f-a0a5-1f1b0f27daa9.PNG){: width="100%" height="100%"}{: .align-center}

![checkthis](https://user-images.githubusercontent.com/80055816/217284335-c6437fef-9592-40c4-870b-d71ad6f18d1d.PNG){: width="100%" height="100%"}{: .align-center}

<br>

## Chapter 16 Khaimera

### 16-313 Adding Khaimera

![grux](https://user-images.githubusercontent.com/80055816/217304231-104c685b-e8f3-435b-a27e-6d2c8f6e79cf.PNG){: width="100%" height="100%"}{: .align-center}

![mesh](https://user-images.githubusercontent.com/80055816/217304298-911ce8bf-173a-46bf-96d4-ce4519a1596c.PNG){: width="100%" height="100%"}{: .align-center}

![rename](https://user-images.githubusercontent.com/80055816/217304369-aaf9bcc5-0455-4f2a-a109-205a4bcd34c2.PNG){: width="100%" height="100%"}{: .align-center}

![soc](https://user-images.githubusercontent.com/80055816/217304443-d405231b-854c-4462-9918-720035650cce.PNG){: width="100%" height="100%"}{: .align-center}

![bp](https://user-images.githubusercontent.com/80055816/217304501-938fb9c9-58e1-4364-b32a-4849af9838a9.PNG){: width="100%" height="100%"}{: .align-center}

### 16-314 Khaimera Animation Blueprint

![connect](https://user-images.githubusercontent.com/80055816/217320697-b317e1ff-81cd-4ef2-8eb8-89c5fe82f533.PNG){: width="100%" height="100%"}{: .align-center}

![blend](https://user-images.githubusercontent.com/80055816/217320759-e58660fd-5384-4890-806e-496cda2b51c8.PNG){: width="100%" height="100%"}{: .align-center}

![space](https://user-images.githubusercontent.com/80055816/217320827-a4262881-07ee-4324-898a-b335b3ff609b.PNG){: width="100%" height="100%"}{: .align-center}

![loco](https://user-images.githubusercontent.com/80055816/217320899-c0af3185-3a8b-4c8e-93d5-1cac5df37c59.PNG){: width="100%" height="100%"}{: .align-center}

![mon](https://user-images.githubusercontent.com/80055816/217320965-0b312ab4-a1da-464f-9ae0-5da89651732b.PNG){: width="100%" height="100%"}{: .align-center}

![melee](https://user-images.githubusercontent.com/80055816/217321020-dc469903-758d-46b8-b015-65dafd09c59a.PNG){: width="100%" height="100%"}{: .align-center}

![group](https://user-images.githubusercontent.com/80055816/217321084-13a0ef70-bfc9-42aa-8da1-b823532659e0.PNG){: width="100%" height="100%"}{: .align-center}

![good](https://user-images.githubusercontent.com/80055816/217321154-8e555af9-eb1b-486a-aad8-ae26b39a335d.PNG){: width="100%" height="100%"}{: .align-center}

![attack](https://user-images.githubusercontent.com/80055816/217321208-bf9ac438-6785-4d36-ba1a-6154eb553ac5.PNG){: width="100%" height="100%"}{: .align-center}

### 16-315 Khaimera Attack Montage Notifies

![ac](https://user-images.githubusercontent.com/80055816/217333860-bcd422e4-be75-4682-8180-1bc375a8e3e3.PNG){: width="100%" height="100%"}{: .align-center}

![trail](https://user-images.githubusercontent.com/80055816/217333927-79c7a8b6-e269-437b-8335-6775510a35e9.PNG){: width="100%" height="100%"}{: .align-center}

![this](https://user-images.githubusercontent.com/80055816/217333975-62cc7d58-685e-47d8-bb3e-1e1a04ab30a7.PNG){: width="100%" height="100%"}{: .align-center}

### 16-316 Khaimera Death and Hit Montages

![tage](https://user-images.githubusercontent.com/80055816/217350829-4e9cb307-45e0-4ce8-b158-192262802d41.PNG){: width="100%" height="100%"}{: .align-center}

![kahi](https://user-images.githubusercontent.com/80055816/217350897-3cdc6535-7870-4dcf-aa5e-25f674e43b03.PNG){: width="100%" height="100%"}{: .align-center}

![death](https://user-images.githubusercontent.com/80055816/217350974-2a343f9e-90af-4582-9601-3bdffbef18fb.PNG){: width="100%" height="100%"}{: .align-center}

![what](https://user-images.githubusercontent.com/80055816/217351039-37559531-8853-45be-a566-e1962fba1d11.PNG){: width="100%" height="100%"}{: .align-center}

![save](https://user-images.githubusercontent.com/80055816/217351094-89094f04-2f05-4a25-af86-a21e98c24da2.PNG){: width="100%" height="100%"}{: .align-center}

![react](https://user-images.githubusercontent.com/80055816/217351148-17e3801a-117c-4015-8798-4184635e419f.PNG){: width="100%" height="100%"}{: .align-center}

### 16-317 Khaimera Anim Notifies

![nodeone](https://user-images.githubusercontent.com/80055816/217352695-3797505d-0359-4de7-bf10-7f68cf2c7b14.PNG){: width="100%" height="100%"}{: .align-center}

![nodetwo](https://user-images.githubusercontent.com/80055816/217352754-246f0a8d-8080-4e89-88b7-f0218566fb7e.PNG){: width="100%" height="100%"}{: .align-center}

### 16-318 Finishing Khaimera AnimBP

![mon](https://user-images.githubusercontent.com/80055816/217469635-478d4149-530d-4a9d-8f3c-d52023e3c47b.PNG){: width="100%" height="100%"}{: .align-center}

![time](https://user-images.githubusercontent.com/80055816/217469690-7c627336-183a-4f2f-b8ca-6f3e6208ff87.PNG){: width="100%" height="100%"}{: .align-center}

![remove](https://user-images.githubusercontent.com/80055816/217469754-20160f5f-04d5-49bb-a403-2ce319dabfd1.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/217469822-5a059a11-aca1-4c71-a4c3-65ab357f05a5.PNG){: width="100%" height="100%"}{: .align-center}

### 16-319 Different Khaimera Skins

![dam](https://user-images.githubusercontent.com/80055816/217472581-df35ccc3-f843-4048-a0b3-df60331fc90b.PNG){: width="100%" height="100%"}{: .align-center}

### 16-320 Explosive Get Overlapping Actors

```cpp
AExplosive::AExplosive()
{
	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

	ExplosiveMesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("ExplosiveMesh"));
	SetRootComponent(ExplosiveMesh);

	OverlapSphere = CreateDefaultSubobject<USphereComponent>(TEXT("OverlapSphere"));
	OverlapSphere->SetupAttachment(GetRootComponent());
}
```

```cpp
void AExplosive::BulletHit_Implementation(FHitResult HitResult)
{
	//..

	// Apply explosive damage
	TArray<AActor*> OverlappingActors;
	GetOverlappingActors(OverlappingActors, ACharacter::StaticClass());

	for (auto Actor : OverlappingActors)
	{
		UE_LOG(LogTemp, Warning, TEXT("Actor damaged by explosive: %s"), *Actor->GetName());
	}

	Destroy();

}
```

![code](https://user-images.githubusercontent.com/80055816/217481334-51e6a617-c1c2-44b8-8851-b030b32b66ed.PNG){: width="100%" height="100%"}{: .align-center}

![delete](https://user-images.githubusercontent.com/80055816/217481402-a743d4c4-3bfa-4c70-abcf-f6b37e9ae5ca.PNG){: width="100%" height="100%"}{: .align-center}

![ex](https://user-images.githubusercontent.com/80055816/217481474-94684932-fec2-4c71-8c4e-bcaa4b10d4cd.PNG){: width="100%" height="100%"}{: .align-center}

![bar](https://user-images.githubusercontent.com/80055816/217481534-4c4a554b-0c88-4d7a-96b0-6f88353a8fed.PNG){: width="100%" height="100%"}{: .align-center}

### 16-321 Explosive Agro Enemy

```cpp
// Shooter, ShooterController 매개 변수가 추가되었다
void AExplosive::BulletHit_Implementation(FHitResult HitResult, AActor* Shooter, AController* ShooterController)
{
	//..

	// Apply explosive damage
	TArray<AActor*> OverlappingActors;
	GetOverlappingActors(OverlappingActors, ACharacter::StaticClass());

	for (auto Actor : OverlappingActors)
	{
		UE_LOG(LogTemp, Warning, TEXT("Actor damaged by explosive: %s"), *Actor->GetName());

		UGameplayStatics::ApplyDamage(
			Actor,
			Damage,
			ShooterController,
			Shooter,
			UDamageType::StaticClass()
		);
	}

	Destroy();
}
```

### 16-322 Health Pickup

![blueprint](https://user-images.githubusercontent.com/80055816/217575610-ef11a4ef-9ca0-4469-9f89-2b4d60c08b42.PNG){: width="100%" height="100%"}{: .align-center}

![mesh](https://user-images.githubusercontent.com/80055816/217575751-cb99b208-d75f-45e4-be91-a773d0764d2a.PNG){: width="100%" height="100%"}{: .align-center}

![sphere](https://user-images.githubusercontent.com/80055816/217575820-b2e7c32b-c02b-4d0f-a7bf-ce9e5da3de24.PNG){: width="100%" height="100%"}{: .align-center}

![add](https://user-images.githubusercontent.com/80055816/217575884-b7de7b68-a934-4be3-894d-e0c8161138fb.PNG){: width="100%" height="100%"}{: .align-center}

![amount](https://user-images.githubusercontent.com/80055816/217575961-802e0ff8-1536-4570-a227-ccda1c579e46.PNG){: width="100%" height="100%"}{: .align-center}

![option](https://user-images.githubusercontent.com/80055816/217576028-d581652c-785e-4f9b-b091-a89bb5fd86f3.PNG){: width="100%" height="100%"}{: .align-center}

<br>

## Chapter 17 Level Creation and Finishing the Game!

### 17-323 Level Prototyping Tools

![start](https://user-images.githubusercontent.com/80055816/217600137-2714e1bc-a29d-434a-8fa8-aefa3b12b0a8.PNG){: width="100%" height="100%"}{: .align-center}

![file](https://user-images.githubusercontent.com/80055816/217600208-ddb36f8d-af91-416a-92ed-114d575201d0.PNG){: width="100%" height="100%"}{: .align-center}

![proto](https://user-images.githubusercontent.com/80055816/217606505-d5b458cd-4555-4c98-8dd3-98661ff0ce5c.PNG){: width="100%" height="100%"}{: .align-center}

### 17-324 Level Prototype - Starting Area

![add](https://user-images.githubusercontent.com/80055816/217612767-5b6a5d77-addf-48ab-be5c-fe9e8d4bb67c.PNG){: width="100%" height="100%"}{: .align-center}

### 17-325 Level Prototype - Courtyard

![group](https://user-images.githubusercontent.com/80055816/217748912-a7ec9e59-2570-4c64-b829-285721b45e64.PNG){: width="100%" height="100%"}{: .align-center}

### 17-326 Level Prototype - Courtyard2

![mig](https://user-images.githubusercontent.com/80055816/217775434-d65d21aa-9b3d-427c-ac88-6899f3a5c7d9.PNG){: width="100%" height="100%"}{: .align-center}

### 17-327 Level Prototype - Using Paragon Props

![mesh](https://user-images.githubusercontent.com/80055816/217830920-8c8fbd8a-ab8b-4f99-baa0-7756bcb0afb0.PNG){: width="100%" height="100%"}{: .align-center}

### 17-328 Ruins Assets - Floor

![check](https://user-images.githubusercontent.com/80055816/217913171-4605ae52-3b48-4e93-b884-e1ce18a81c9e.PNG){: width="100%" height="100%"}{: .align-center}

![blank](https://user-images.githubusercontent.com/80055816/217866730-b4d569f8-389e-47e4-aa9c-aaf8cef5791c.PNG){: width="100%" height="100%"}{: .align-center}

### 17-329 Ruins Assets - Walls
- make walls

### 17-330 Ruins Assets - Walls 2
- make walls

### 17-331 Ruins Assets - Corners

![brick](https://user-images.githubusercontent.com/80055816/217895745-850b1306-44b4-4deb-8560-5f0aca00d508.PNG){: width="100%" height="100%"}{: .align-center}

### 17-332 Ruins Assets - Levels
- make levels

### 17-333 Ruins Assets - Levels 2

![work](https://user-images.githubusercontent.com/80055816/217900366-f6c1c141-4065-4f11-be63-4d31c4b010f1.PNG){: width="100%" height="100%"}{: .align-center}

### 17-334 Ruins Assets - Levels 3

![regroup](https://user-images.githubusercontent.com/80055816/217902862-f61846fb-f91a-412d-b2cd-8e25ffa58a99.PNG){: width="100%" height="100%"}{: .align-center}

![mat](https://user-images.githubusercontent.com/80055816/217902949-cc564f61-ce37-4b08-b2de-731906725d82.PNG){: width="100%" height="100%"}{: .align-center}

### 17-335 Ruins Assets - Arches
- make arches

### 17-336 Ruins Assets - Arches2
- make arches2

### 17-337 Ruins Assets - Large Stairs
- 액션 모빌리티에 대한 설명은 ([**참고**](https://docs.unrealengine.com/4.27/ko/Basics/Actors/Mobility/))를 확인하자

### 17-338 Ruins Assets - Boss Room
- make boss room

### 17-339 Ruins Assets - Finishing Up the Level

![mesh](https://user-images.githubusercontent.com/80055816/218060076-78f2085b-4aa5-476d-8f9c-71a751342f49.PNG){: width="100%" height="100%"}{: .align-center}

![two](https://user-images.githubusercontent.com/80055816/218060146-25ca85ea-e120-4a8e-a835-d6ed33c9ac59.PNG){: width="100%" height="100%"}{: .align-center}

### 17-340 Level Design - Enemies

![nav](https://user-images.githubusercontent.com/80055816/218082730-5cb533d8-1eaa-4eec-a5c5-71bc9b3c7822.PNG){: width="100%" height="100%"}{: .align-center}

![value](https://user-images.githubusercontent.com/80055816/218082839-7b1556f0-a455-407b-a012-f21a5ccd4092.PNG){: width="100%" height="100%"}{: .align-center}

![sim](https://user-images.githubusercontent.com/80055816/218082880-2a61720e-8c25-4c2c-952e-97d211c77cd3.PNG){: width="100%" height="100%"}{: .align-center}

![radi](https://user-images.githubusercontent.com/80055816/218082915-c905977a-a657-48e5-acc1-5f1b55a97cc1.PNG){: width="100%" height="100%"}{: .align-center}

### 17-341 Polishing Gameplay

```cpp
class SHOOTER_API AShooterCharacter : public ACharacter
{
	//..

	/** true when Character dies */
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = Combat, meta = (AllowPrivateAccess = "true"))
	bool bDead;
}
```

![chardead](https://user-images.githubusercontent.com/80055816/218096481-57c2870f-d456-4632-b980-c8e68e74106a.PNG){: width="100%" height="100%"}{: .align-center}

![bp](https://user-images.githubusercontent.com/80055816/218096865-e19819e1-cddb-4bb2-8931-16ef7e865a09.PNG){: width="100%" height="100%"}{: .align-center}

### 17-342 Polishing Gameplay 2

![type](https://user-images.githubusercontent.com/80055816/218147659-d11b25f5-06d3-4a0a-b30a-21b70b427628.PNG){: width="100%" height="100%"}{: .align-center}

![dup](https://user-images.githubusercontent.com/80055816/218148318-6ea0b275-6143-4de0-8e6d-79e7c275a5bd.PNG){: width="100%" height="100%"}{: .align-center}

![rar](https://user-images.githubusercontent.com/80055816/218148373-4624e5ad-c87d-4db8-aad3-0326be70b5d0.PNG){: width="100%" height="100%"}{: .align-center}

### 17-343 Prevent Attack when Enemy Dead

![next](https://user-images.githubusercontent.com/80055816/218151628-5706a6fe-61c4-4ee2-a02d-d60d4156b1e5.PNG){: width="100%" height="100%"}{: .align-center}

### 17-344 Sphere Reflection Capture

![dark](https://user-images.githubusercontent.com/80055816/218157742-243a287b-8636-4886-bc40-9e5798442257.PNG){: width="100%" height="100%"}{: .align-center}

![light](https://user-images.githubusercontent.com/80055816/218157780-6dea35e0-80e8-4477-86d1-80fbc0e27be8.PNG){: width="100%" height="100%"}{: .align-center}

- LightmassImportanceVolume 은 라이트매스가 광자를 방출하는 영역을 제한시켜, 디테일한 간접광이 필요한 지역에 집중시킬 수 있습니다 ([**참고**](https://docs.unrealengine.com/4.27/ko/RenderingAndGraphics/Lightmass/Basics/))

### 17-345 Post Process Effects

![nav](https://user-images.githubusercontent.com/80055816/218268730-58a33334-d4a0-4d23-9fd5-bdea4aae1278.PNG){: width="100%" height="100%"}{: .align-center}

![black](https://user-images.githubusercontent.com/80055816/218268765-00cba8a6-19a0-4295-b8f5-0d07230a2900.PNG){: width="100%" height="100%"}{: .align-center}

![infinit](https://user-images.githubusercontent.com/80055816/218268779-0b71bfe5-6bf0-46f9-87f5-62cb65df02ed.PNG){: width="100%" height="100%"}{: .align-center}

![bloom](https://user-images.githubusercontent.com/80055816/218268800-cba6f6b8-36b5-4d86-a6f7-2c9ac0ff533f.PNG){: width="100%" height="100%"}{: .align-center}

![manual](https://user-images.githubusercontent.com/80055816/218268831-d79a1756-cdc2-4e4a-a262-7eb85e0d1667.PNG){: width="100%" height="100%"}{: .align-center}

![camera](https://user-images.githubusercontent.com/80055816/218268847-2a2ae65c-b36e-4daf-b115-3170bdbcf4e4.PNG){: width="100%" height="100%"}{: .align-center}

![chromatic](https://user-images.githubusercontent.com/80055816/218268870-c2980ed8-50d8-42b2-b88f-3c331fe72576.PNG){: width="100%" height="100%"}{: .align-center}

- 색수차(Chromatic Aberration)는 광선이 렌즈의 여러 지점으로 들어가면서 RGB 컬러의 분리를 유발하는 현상입니다 ([**참고**](https://docs.unrealengine.com/5.0/ko/post-process-effects-in-unreal-engine/))

![effect](https://user-images.githubusercontent.com/80055816/218268895-c9994f93-516a-44ff-b8aa-bc844a0d06e7.PNG){: width="100%" height="100%"}{: .align-center}

![intense](https://user-images.githubusercontent.com/80055816/218268922-a731eee9-50ed-4bf4-ad36-b2ab966b63c0.PNG){: width="100%" height="100%"}{: .align-center}

![shadow](https://user-images.githubusercontent.com/80055816/218268940-811cb8f1-352b-419b-94af-295251cf0c3f.PNG){: width="100%" height="100%"}{: .align-center}

![high](https://user-images.githubusercontent.com/80055816/218268952-22f1ad99-05b2-4f56-9af3-7a28ee9ca3f1.PNG){: width="100%" height="100%"}{: .align-center}

![oc](https://user-images.githubusercontent.com/80055816/218268972-80d132d2-d909-493e-90eb-51b3d7f865e2.PNG){: width="100%" height="100%"}{: .align-center}

- Ambient Occlusion은 빛의 차폐로 인한 감쇠 근사치를 구하는 이펙트입니다 현실에서도 방의 구석 부분은 훨씬 더 어둡듯이, 구석이나 틈 같은 곳을 더 어둡게 하여 더욱 자연스럽고 사실적인 느낌을 낼 수 있도록, 표준 글로벌 일루미네이션에 더해 미묘한 이펙트로 사용하는 것이 보통 가장 좋습니다 ([**참고**](https://docs.unrealengine.com/4.27/ko/RenderingAndGraphics/PostProcessEffects/AmbientOcclusion/))

![per](https://user-images.githubusercontent.com/80055816/218268989-1978c173-ffb5-4ba1-986e-7b5e9fb488ef.PNG){: width="100%" height="100%"}{: .align-center}

### 17-346 ESC to Quit

![quit](https://user-images.githubusercontent.com/80055816/218271209-49e6c91d-32d8-4aff-9d02-e0d82ebfcec4.PNG){: width="100%" height="100%"}{: .align-center}

### 17-347 Finishing the Game

![phy](https://user-images.githubusercontent.com/80055816/218272229-31c7dfbc-c9ce-40c9-bde6-029f78cec6b0.PNG){: width="100%" height="100%"}{: .align-center}

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
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

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
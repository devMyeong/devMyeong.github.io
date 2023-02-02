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

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
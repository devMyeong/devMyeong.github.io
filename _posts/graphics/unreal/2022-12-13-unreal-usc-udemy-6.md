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
- 

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
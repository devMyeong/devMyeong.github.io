---
title:  "Unreal Engine C++ The Ultimate Shooter Course (3)"

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

## Chapter 9 Ammo Pickups

### 09-135 Item Interping Slide

![weapon](https://user-images.githubusercontent.com/80055816/211625303-047e0f18-1a21-42ff-a670-907d054f5de2.PNG){: width="100%" height="100%"}{: .align-center}

### 09-136 Ammo Class

![item](https://user-images.githubusercontent.com/80055816/211721397-56215efc-47de-47f9-bce0-b01821332f5a.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/211721429-14e0f85f-49e6-4437-8680-8e9866862082.PNG){: width="100%" height="100%"}{: .align-center}

![select](https://user-images.githubusercontent.com/80055816/211721467-e42a5899-936b-4797-abad-2776c95e5929.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/211721503-3eee7ddc-c7f4-4494-84f5-d4d7048efc25.PNG){: width="100%" height="100%"}{: .align-center}

### 09-137 Overriding SetItemProperties
- 왜 AAmmo 클래스에서 AItem 클래스의 SetItemProperties() 함수가 정상동작 하지 않는가? AAmmo 클래스는 AmmoMesh를 사용하기 때문이다

### 09-138 PickupAmmo Function

![real](https://user-images.githubusercontent.com/80055816/211761372-baf4faef-c77e-4c94-a6f8-3af18f2c960a.PNG){: width="100%" height="100%"}{: .align-center}

### 09-139 Ammo Widget
- Overlay에 대해 설명하면? This will allow us to stack things on top of each other

![hud](https://user-images.githubusercontent.com/80055816/211761519-c9aa0f8b-a715-4c3c-a8c4-d260fbfd901d.PNG){: width="100%" height="100%"}{: .align-center}

![align](https://user-images.githubusercontent.com/80055816/211761629-86b648aa-b133-41d6-9b30-f93735cc9382.PNG){: width="100%" height="100%"}{: .align-center}

![color](https://user-images.githubusercontent.com/80055816/211761665-f08d996f-5a54-466a-98a1-34d7e046d1e7.PNG){: width="100%" height="100%"}{: .align-center}

![cen](https://user-images.githubusercontent.com/80055816/211761722-f9bb4f27-35f4-4c14-b060-cd85a3ded550.PNG){: width="100%" height="100%"}{: .align-center}

![pos](https://user-images.githubusercontent.com/80055816/211761791-a0a66123-7acd-4121-b177-0aa8a9da6f15.PNG){: width="100%" height="100%"}{: .align-center}

### 09-140 Ammo Widget Continued

![click](https://user-images.githubusercontent.com/80055816/211798087-0420339e-903b-443c-bbd2-6ac93c405d2c.PNG){: width="100%" height="100%"}{: .align-center}

![main](https://user-images.githubusercontent.com/80055816/211798155-aa6a3c47-0413-49f0-92ae-3113a4f2de5c.PNG){: width="100%" height="100%"}{: .align-center}

### 09-141 Bind Ammo Count

![ammo](https://user-images.githubusercontent.com/80055816/211813548-180d268b-1608-4d26-b551-ebfa96dbc1b0.PNG){: width="100%" height="100%"}{: .align-center}

![get](https://user-images.githubusercontent.com/80055816/211813639-2cdeff85-a142-4776-a970-ac10162a4f13.PNG){: width="100%" height="100%"}{: .align-center}

![count](https://user-images.githubusercontent.com/80055816/211813683-6860cdcd-2254-4340-bfdd-47ad95ce776e.PNG){: width="100%" height="100%"}{: .align-center}

### 09-142 Bind Ammo Icon

![texture](https://user-images.githubusercontent.com/80055816/211818052-8eaf674a-cdc4-4ce6-bc1d-a36deeeef7ed.PNG){: width="100%" height="100%"}{: .align-center}

![ar](https://user-images.githubusercontent.com/80055816/211818239-c63077df-8e30-4f43-b9b6-51b53598fc55.PNG){: width="100%" height="100%"}{: .align-center}

### 09-143 Pickup Ammo on Overlap

```cpp
void AAmmo::AmmoSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	// 이 코드는 무엇을 위한 코드인가?
	// 아이템 콜라이더에 충돌하면 자동으로 아이템이 습득되게 하는 코드
	if (OtherActor)
	{
		auto OverlappedCharacter = Cast<AShooterCharacter>(OtherActor);
		if (OverlappedCharacter)
		{
			StartItemCurve(OverlappedCharacter);
			AmmoCollisionSphere->SetCollisionEnabled(ECollisionEnabled::NoCollision);
		}
	}
}
```

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
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

### 09-144 Interpolation Scene Components

![static](https://user-images.githubusercontent.com/80055816/211864692-3a7b49f3-8eee-42e8-9a64-e777096d8095.PNG){: width="100%" height="100%"}{: .align-center}

![sm](https://user-images.githubusercontent.com/80055816/211864758-54c8daef-8e8e-48e8-8e29-a33011653813.PNG){: width="100%" height="100%"}{: .align-center}

### 09-145 Setup Interp Locations

```cpp
void AShooterCharacter::InitializeInterpLocations()
{
	FInterpLocation WeaponLocation{ WeaponInterpComp, 0 };
	InterpLocations.Add(WeaponLocation);

	FInterpLocation InterpLoc1{ InterpComp1, 0 };
	InterpLocations.Add(InterpLoc1);

	FInterpLocation InterpLoc2{ InterpComp2, 0 };
	InterpLocations.Add(InterpLoc2);

	FInterpLocation InterpLoc3{ InterpComp3, 0 };
	InterpLocations.Add(InterpLoc3);

	FInterpLocation InterpLoc4{ InterpComp4, 0 };
	InterpLocations.Add(InterpLoc4);

	FInterpLocation InterpLoc5{ InterpComp5, 0 };
	InterpLocations.Add(InterpLoc5);

	FInterpLocation InterpLoc6{ InterpComp6, 0 };
	InterpLocations.Add(InterpLoc6);
}
```

### 09-146 Interp to Multiple Locations

![default](https://user-images.githubusercontent.com/80055816/212010273-311a1cf8-d050-4419-a746-ace2a73f5905.PNG){: width="100%" height="100%"}{: .align-center}

![type](https://user-images.githubusercontent.com/80055816/212010388-383a2755-a993-4f74-970c-4a88ef030a3b.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
FVector AItem::GetInterpLocation()
{
	if (Character == nullptr) return FVector(0.f);

	switch (ItemType)
	{
	case EItemType::EIT_Ammo:
		return Character->GetInterpLocation(InterpLocIndex).SceneComponent->GetComponentLocation();
		break;

	case EItemType::EIT_Weapon:
		return Character->GetInterpLocation(0).SceneComponent->GetComponentLocation();
		break;
	}

	return FVector();
}
```

### 09-147 Limit Pickup and Equip Sounds

```cpp
void AItem::PlayPickupSound()
{
	if (Character)
	{
		if (Character->ShouldPlayPickupSound())
		{
			Character->StartPickupSoundTimer();
			if (PickupSound)
			{
				UGameplayStatics::PlaySound2D(this, PickupSound);
			}
		}
	}
}
```

```cpp
void AShooterCharacter::StartPickupSoundTimer()
{
	bShouldPlayPickupSound = false;
	GetWorldTimerManager().SetTimer(
		PickupSoundTimer,
		this,
		&AShooterCharacter::ResetPickupSoundTimer,
		PickupSoundResetTime);
}
```

<br>

## Chapter 10 Outline and Glow Effects

### 10-148 Outline Effect Theory
- When Unreal Engine renders the scene, it uses several buffers
- A buffer is just information pertaining to each pixel

![buffer](https://user-images.githubusercontent.com/80055816/212023729-afa4a115-4163-4feb-af04-98f64a8c3148.PNG){: width="100%" height="100%"}{: .align-center}

- Now, the reason we're talking about these buffers is because in order to create an outline effect

![depth](https://user-images.githubusercontent.com/80055816/212023851-e4b0bcf1-cf3e-44be-a6b8-9cf27f5f9a15.PNG){: width="100%" height="100%"}{: .align-center}

- Now, Unreal Engine has an additional buffer called custom depth, and you're allowed to select specific objects to participate in the custom depth buffer
- A pixel that's not on an object in the scene depth buffer has a same depth of value of a thousand
- Let's take all the pixels that have no white on them and let's call these pixels object pixels
- No white neighbors, call these interior pixels
- And here's the result of subtracting away the interior pixels for each object(inverse object pixels) This is the basis for creating an outline effect

### 10-149 Post Process Materials

![mat](https://user-images.githubusercontent.com/80055816/212042321-f12fc8fb-dc0f-4b98-8615-0c6e89466c03.PNG){: width="100%" height="100%"}{: .align-center}

![pp](https://user-images.githubusercontent.com/80055816/212042472-74cc8a16-610e-49d9-b89f-8e7f1f62aa22.PNG){: width="100%" height="100%"}{: .align-center}

![volume](https://user-images.githubusercontent.com/80055816/212042534-20273592-bc62-4ed1-bcf7-f835eb4bc57f.PNG){: width="100%" height="100%"}{: .align-center}

![level](https://user-images.githubusercontent.com/80055816/212042586-7a54687e-3db0-4299-9de9-49f9c60f04b1.PNG){: width="100%" height="100%"}{: .align-center}

![adapt](https://user-images.githubusercontent.com/80055816/212042637-453fa93f-312e-4f80-a408-0d32a5115206.PNG){: width="100%" height="100%"}{: .align-center}

![color](https://user-images.githubusercontent.com/80055816/212042713-7362cc48-45d2-4ef2-9f8b-e195e7ae268b.PNG){: width="100%" height="100%"}{: .align-center}

![post](https://user-images.githubusercontent.com/80055816/212042774-1575222a-9797-465f-ab25-dd94294f704d.PNG){: width="100%" height="100%"}{: .align-center}

![blue](https://user-images.githubusercontent.com/80055816/212042827-1f0af172-4164-4650-b122-3de5f922c5ec.PNG){: width="100%" height="100%"}{: .align-center}

### 10-150 Custom Depth

![check](https://user-images.githubusercontent.com/80055816/212080855-430ed1ce-dad2-493d-9f2a-8859295e585c.PNG){: width="100%" height="100%"}{: .align-center}

![buffer](https://user-images.githubusercontent.com/80055816/212080933-d2e3ea53-c548-435f-bf5c-1a0bcd5e5055.PNG){: width="100%" height="100%"}{: .align-center}

### 10-151 Texel Position and Size

![node](https://user-images.githubusercontent.com/80055816/212102557-c817c85f-8580-455c-8936-ab12f4a20b9e.PNG){: width="100%" height="100%"}{: .align-center}

- Textiles are containers for pixels
- Screen position is giving us the position on the screen and U, V coordinates for a given Texel

### 10-152 Show Interior Pixels

![nodenext](https://user-images.githubusercontent.com/80055816/212121253-87b14900-1eb7-4a50-926b-3f9ce1e0bdd4.PNG){: width="100%" height="100%"}{: .align-center}

![now](https://user-images.githubusercontent.com/80055816/212121359-c5810f67-419d-464c-a846-ab09439094c7.PNG){: width="100%" height="100%"}{: .align-center}

![sum](https://user-images.githubusercontent.com/80055816/212121427-f8ec166a-6181-4150-9de8-a78c2e6732a7.PNG){: width="100%" height="100%"}{: .align-center}

![error](https://user-images.githubusercontent.com/80055816/212121500-bab0ba9f-1208-4b19-a971-4ed6c1c07e25.PNG){: width="100%" height="100%"}{: .align-center}

![conclude](https://user-images.githubusercontent.com/80055816/212121568-bb662435-994c-4ff2-9965-1ab64907be03.PNG){: width="100%" height="100%"}{: .align-center}

- If it's negative after ceil and clamp, it's going to be zero And if it's positive after ceil and clamp, it's going to be one
- Ceil에 대해 설명하면? 소수점을 무조건 올려 더 큰 정수로 만든 결과를 출력합니다 ([**참고**](https://docs.unrealengine.com/4.27/ko/RenderingAndGraphics/Materials/ExpressionReference/Math/))
- Clamp에 대해 설명하면? 값을 받아 최소치와 최대치로 정의된 특정 범위로 제한시킵니다 ([**참고**](https://docs.unrealengine.com/4.27/ko/RenderingAndGraphics/Materials/ExpressionReference/Math/))

### 10-153 Getting The Border

![edge](https://user-images.githubusercontent.com/80055816/212136222-1f819bf6-4c6c-42f4-a80b-a3c4b9bb8655.PNG){: width="100%" height="100%"}{: .align-center}

- At Multiply, the only thing that these two have in common that are both one is the border

### 10-154 Adding the Border to the Scene Color

![end](https://user-images.githubusercontent.com/80055816/212145584-5b8d2cbf-4146-4ac7-856c-bdbc99892307.PNG){: width="100%" height="100%"}{: .align-center}

![second](https://user-images.githubusercontent.com/80055816/212145650-141bdc78-ac5b-41ab-bbdb-91e3c5895a33.PNG){: width="100%" height="100%"}{: .align-center}

- Lerp 노드에 대해 설명하면? If Alpha is 0.0, the first input is used If Alpha is 1.0, the second input is used ([**참고**](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/Materials/ExpressionReference/Math/#linearinterpolate))

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
---
title:  "Unreal Engine C++ The Ultimate Shooter Course (4)"

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

- Texels are containers for pixels
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

- Lerp 노드에 대해 설명하면? If Alpha is 0.0, the first input is used If Alpha is 1.0, the second input is used ([**참고**](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/Materials/ExpressionReference/Math/#linearinterpolate))

![second](https://user-images.githubusercontent.com/80055816/212145650-141bdc78-ac5b-41ab-bbdb-91e3c5895a33.PNG){: width="100%" height="100%"}{: .align-center}

### 10-155 Hide Occluded Pixels

![ppre](https://user-images.githubusercontent.com/80055816/212164055-6015d741-4d17-4bed-b644-2e980809c9de.PNG){: width="100%" height="100%"}{: .align-center}

![pre](https://user-images.githubusercontent.com/80055816/212164104-3a8d587e-ffaf-4b78-9c5b-27e42feb0be5.PNG){: width="100%" height="100%"}{: .align-center}

![conclude](https://user-images.githubusercontent.com/80055816/212164152-75e99880-7f39-4bb6-b5d8-aaf600542b1c.PNG){: width="100%" height="100%"}{: .align-center}

### 10-156 Change Outline Thickness

![texel](https://user-images.githubusercontent.com/80055816/212245353-126e05ab-7903-4b5f-9b9f-a2035542d3c4.PNG){: width="100%" height="100%"}{: .align-center}

![uv](https://user-images.githubusercontent.com/80055816/212245384-8bbc80e8-8af7-4646-9429-cb72d3a8b437.PNG){: width="100%" height="100%"}{: .align-center}

### 10-157 Color Tinting Effect

![conclude](https://user-images.githubusercontent.com/80055816/212249516-ca8bb7a5-cce2-4670-9f92-9eff82458d0c.PNG){: width="100%" height="100%"}{: .align-center}

### 10-158 Multiple Colors with Custom Depth Stencil

![stencil](https://user-images.githubusercontent.com/80055816/212295399-85b6098e-87d1-4647-a0dd-bbac4d7871e8.PNG){: width="100%" height="100%"}{: .align-center}

![value](https://user-images.githubusercontent.com/80055816/212295469-f96e39c3-61fd-46a3-9d75-7d5035da01ec.PNG){: width="100%" height="100%"}{: .align-center}

![blue](https://user-images.githubusercontent.com/80055816/212295539-9f78a430-4a07-436b-b457-fd4485a0246e.PNG){: width="100%" height="100%"}{: .align-center}

![create](https://user-images.githubusercontent.com/80055816/212295623-ac3d7f9a-6d2e-4e98-95b8-8dd152c1e67c.PNG){: width="100%" height="100%"}{: .align-center}

![dif](https://user-images.githubusercontent.com/80055816/212295691-e063e74a-c531-47fb-9c3f-613b1990cf62.PNG){: width="100%" height="100%"}{: .align-center}

![line](https://user-images.githubusercontent.com/80055816/212295792-a3ddc56e-babc-4163-a532-90926aa44234.PNG){: width="100%" height="100%"}{: .align-center}

![soft](https://user-images.githubusercontent.com/80055816/212295867-351c8972-ba86-41a5-bb27-a8fb7a67e356.PNG){: width="100%" height="100%"}{: .align-center}

![occpro](https://user-images.githubusercontent.com/80055816/212295907-6773a82d-fc87-49d4-bec9-24c35550f8eb.PNG){: width="100%" height="100%"}{: .align-center}

### 10-159 Blend Materials with Material Functions

![mf](https://user-images.githubusercontent.com/80055816/212316256-0f26b55b-6330-4fd7-82cb-9e7438fc1eda.PNG){: width="100%" height="100%"}{: .align-center}

![mat](https://user-images.githubusercontent.com/80055816/212316292-91918a84-1a1c-4142-8c2f-cdbfd5d717bd.PNG){: width="100%" height="100%"}{: .align-center}

![drag](https://user-images.githubusercontent.com/80055816/212316325-65034fcc-68d0-4577-a3e2-edc050389e27.PNG){: width="100%" height="100%"}{: .align-center}

![cube](https://user-images.githubusercontent.com/80055816/212316362-044abca2-5004-4fcd-81d8-dc46b823a5a2.PNG){: width="100%" height="100%"}{: .align-center}

![ori](https://user-images.githubusercontent.com/80055816/212316400-95981817-49ef-43db-9286-126459d919f3.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/212316428-8fee8ce9-c7be-42ae-81f5-3612fc0a27a4.PNG){: width="100%" height="100%"}{: .align-center}

![guns](https://user-images.githubusercontent.com/80055816/212316469-cbe04123-a5bb-4930-b9b3-84ff4a0a537b.PNG){: width="100%" height="100%"}{: .align-center}

![half](https://user-images.githubusercontent.com/80055816/212316529-85586b15-ab54-4b56-bb6f-8bd87edb11b5.PNG){: width="100%" height="100%"}{: .align-center}

### 10-160 Fresnel Effect on Glow Material

![par](https://user-images.githubusercontent.com/80055816/212347159-ae999d1e-11cc-49c8-a944-2ad1d145db74.PNG){: width="100%" height="100%"}{: .align-center}

![fres](https://user-images.githubusercontent.com/80055816/212347225-6df39e58-48e0-4a28-b12e-03bd721afd1c.PNG){: width="100%" height="100%"}{: .align-center}

![change](https://user-images.githubusercontent.com/80055816/212347278-c0a31266-1c81-40e1-9ec1-8a92d2df6ef3.PNG){: width="100%" height="100%"}{: .align-center}

![ref](https://user-images.githubusercontent.com/80055816/212347344-e56113e7-2ae0-4cc4-a63b-d766011d7e56.PNG){: width="100%" height="100%"}{: .align-center}

![more](https://user-images.githubusercontent.com/80055816/212347406-77742295-63a1-4dcc-b8cd-ec6431732f66.PNG){: width="100%" height="100%"}{: .align-center}

![effect](https://user-images.githubusercontent.com/80055816/212347489-a64cdba6-20cd-496e-a43e-c60c37525e2b.PNG){: width="100%" height="100%"}{: .align-center}

![convert](https://user-images.githubusercontent.com/80055816/212347538-47547f3d-6536-433d-a882-3e600522cade.PNG){: width="100%" height="100%"}{: .align-center}

![last](https://user-images.githubusercontent.com/80055816/212347597-df90a524-fd05-474d-a868-d13b8bd68288.PNG){: width="100%" height="100%"}{: .align-center}

### 10-161 Material Instances

![ins](https://user-images.githubusercontent.com/80055816/212387286-4d12b2ac-57f8-4b8e-b442-9e17cdfab87e.PNG){: width="100%" height="100%"}{: .align-center}

![import](https://user-images.githubusercontent.com/80055816/212387345-e8a3b9d5-9e06-424f-baa5-7a5f8ea7b292.PNG){: width="100%" height="100%"}{: .align-center}

### 10-162 Scrolling Lines Effect

![texture](https://user-images.githubusercontent.com/80055816/212457197-e02439dc-d83f-4d7b-b7ba-ac90416cb9e3.PNG){: width="100%" height="100%"}{: .align-center}

![problem](https://user-images.githubusercontent.com/80055816/212466740-e147f4fb-8fdf-45af-a57a-b1bc9561ef11.PNG){: width="100%" height="100%"}{: .align-center}

![size](https://user-images.githubusercontent.com/80055816/212457279-bf385b13-1b76-42b2-a0cd-2d2773aff82b.PNG){: width="100%" height="100%"}{: .align-center}

### 10-163 Enable Custom Depth in C++

```cpp
void AShooterCharacter::TraceForItems()
{
	if (bShouldTraceForItems)
	{
		FHitResult ItemTraceResult;
		FVector HitLocation;
		TraceUnderCrosshairs(ItemTraceResult, HitLocation);
		if (ItemTraceResult.bBlockingHit)
		{
			TraceHitItem = Cast<AItem>(ItemTraceResult.Actor);
			if (TraceHitItem && TraceHitItem->GetPickupWidget())
			{
				TraceHitItem->GetPickupWidget()->SetVisibility(true);

				// Enable Custom Depth
				TraceHitItem->EnableCustomDepth();
			}

			if (TraceHitItemLastFrame)
			{
				if (TraceHitItem != TraceHitItemLastFrame)
				{
					TraceHitItemLastFrame->GetPickupWidget()->SetVisibility(false);

					// Disable Custom Depth
					TraceHitItemLastFrame->DisableCustomDepth();
				}
			}
			TraceHitItemLastFrame = TraceHitItem;
		}
	}
	else if (TraceHitItemLastFrame)
	{
		TraceHitItemLastFrame->GetPickupWidget()->SetVisibility(false);

		// Disable Custom Depth
		TraceHitItemLastFrame->DisableCustomDepth();
	}
}
```

### 10-164 Dynamic Material Instances
- So we're going to add this material index as a variable on the item class
- Now, the next thing we're going to do if we want to be able to change materials properties at runtime is we're going to use something called a dynamic material A dynamic material instance allows us to use a material instance and set properties on that material instance at runtime
- So why two different variables? The material instance is what we will select in our blueprint That way we'll know which material instance to use And once we create a dynamic material instance, we're going to use this material instance in that dynamic material instance
- The construction script is a script that runs under certain conditions It runs if we move our actor in the world or if we change a property on the actor This is run before the game starts Nothing in the construction script will be run during the game
- OnConstruction() is the C++ version of the construction script

![dynamic](https://user-images.githubusercontent.com/80055816/212462543-b6753ef7-ee99-45c1-9511-78f1dc9255ac.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AItem::OnConstruction(const FTransform& Transform)
{
	if (MaterialInstance)
	{
		DynamicMaterialInstance = UMaterialInstanceDynamic::Create(MaterialInstance, this);

		// It's going to assign this instance for the material with the given material index
		ItemMesh->SetMaterial(MaterialIndex, DynamicMaterialInstance);
	}
}
```

### 10-165 Enable Glow Material in C++

![code](https://user-images.githubusercontent.com/80055816/212466559-dd1293ae-cceb-4032-8533-305ea870d72a.PNG){: width="100%" height="100%"}{: .align-center}

### 10-166 Show Outline While Interping

![type](https://user-images.githubusercontent.com/80055816/212470928-be6e2a18-8966-4620-b3af-d7f0f0866b30.PNG){: width="100%" height="100%"}{: .align-center}

![in](https://user-images.githubusercontent.com/80055816/212470940-de2660c7-8e42-4c04-b860-48e445784c47.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AItem::StartItemCurve(AShooterCharacter* Char)
{
	//..

	// 이코드가 필요한 이유는?
	// ItemCurve가 진행되는 동안에는 CustomDepth가 살아있어야 하기 때문이다
	bCanChangeCustomDepth = false;
}
```

### 10-167 Curve Vector for Material Parameters

![curve](https://user-images.githubusercontent.com/80055816/212482320-c5272452-7da0-40b8-b4c0-1fbd99aa986c.PNG){: width="100%" height="100%"}{: .align-center}

![addkey](https://user-images.githubusercontent.com/80055816/212483481-45a406c3-e0e4-40c0-a974-e2bd26335e40.PNG){: width="100%" height="100%"}{: .align-center}

![palse](https://user-images.githubusercontent.com/80055816/212482359-1039495b-1c35-400d-ac68-df03d50f192a.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AItem::UpdatePulse()
{
	if (ItemState != EItemState::EIS_Pickup) return;

	const float ElapsedTime{ GetWorldTimerManager().GetTimerElapsed(PulseTimer) };
	if (PulseCurve)
	{
		FVector CurveValue{ PulseCurve->GetVectorValue(ElapsedTime) };

		DynamicMaterialInstance->SetScalarParameterValue(TEXT("GlowAmount"), CurveValue.X * GlowAmount);
		DynamicMaterialInstance->SetScalarParameterValue(TEXT("FresnelExponent"), CurveValue.Y * FresnelExponent);
		DynamicMaterialInstance->SetScalarParameterValue(TEXT("FresnelReflectFraction"), CurveValue.Z * FresnelReflectFraction);
	}
}
```

### 10-168 Material Pulse When Interping

![again](https://user-images.githubusercontent.com/80055816/212486969-370e42b1-3876-474d-ac2b-2ea9e7b25636.PNG){: width="100%" height="100%"}{: .align-center}

![sector](https://user-images.githubusercontent.com/80055816/212486978-d894f240-2129-4e24-b9fb-fced1d8ba5ea.PNG){: width="100%" height="100%"}{: .align-center}

![conclude](https://user-images.githubusercontent.com/80055816/212487042-3a8db6c1-ef56-43f6-8103-dde10e3b7ed1.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AItem::UpdatePulse()
{
	float ElapsedTime{};
	FVector CurveValue{};
	switch (ItemState)
	{
	case EItemState::EIS_Pickup:
		if (PulseCurve)
		{
			ElapsedTime = GetWorldTimerManager().GetTimerElapsed(PulseTimer);
			CurveValue = PulseCurve->GetVectorValue(ElapsedTime);
		}
		break;
	case EItemState::EIS_EquipInterping:
		if (InterpPulseCurve)
		{
			// ItemInterpTimer를 사용해도 되는 이유는?
			// itemZCurve의 특정 구간과 MaterialPulseCurve의 특정 구간을 동기화 시켜
			// 해당 구간을 반짝이게 하고 싶기 때문이다
			ElapsedTime = GetWorldTimerManager().GetTimerElapsed(ItemInterpTimer);
			CurveValue = InterpPulseCurve->GetVectorValue(ElapsedTime);
		}
		break;
	}
	if (DynamicMaterialInstance)
	{
		DynamicMaterialInstance->SetScalarParameterValue(TEXT("GlowAmount"), CurveValue.X * GlowAmount);
		DynamicMaterialInstance->SetScalarParameterValue(TEXT("FresnelExponent"), CurveValue.Y * FresnelExponent);
		DynamicMaterialInstance->SetScalarParameterValue(TEXT("FresnelReflectFraction"), CurveValue.Z * FresnelReflectFraction);
	}
}
```

```cpp
void AItem::StartItemCurve(AShooterCharacter* Char)
{
	//..

	// 이 코드의 목적은?
	// 타이머 초기화
	GetWorldTimerManager().ClearTimer(PulseTimer);

	//..
}
```

### 10-169 Inventory Slot Widget

![2d](https://user-images.githubusercontent.com/80055816/212490566-8180f146-97c0-4beb-aa51-11dafb7f9433.PNG){: width="100%" height="100%"}{: .align-center}

![bp](https://user-images.githubusercontent.com/80055816/212490573-be885e9f-5dfc-4a46-93a1-1d6078d3c850.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/212490578-e7595ae1-fb21-40eb-9a82-a31717a22165.PNG){: width="100%" height="100%"}{: .align-center}

### 10-170 Inventory Bar Widget

![create](https://user-images.githubusercontent.com/80055816/212542843-23a016fe-39a9-48c0-bec7-f66ebdd45ffe.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/212542855-7383ca5d-1f35-4be8-86a9-ee2b50e65522.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/212542891-a996905d-ec04-443f-8705-f8697402b5e9.PNG){: width="100%" height="100%"}{: .align-center}

### 10-171 Add Button Icons to the Inventory Bar

![conclude](https://user-images.githubusercontent.com/80055816/212545094-e968688c-b7a7-4d21-8e96-9a22bdcfee3f.PNG){: width="100%" height="100%"}{: .align-center}

### 10-172 Add Inventory Bar to the ShooterHUDOverlay

![insert](https://user-images.githubusercontent.com/80055816/212546500-24e6bcdf-89fb-4960-897e-607edd81f355.PNG){: width="100%" height="100%"}{: .align-center}

### 10-173 Inventory Array in C++

```cpp
class SHOOTER_API AShooterCharacter : public ACharacter
{
	//..

	// 헤더파일에서 유니폼 초기화가 가능하다
	const int32 INVENTORY_CAPACITY{ 6 };

	//..
}
```

![inven](https://user-images.githubusercontent.com/80055816/212555015-de5459ee-a3e9-4eac-bcf3-dea09270ecd4.PNG){: width="100%" height="100%"}{: .align-center}

![slot](https://user-images.githubusercontent.com/80055816/212555045-301becaf-fe0b-4356-bf6f-dfad2316c2f2.PNG){: width="100%" height="100%"}{: .align-center}

![last](https://user-images.githubusercontent.com/80055816/212555062-2aac2606-f25b-4a0a-b0cd-b721f98220e1.PNG){: width="100%" height="100%"}{: .align-center}

### 10-174 Binding the Background Icon

![bind](https://user-images.githubusercontent.com/80055816/212611576-ea0b3a6b-25f4-4df1-9cfd-c94bb2e02054.PNG){: width="100%" height="100%"}{: .align-center}

![button](https://user-images.githubusercontent.com/80055816/212611642-722ea852-6155-40ca-947d-4799267bb0cc.PNG){: width="100%" height="100%"}{: .align-center}

![good](https://user-images.githubusercontent.com/80055816/212611685-3f549270-8b71-4fbf-bbdb-cc4cfe0ebce1.PNG){: width="100%" height="100%"}{: .align-center}

![wis](https://user-images.githubusercontent.com/80055816/212611732-d93a4f85-ce1c-44da-b1a5-bc68f10e80fb.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/212611767-534c341a-1df5-4559-9cbd-d44dc2015003.PNG){: width="100%" height="100%"}{: .align-center}

### 10-175 Binding the Item Icon

![newbind](https://user-images.githubusercontent.com/80055816/212630955-37700503-5c79-48ef-b1ca-d55c4b2f84e4.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/212631004-cf72d69d-88cc-4c89-9362-569cbf6abff1.PNG){: width="100%" height="100%"}{: .align-center}

![possible](https://user-images.githubusercontent.com/80055816/212631050-d5314a2b-2f94-4a14-aca0-f9ffe73a2b18.PNG){: width="100%" height="100%"}{: .align-center}

### 10-176 Binding the Ammo Icon

![re](https://user-images.githubusercontent.com/80055816/212642837-e95964f9-6d5e-4506-b0d7-b4ac20a3d4bf.PNG){: width="100%" height="100%"}{: .align-center}

![ammo](https://user-images.githubusercontent.com/80055816/212642904-53182702-f787-4d48-9b68-5f38b49f2b72.PNG){: width="100%" height="100%"}{: .align-center}

### 10-177 Binding Weapon Ammo Text

![createb](https://user-images.githubusercontent.com/80055816/212689524-3588bbc5-f7d8-419f-982a-2b9d12909adb.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/212689592-12bd0dc9-07dc-4a54-9e90-446b8dbfd446.PNG){: width="100%" height="100%"}{: .align-center}

![value](https://user-images.githubusercontent.com/80055816/212689652-1c7c4c72-8eb2-4745-a883-a830de21c46e.PNG){: width="100%" height="100%"}{: .align-center}

### 10-178 Set Item State for Picked Up

```cpp
void AShooterCharacter::GetPickupItem(AItem* Item)
{
	//..

	if (Weapon)
	{
		// 이 코드의 결과는?
		// 슬롯 인덱스가 하나씩 밀려나간다
		Weapon->SetSlotIndex(Inventory.Num());
		if (Inventory.Num() < INVENTORY_CAPACITY)
		{
			Inventory.Add(Weapon);
			Weapon->SetItemState(EItemState::EIS_PickedUp);
		}
		else // Inventory is full! Swap with EquippedWeapon
		{
			SwapWeapon(Weapon);
		}
	}

	//..
}
```

### 10-179 Send Slot Index with a Delegate

![new](https://user-images.githubusercontent.com/80055816/212706263-3bbb1cc8-3380-45b7-b6f5-e29af7a49de3.PNG){: width="100%" height="100%"}{: .align-center}

![del](https://user-images.githubusercontent.com/80055816/212706447-a9314b1d-6999-4b57-b2fe-0ca511c9d31c.PNG){: width="100%" height="100%"}{: .align-center}

![nodegood](https://user-images.githubusercontent.com/80055816/212706505-1eb516c6-264d-4145-961e-904b617f6de4.PNG){: width="100%" height="100%"}{: .align-center}

### 10-180 Play Widget Animation

![ani](https://user-images.githubusercontent.com/80055816/212728664-d6c9bd97-f2d1-4c8f-9120-5a0900907a4d.PNG){: width="100%" height="100%"}{: .align-center}

![move](https://user-images.githubusercontent.com/80055816/212728735-ff771419-6995-4793-9ad2-505237d1b06c.PNG){: width="100%" height="100%"}{: .align-center}

![box](https://user-images.githubusercontent.com/80055816/212728785-3688e444-3313-48eb-be70-e277860d87d7.PNG){: width="100%" height="100%"}{: .align-center}

![reverse](https://user-images.githubusercontent.com/80055816/212728824-c5afe10f-e9e4-4fad-872a-0a24e83fce6e.PNG){: width="100%" height="100%"}{: .align-center}

![col](https://user-images.githubusercontent.com/80055816/212728868-b293414e-7444-420b-b451-f974ff0aa1e5.PNG){: width="100%" height="100%"}{: .align-center}

![invenbar](https://user-images.githubusercontent.com/80055816/212728921-1f5fc47b-d2e9-49e1-b0a6-8c22162895f6.PNG){: width="100%" height="100%"}{: .align-center}

### 10-181 Exchange Inventory Items

```cpp
void AShooterCharacter::SwapWeapon(AWeapon* WeaponToSwap)
{
	//..

	// 아래 코드에 대해 설명하면?
	// 인벤토리의 아이템이 교체된다
	if (Inventory.Num() - 1 >= EquippedWeapon->GetSlotIndex())
	{
		Inventory[EquippedWeapon->GetSlotIndex()] = WeaponToSwap;
		WeaponToSwap->SetSlotIndex(EquippedWeapon->GetSlotIndex());
	}

	//..
}
```

```cpp
void AShooterCharacter::ExchangeInventoryItems(int32 CurrentItemIndex, int32 NewItemIndex)
{
	if ((CurrentItemIndex == NewItemIndex) || (NewItemIndex >= Inventory.Num()) || (CombatState != ECombatState::ECS_Unoccupied)) return;
	auto OldEquippedWeapon = EquippedWeapon;
	auto NewWeapon = Cast<AWeapon>(Inventory[NewItemIndex]);
	EquipWeapon(NewWeapon);

	// 아래 상태는 무슨 상태로 전환 되는 것인지 설명하면?
	// 장착하고 있지는 않으나 인벤토리에 있는 상태
	OldEquippedWeapon->SetItemState(EItemState::EIS_PickedUp);
	NewWeapon->SetItemState(EItemState::EIS_Equipped);
}
```

### 10-182 Disable Trace While Interping

```cpp
void AShooterCharacter::SelectButtonPressed()
{
	if (CombatState != ECombatState::ECS_Unoccupied) return;
	if (TraceHitItem)
	{
		TraceHitItem->StartItemCurve(this);

		// 아래 코드가 필요한 이유는?
		// It's not going to allow us to hit the select button again and
		// Start the item curve again when we just started the item curve
		TraceHitItem = nullptr;
	}
}
```

### 10-183 Prevent Swapping while Reloading

```cpp
void AShooterCharacter::SelectButtonPressed()
{
	// 아래 코드가 필요한 이유는?
	// ECS_Unoccupied 상태가 아니면 아이템을 얻지 못하게 하기 위함
	if (CombatState != ECombatState::ECS_Unoccupied) return;
	if (TraceHitItem)
	{
		TraceHitItem->StartItemCurve(this);
		TraceHitItem = nullptr;
	}
}
```

```cpp
void AShooterCharacter::ExchangeInventoryItems(int32 CurrentItemIndex, int32 NewItemIndex)
{
	// 아래 코드에서 가장 중요한 것을 설명하면?
	// ECombatState::ECS_Unoccupied 상태가 아니면 아이템 교환을 못하게 한다
	if ((CurrentItemIndex == NewItemIndex) || (NewItemIndex >= Inventory.Num()) || (CombatState != ECombatState::ECS_Unoccupied)) return;
	auto OldEquippedWeapon = EquippedWeapon;
	auto NewWeapon = Cast<AWeapon>(Inventory[NewItemIndex]);
	EquipWeapon(NewWeapon);

	OldEquippedWeapon->SetItemState(EItemState::EIS_PickedUp);
	NewWeapon->SetItemState(EItemState::EIS_Equipped);
}
```

### 10-184 Equip Montage

![dup](https://user-images.githubusercontent.com/80055816/212833407-e33e9430-a25d-40cf-8466-740e4d8648eb.PNG){: width="100%" height="100%"}{: .align-center}

![remove](https://user-images.githubusercontent.com/80055816/212833538-f4d5f791-cbb7-4849-bf29-5eb7713a1b4e.PNG){: width="100%" height="100%"}{: .align-center}

![mont](https://user-images.githubusercontent.com/80055816/212833568-ae53e418-cbca-4b70-b2b9-a76d991cdad2.PNG){: width="100%" height="100%"}{: .align-center}

![info](https://user-images.githubusercontent.com/80055816/212833605-cc227e29-aca3-46d3-a8f9-169884556984.PNG){: width="100%" height="100%"}{: .align-center}

![finish](https://user-images.githubusercontent.com/80055816/212833677-fa46a64f-6e3a-4711-bf83-492bfc0cabf3.PNG){: width="100%" height="100%"}{: .align-center}

![each](https://user-images.githubusercontent.com/80055816/212833746-8df94bf5-7964-43eb-8538-714d4699df9a.PNG){: width="100%" height="100%"}{: .align-center}

![zero](https://user-images.githubusercontent.com/80055816/212833781-00c4d21d-9eff-4ad7-b09a-ae80db5935d6.PNG){: width="100%" height="100%"}{: .align-center}

### 10-185 Play Equip Sound while Swapping

```cpp
void AItem::PlayEquipSound(bool bForcePlaySound)
{
	if (Character)
	{
		// bForcePlaySound 매개변수가 필요한 이유는?
		// 사운드를 강제적으로 내야할 상황에 사용하기 위함이다
		if (bForcePlaySound)
		{
			if (EquipSound)
			{
				UGameplayStatics::PlaySound2D(this, EquipSound);
			}
		}
		else if (Character->ShouldPlayEquipSound())
		{
			Character->StartEquipSoundTimer();
			if (EquipSound)
			{
				UGameplayStatics::PlaySound2D(this, EquipSound);
			}
		}
	}
}
```

### 10-186 Swap Pickup Text

![bind](https://user-images.githubusercontent.com/80055816/212851081-5eb09a48-a993-402e-9907-701b742bceb3.PNG){: width="100%" height="100%"}{: .align-center}

![node](https://user-images.githubusercontent.com/80055816/212851143-12d14971-7ea0-4a87-99f6-d1b1d7608095.PNG){: width="100%" height="100%"}{: .align-center}

### 10-187 Swap Animation Limitations

```cpp
void AShooterCharacter::EquipWeapon(AWeapon* WeaponToEquip, bool bSwapping)
{
	if (WeaponToEquip)
	{
		const USkeletalMeshSocket* HandSocket = GetMesh()->GetSocketByName(
			FName("RightHandSocket"));
		if (HandSocket)
		{
			// Attach the Weapon to the hand socket RightHandSocket
			HandSocket->AttachActor(WeaponToEquip, GetMesh());
		}

		if (EquippedWeapon == nullptr)
		{
			// -1 == no EquippedWeapon yet. No need to reverse the icon animation
			EquipItemDelegate.Broadcast(-1, WeaponToEquip->GetSlotIndex());
		}
		// bSwapping 변수가 필요한 이유를 설명하면?
		// 무기를 교체하는 상황이 아니라 무기를 얻고있는 상황에서는 UI 애니메이션이 동작하지 않게 하기위해
		else if(!bSwapping)
		{
			EquipItemDelegate.Broadcast(EquippedWeapon->GetSlotIndex(), WeaponToEquip->GetSlotIndex());
		}

		// Set EquippedWeapon to the newly spawned Weapon
		EquippedWeapon = WeaponToEquip;
		EquippedWeapon->SetItemState(EItemState::EIS_Equipped);
	}
}
```

```cpp
void AShooterCharacter::ExchangeInventoryItems(int32 CurrentItemIndex, int32 NewItemIndex)
{
	// ECombatState::ECS_Equipping 상태를 추가한 이유를 설명하면?
	// 무기를 교체중인 상황에서 다시 교체를 시도했을때 기다리지 않고 교체되게 하기 위함이다
	const bool bCanExchangeItems =
		(CurrentItemIndex != NewItemIndex) &&
		(NewItemIndex < Inventory.Num()) &&
		(CombatState == ECombatState::ECS_Unoccupied || CombatState == ECombatState::ECS_Equipping);

	if (bCanExchangeItems)
	{
		auto OldEquippedWeapon = EquippedWeapon;
		auto NewWeapon = Cast<AWeapon>(Inventory[NewItemIndex]);
		EquipWeapon(NewWeapon);

		OldEquippedWeapon->SetItemState(EItemState::EIS_PickedUp);
		NewWeapon->SetItemState(EItemState::EIS_Equipped);

		CombatState = ECombatState::ECS_Equipping;
		UAnimInstance* AnimInstance = GetMesh()->GetAnimInstance();
		if (AnimInstance && EquipMontage)
		{
			AnimInstance->Montage_Play(EquipMontage, 1.0f);
			AnimInstance->Montage_JumpToSection(FName("Equip"));
		}
		NewWeapon->PlayEquipSound(true);
	}
}
```

### 10-188 Create Icon Animation

![margin](https://user-images.githubusercontent.com/80055816/212942216-f959e880-36b3-479a-9edb-1dc2ba3e73fc.png){: width="100%" height="100%"}{: .align-center}

![trackone](https://user-images.githubusercontent.com/80055816/212912156-abd96ec7-7ac7-4acd-a22f-162855592f6a.PNG){: width="100%" height="100%"}{: .align-center}

![render](https://user-images.githubusercontent.com/80055816/212912213-bd6d195b-2406-4809-968e-901ee9861df6.PNG){: width="100%" height="100%"}{: .align-center}

![small](https://user-images.githubusercontent.com/80055816/212912260-a1388412-1eb7-4b97-838e-1986a9adea26.PNG){: width="100%" height="100%"}{: .align-center}

![arrow](https://user-images.githubusercontent.com/80055816/212912323-aa5aeeae-8d91-4fd7-b722-ab7dcc3dfc89.PNG){: width="100%" height="100%"}{: .align-center}

![op](https://user-images.githubusercontent.com/80055816/212912462-6ffd2fc7-bab5-4af3-940f-e60d7e16632c.PNG){: width="100%" height="100%"}{: .align-center}

![scale](https://user-images.githubusercontent.com/80055816/212912516-f011246c-c28d-4a34-bf73-8ce75ac5d072.PNG){: width="100%" height="100%"}{: .align-center}

### 10-189 Create Icon Highlight Delegate

```cpp
// 아이콘 애니메이션 델리게이트 선언
DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams(FHighlightIconDelegate, int32, SlotIndex, bool, bStartAnimation);
```

### 10-190 Functions to Broadcast Icon Highlight Delegate

```cpp
void AShooterCharacter::HighlightInventorySlot()
{
	// 아래 코드에 대해 설명하면?
	// 델리게이트를 호출하는 코드이다
	const int32 EmptySlot{ GetEmptyInventorySlot() };
	HighlightIconDelegate.Broadcast(EmptySlot, true);
	HighlightedSlot = EmptySlot;
}
```

### 10-191 Calling Highlight and UnHighlight Inventory Slot

```cpp
void AItem::FinishInterping()
{
	bInterping = false;
	if (Character)
	{
		Character->IncrementInterpLocItemCount(InterpLocIndex, -1);
		Character->GetPickupItem(this);

		// Important
		Character->UnHighlightInventorySlot();
	}

	//..
}
```

```cpp
void AItem::OnSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	if (OtherActor)
	{
		AShooterCharacter* ShooterCharacter = Cast<AShooterCharacter>(OtherActor);
		if (ShooterCharacter)
		{
			ShooterCharacter->IncrementOverlappedItemCount(1);

			// Important
			ShooterCharacter->UnHighlightInventorySlot();
		}
	}
}
```

### 10-192 Assigning the Icon Delegate

![assign](https://user-images.githubusercontent.com/80055816/212966168-2239bc3b-6054-48c4-b8e7-f7ba943b061c.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/213091508-16972b27-8572-49cb-84d5-0e3d0ba54a20.PNG){: width="100%" height="100%"}{: .align-center}

![ani](https://user-images.githubusercontent.com/80055816/212966223-34a602ca-00d0-4a74-b611-fdcf8a8a1ff0.PNG){: width="100%" height="100%"}{: .align-center}

![hidden](https://user-images.githubusercontent.com/80055816/212966282-f5b68203-e97e-41d1-aedd-08416b09da5c.PNG){: width="100%" height="100%"}{: .align-center}

![high](https://user-images.githubusercontent.com/80055816/212966332-d7438c80-7a35-4ec8-ba09-ba72a1bcec89.PNG){: width="100%" height="100%"}{: .align-center}

![vari](https://user-images.githubusercontent.com/80055816/212966386-e1eb249d-b8d9-4cc8-b6a2-000929144b10.PNG){: width="100%" height="100%"}{: .align-center}

![copy](https://user-images.githubusercontent.com/80055816/212966436-8c698b88-6b49-454a-87d5-c141377d968b.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/212966492-321f5a0d-7f02-408f-a1ee-8f7fe376051f.PNG){: width="100%" height="100%"}{: .align-center}

![conre](https://user-images.githubusercontent.com/80055816/212966535-a3a68087-0aaf-4bc2-9641-a8d26c0f7e2f.PNG){: width="100%" height="100%"}{: .align-center}

### 10-193 Data Tables FTableRowBase

```cpp
USTRUCT(BlueprintType)
struct FItemRarityTable : public FTableRowBase
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		FLinearColor GlowColor;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		FLinearColor LightColor;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		FLinearColor DarkColor;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		int32 NumberOfStars;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		UTexture2D* IconBackground;
};
```

```cpp
class SHOOTER_API AItem : public AActor
{
	GENERATED_BODY()

	//..

	/** Item Rarity data table */
	UPROPERTY(EditDefaultsOnly, BlueprintReadOnly, Category = DataTable, meta = (AllowPrivateAccess = "true"))
		class UDataTable* ItemRarityDataTable;

	//..
}
```

- Now, UDataTable is not the same thing as FTableRowBase
- OnConstruction() 함수에 대해 설명하면? It gets called When the item changes or when we move it in the world

![table](https://user-images.githubusercontent.com/80055816/212987240-6da0f759-f1dc-478e-ba00-7e659f933929.PNG){: width="100%" height="100%"}{: .align-center}

![data](https://user-images.githubusercontent.com/80055816/212987299-e02c0514-c7b2-41dd-a1bd-ffc486a79c88.PNG){: width="100%" height="100%"}{: .align-center}

![ref](https://user-images.githubusercontent.com/80055816/212987362-6795c20c-95d7-4615-9bab-440ed32d8a60.PNG){: width="100%" height="100%"}{: .align-center}

### 10-194 Accessing Data Table Rows in C++

```cpp
void AItem::OnConstruction(const FTransform& Transform)
{
	if (MaterialInstance)
	{
		DynamicMaterialInstance = UMaterialInstanceDynamic::Create(MaterialInstance, this);
		ItemMesh->SetMaterial(MaterialIndex, DynamicMaterialInstance);
	}

	EnableGlowMaterial();

	// Load the data in the Item Rarity Data Table

	// Path to the Item Rarity Data Table
	FString RarityTablePath(TEXT("DataTable'/Game/_Game/DataTable/ItemRarityDataTable.ItemRarityDataTable'"));
	UDataTable* RarityTableObject = Cast<UDataTable>(StaticLoadObject(UDataTable::StaticClass(), nullptr, *RarityTablePath));
	if (RarityTableObject)
	{
		FItemRarityTable* RarityRow = nullptr;
		switch (ItemRarity)
		{
		case EItemRarity::EIR_Damaged:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Damaged"), TEXT(""));
			break;
		case EItemRarity::EIR_Common:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Common"), TEXT(""));
			break;
		case EItemRarity::EIR_Uncommon:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Uncommon"), TEXT(""));
			break;
		case EItemRarity::EIR_Rare:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Rare"), TEXT(""));
			break;
		case EItemRarity::EIR_Legendary:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Legendary"), TEXT(""));
			break;
		}

		if (RarityRow)
		{
			GlowColor = RarityRow->GlowColor;
			LightColor = RarityRow->LightColor;
			DarkColor = RarityRow->DarkColor;
			NumberOfStars = RarityRow->NumberOfStars;
			IconBackground = RarityRow->IconBackground;
		}
	}
}
```

![rarity](https://user-images.githubusercontent.com/80055816/213094855-0afae333-b835-47e3-a05a-f84725e6e5a7.PNG){: width="100%" height="100%"}{: .align-center}

- OnConstruction() 함수에 대해 설명하면? It gets called When the item changes or when we move it in the world
- StaticLoadObject()에 대해 설명하면? Find or load an object by string name with optional outer and filename specifications ([**참고**](https://docs.unrealengine.com/4.27/en-US/API/Runtime/CoreUObject/UObject/StaticLoadObject/))

### 10-195 Setting Widget Colors from Data Table Values

![sel](https://user-images.githubusercontent.com/80055816/213099369-ac7f9cad-914f-488f-a438-ec7c7e078f84.PNG){: width="100%" height="100%"}{: .align-center}

![seln](https://user-images.githubusercontent.com/80055816/213099428-a06abdca-a1ba-4475-b9e5-2e630fdafd7e.PNG){: width="100%" height="100%"}{: .align-center}

### 10-196 Setting Glow Color from Data Table Value

```cpp
void AItem::OnConstruction(const FTransform& Transform)
{
	// Load the data in the Item Rarity Data Table

	// Path to the Item Rarity Data Table
	FString RarityTablePath(TEXT("DataTable'/Game/_Game/DataTable/ItemRarityDataTable.ItemRarityDataTable'"));
	UDataTable* RarityTableObject = Cast<UDataTable>(StaticLoadObject(UDataTable::StaticClass(), nullptr, *RarityTablePath));
	if (RarityTableObject)
	{
		FItemRarityTable* RarityRow = nullptr;
		switch (ItemRarity)
		{
		case EItemRarity::EIR_Damaged:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Damaged"), TEXT(""));
			break;
		case EItemRarity::EIR_Common:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Common"), TEXT(""));
			break;
		case EItemRarity::EIR_Uncommon:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Uncommon"), TEXT(""));
			break;
		case EItemRarity::EIR_Rare:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Rare"), TEXT(""));
			break;
		case EItemRarity::EIR_Legendary:
			RarityRow = RarityTableObject->FindRow<FItemRarityTable>(FName("Legendary"), TEXT(""));
			break;
		}

		if (RarityRow)
		{
			GlowColor = RarityRow->GlowColor;
			LightColor = RarityRow->LightColor;
			DarkColor = RarityRow->DarkColor;
			NumberOfStars = RarityRow->NumberOfStars;
			IconBackground = RarityRow->IconBackground;
		}
	}

	// 이코드가 여기있는 이유는?
	// 위에서 GlowColor를 불러오고 난뒤 Set을 해줘야 하기 때문이다
	if (MaterialInstance)
	{
		DynamicMaterialInstance = UMaterialInstanceDynamic::Create(MaterialInstance, this);
		DynamicMaterialInstance->SetVectorParameterValue(TEXT("FresnelColor"), GlowColor);
		ItemMesh->SetMaterial(MaterialIndex, DynamicMaterialInstance);

		EnableGlowMaterial();
	}
}
```

### 10-197 Set Post Process Highlight Color from Data Table

```cpp
struct FItemRarityTable : public FTableRowBase
{
	GENERATED_BODY()

	//..

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		int32 CustomDepthStencil;
};
```
![remind](https://user-images.githubusercontent.com/80055816/213106037-ef339f0c-1d60-4087-874c-b48eacf19c17.PNG){: width="100%" height="100%"}{: .align-center}

![table](https://user-images.githubusercontent.com/80055816/213106105-cf6aa193-8c26-4892-bb63-a0cc4ea32cb6.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AItem::OnConstruction(const FTransform& Transform)
{
	//..

	if (GetItemMesh())
	{
		GetItemMesh()->SetCustomDepthStencilValue(RarityRow->CustomDepthStencil);
	}

	//..
}
```

- What does StaticLoadObject do? Takes a UClass and a path and constructs an instance of the specified class

<br>

## Chapter 11 Multiple Weapon Types

### 11-198 Weapon Data Table

![new](https://user-images.githubusercontent.com/80055816/213142510-2f63c76e-48cc-4acc-a8b6-3a28eace481d.PNG){: width="100%" height="100%"}{: .align-center}

![newdata](https://user-images.githubusercontent.com/80055816/213142592-feeeda59-6f56-4cf7-bfbd-6059a75adec8.PNG){: width="100%" height="100%"}{: .align-center}

### 11-199 Getting Data from Weapon Data Table

```cpp
void AWeapon::OnConstruction(const FTransform& Transform)
{
	Super::OnConstruction(Transform);
	const FString WeaponTablePath{ TEXT("DataTable'/Game/_Game/DataTable/WeaponDataTable.WeaponDataTable'") };
	UDataTable* WeaponTableObject = Cast<UDataTable>(StaticLoadObject(UDataTable::StaticClass(), nullptr, *WeaponTablePath));

	if (WeaponTableObject)
	{
		FWeaponDataTable* WeaponDataRow = nullptr;
		switch (WeaponType)
		{
		case EWeaponType::EWT_SubmachineGun:
			WeaponDataRow = WeaponTableObject->FindRow<FWeaponDataTable>(FName("SubmachineGun"), TEXT(""));
			break;
		case EWeaponType::EWT_AssaultRifle:
			WeaponDataRow = WeaponTableObject->FindRow<FWeaponDataTable>(FName("AssaultRifle"), TEXT(""));
			break;
		}

		if (WeaponDataRow)
		{
			AmmoType = WeaponDataRow->AmmoType;
			Ammo = WeaponDataRow->WeaponAmmo;
			MagazineCapacity = WeaponDataRow->MagazingCapacity;
			SetPickupSound(WeaponDataRow->PickupSound);
			SetEquipSound(WeaponDataRow->EquipSound);
			GetItemMesh()->SetSkeletalMesh(WeaponDataRow->ItemMesh);
			SetItemName(WeaponDataRow->ItemName);
			SetIconItem(WeaponDataRow->InventoryIcon);
			SetAmmoIcon(WeaponDataRow->AmmoIcon);
		}
	}
}
```

![cons](https://user-images.githubusercontent.com/80055816/213181136-1b074a33-5936-45cc-a294-3a53c8111969.PNG){: width="100%" height="100%"}{: .align-center}

![weapon](https://user-images.githubusercontent.com/80055816/213181243-86865b05-1ed5-4135-98d8-3c883189a83a.PNG){: width="100%" height="100%"}{: .align-center}

![bp](https://user-images.githubusercontent.com/80055816/213181300-9f61cb83-76df-4225-ba4d-899ed2e6988d.PNG){: width="100%" height="100%"}{: .align-center}

### 11-200 Assault Rifle Glow Material

![fun](https://user-images.githubusercontent.com/80055816/213238730-c84b683a-e0d4-4385-977e-df9b9982f9e7.PNG){: width="100%" height="100%"}{: .align-center}

![drag](https://user-images.githubusercontent.com/80055816/213238889-15a51754-9aef-4915-b214-91c24509a347.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/213239023-26fcf0d4-dc24-41d5-b4ec-6d9f18d2470e.PNG){: width="100%" height="100%"}{: .align-center}

![next](https://user-images.githubusercontent.com/80055816/213239136-7bcc782c-e40f-4ef6-9d4d-e4fb7520694e.PNG){: width="100%" height="100%"}{: .align-center}

![re](https://user-images.githubusercontent.com/80055816/213239233-8c1a5d56-4203-4e87-82fa-340311565371.PNG){: width="100%" height="100%"}{: .align-center}

![copy](https://user-images.githubusercontent.com/80055816/213239343-f7a2f8d5-5f3c-446c-b402-1ed985621c8f.PNG){: width="100%" height="100%"}{: .align-center}

![nmat](https://user-images.githubusercontent.com/80055816/213239471-9b903725-40ad-4fba-b539-acb1c20538be.PNG){: width="100%" height="100%"}{: .align-center}

![move](https://user-images.githubusercontent.com/80055816/213239625-0a3964fd-ceb2-405e-a7ec-b6ff600d5b8a.PNG){: width="100%" height="100%"}{: .align-center}

![convert](https://user-images.githubusercontent.com/80055816/213239758-8a87cfb4-2ec4-4b5f-807d-0b401b861626.PNG){: width="100%" height="100%"}{: .align-center}

![must](https://user-images.githubusercontent.com/80055816/213239858-de8dd4b7-44eb-4613-a997-af33649a3678.PNG){: width="100%" height="100%"}{: .align-center}

![create](https://user-images.githubusercontent.com/80055816/213239960-a8e72fd4-1317-40d5-ad8d-13207f23e3bc.PNG){: width="100%" height="100%"}{: .align-center}

![color](https://user-images.githubusercontent.com/80055816/213240074-cc995498-2e41-4868-820c-9db762ef3acf.PNG){: width="100%" height="100%"}{: .align-center}

![final](https://user-images.githubusercontent.com/80055816/213240186-ef655705-8094-46bc-b64c-01ecc85bdc0d.PNG){: width="100%" height="100%"}{: .align-center}

### 11-201 Set Material Instance and Index with Data Table

```cpp
struct FWeaponDataTable : public FTableRowBase
{
	//..

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		UMaterialInstance* MaterialInstance;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
		int32 MaterialIndex;
}
```

```cpp
void AWeapon::OnConstruction(const FTransform& Transform)
{
	Super::OnConstruction(Transform);

	//..

	SetMaterialInstance(WeaponDataRow->MaterialInstance);
	// 아래 두 구문이 필요한 이유는?
	// 무기마다 material 정보가 다르다 때문에 이전 material 정보를 지워 줘야한다
	PreviousMaterialIndex = GetMaterialIndex();
	GetItemMesh()->SetMaterial(PreviousMaterialIndex, nullptr);
	SetMaterialIndex(WeaponDataRow->MaterialIndex);

	// Item 클래스에서 아래 구문을 이미 실행하고 있는데 여기서 또 하는 이유는?
	// Dynamic으로 Material를 교체해주기 위해
	if (GetMaterialInstance())
	{
		SetDynamicMaterialInstance(UMaterialInstanceDynamic::Create(GetMaterialInstance(), this));
		GetDynamicMaterialInstance()->SetVectorParameterValue(TEXT("FresnelColor"), GetGlowColor());
		GetItemMesh()->SetMaterial(GetMaterialIndex(), GetDynamicMaterialInstance());

		EnableGlowMaterial();
	}
}
```

![table](https://user-images.githubusercontent.com/80055816/213272583-89636d1a-4449-4b3b-a47b-f14b27b0b0a6.PNG){: width="100%" height="100%"}{: .align-center}

![tablenext](https://user-images.githubusercontent.com/80055816/213272646-755cf16a-d023-4361-9615-68ef57e2ca28.PNG){: width="100%" height="100%"}{: .align-center}

### 11-202 Adding Barrel Socket to AR

![socket](https://user-images.githubusercontent.com/80055816/213274069-71eba7c2-90b5-44ee-9009-3c89c2339cef.PNG){: width="100%" height="100%"}{: .align-center}

### 11-203 FABRIK IK
- What is the Fabric? Fabric is an inverse kinematics technique that allows us to move a particular bone on our skeleton

![check](https://user-images.githubusercontent.com/80055816/213283990-b74a8876-55a9-4c80-b177-0a5e3b833753.PNG){: width="100%" height="100%"}{: .align-center}

![change](https://user-images.githubusercontent.com/80055816/213284049-bbb17df5-2ce9-453e-8008-87016bd68ad4.PNG){: width="100%" height="100%"}{: .align-center}

![fab](https://user-images.githubusercontent.com/80055816/213284093-3978cd96-0d59-4d3d-b938-cc7264948b5c.PNG){: width="100%" height="100%"}{: .align-center}

![maken](https://user-images.githubusercontent.com/80055816/213284140-c64d48d6-d79e-4ff4-a015-09a208c564fc.PNG){: width="100%" height="100%"}{: .align-center}

![reconnect](https://user-images.githubusercontent.com/80055816/213284197-86f797c1-b87b-4179-afbd-e4e6d1c0e1ca.PNG){: width="100%" height="100%"}{: .align-center}

![remove](https://user-images.githubusercontent.com/80055816/213284255-b089cfc9-9406-49d9-8049-e81d5fb146d6.PNG){: width="100%" height="100%"}{: .align-center}

![this](https://user-images.githubusercontent.com/80055816/213284304-c74102b7-2a64-486a-8658-c36b69013b8c.PNG){: width="100%" height="100%"}{: .align-center}

![pose](https://user-images.githubusercontent.com/80055816/213284370-f2340ced-1498-44e9-92c9-f3dd7a4f775e.PNG){: width="100%" height="100%"}{: .align-center}

### 11-204 Switch On Weapon Type

![create](https://user-images.githubusercontent.com/80055816/213367107-cc68c869-de65-4d2a-986f-3139bd8dfd58.PNG){: width="100%" height="100%"}{: .align-center}

![addpin](https://user-images.githubusercontent.com/80055816/213368030-5ffc86d7-5e28-493e-b053-d9f5cdb67929.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/213368077-5e194521-4d79-4aec-b7f1-0183a9e88fbe.PNG){: width="100%" height="100%"}{: .align-center}

### 11-205 Disable FABRIK when Reloading

![node](https://user-images.githubusercontent.com/80055816/213387287-d1aef900-ca08-4377-9258-03bd74fa4d4e.PNG){: width="100%" height="100%"}{: .align-center}

### 11-206 AR Reload Animation Bone Name

![find](https://user-images.githubusercontent.com/80055816/213397635-a9cb3007-4336-4a21-9b12-93576c006603.PNG){: width="100%" height="100%"}{: .align-center}

![dup](https://user-images.githubusercontent.com/80055816/213397788-c768b1ea-9d22-4cc4-8fcd-a8bbb5dedb0b.PNG){: width="100%" height="100%"}{: .align-center}

![cur](https://user-images.githubusercontent.com/80055816/213397847-6aa7578a-0375-4274-8a0b-f42b8a033967.PNG){: width="100%" height="100%"}{: .align-center}

### 11-207 Reload Montage Section in Data Table

![pre](https://user-images.githubusercontent.com/80055816/213438409-9b5bcba3-e432-4e90-af39-1cd8258017e8.PNG){: width="100%" height="100%"}{: .align-center}

![end](https://user-images.githubusercontent.com/80055816/213438465-3b0193c2-8ebe-486f-b33d-549770cb4296.PNG){: width="100%" height="100%"}{: .align-center}

![table](https://user-images.githubusercontent.com/80055816/213438588-22749bba-6e81-4e51-aa64-14bffef3352d.PNG){: width="100%" height="100%"}{: .align-center}

![clear](https://user-images.githubusercontent.com/80055816/213438706-03fc503f-a7f0-4c7c-8268-7859a80f79ac.PNG){: width="100%" height="100%"}{: .align-center}

![move](https://user-images.githubusercontent.com/80055816/213438758-4b18cdf5-e684-48e6-b8d9-f152c3637f2f.PNG){: width="100%" height="100%"}{: .align-center}

### 11-208 AR AnimBP

![refinish](https://user-images.githubusercontent.com/80055816/213467542-af002987-ebab-4bcb-b8e5-21f9db22fd90.PNG){: width="100%" height="100%"}{: .align-center}

![cbp](https://user-images.githubusercontent.com/80055816/213467605-c062ab38-1f89-408d-9a33-d1d96bc5518c.PNG){: width="100%" height="100%"}{: .align-center}

![vari](https://user-images.githubusercontent.com/80055816/213467665-eafcb6b1-8e0b-487d-b591-c47696c1048e.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/213467720-0077f8ad-9198-49a1-b00b-1d6c4937a0ac.PNG){: width="100%" height="100%"}{: .align-center}

![nodenext](https://user-images.githubusercontent.com/80055816/213467787-6bd93f93-d5a2-4db1-b6bf-e21e52cf7bbd.PNG){: width="100%" height="100%"}{: .align-center}

![nodeend](https://user-images.githubusercontent.com/80055816/213467855-ca616351-8f1b-434e-a016-ae54e212ea77.PNG){: width="100%" height="100%"}{: .align-center}

### 11-209 Set AnimBP for Weapon from Data Table

![animbp](https://user-images.githubusercontent.com/80055816/213483022-62071b1c-9210-404e-a7de-223619a87ec3.PNG){: width="100%" height="100%"}{: .align-center}

![real](https://user-images.githubusercontent.com/80055816/213483113-7d0060bb-d7ab-4f37-a276-b4c55c464f14.PNG){: width="100%" height="100%"}{: .align-center}

### 11-210 Different Crosshairs Per Weapon

![smg](https://user-images.githubusercontent.com/80055816/213491882-c2cca990-3bc9-4dde-b22c-4efc56ce8d4d.PNG){: width="100%" height="100%"}{: .align-center}

![ar](https://user-images.githubusercontent.com/80055816/213491964-918a02b3-f4cf-4d8f-abca-89c487aef770.PNG){: width="100%" height="100%"}{: .align-center}

### 11-211 Drawing Crosshairs from Weapon Variables

![newcon](https://user-images.githubusercontent.com/80055816/213511896-ef1e64a5-ba43-4da8-8695-640aa4669a63.PNG){: width="100%" height="100%"}{: .align-center}

![middle](https://user-images.githubusercontent.com/80055816/213511994-053dda42-d4a0-463b-88ba-ffcc49443c3a.PNG){: width="100%" height="100%"}{: .align-center}

### 11-212 More Weapon Properties in Data Table

![createcue](https://user-images.githubusercontent.com/80055816/213522448-950d5daa-c87e-43b0-8d01-53674d8fe342.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/213522507-38190df7-4d0f-47f3-b4fe-aaacfe94cb66.PNG){: width="100%" height="100%"}{: .align-center}

### 11-213 Use Weapon Properties in FireWeapon

```cpp
struct FWeaponDataTable : public FTableRowBase
{
	//..

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	float AutoFireRate;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	class UParticleSystem* MuzzleFlash;

	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	USoundCue* FireSound;
}
```

```cpp
class SHOOTER_API AWeapon : public AItem
{
	//..

	/** The speed at which automatic fire happens */
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = DataTable, meta = (AllowPrivateAccess = "true"))
	float AutoFireRate;

	/** Particle system spawned at the BarrelSocket */
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = DataTable, meta = (AllowPrivateAccess = "true"))
	UParticleSystem* MuzzleFlash;

	/** Sound played when the weapon is fired */
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = DataTable, meta = (AllowPrivateAccess = "true"))
	USoundCue* FireSound;

	//..

	FORCEINLINE float GetAutoFireRate() const { return AutoFireRate; }
	FORCEINLINE UParticleSystem* GetMuzzleFlash() const { return MuzzleFlash; }
	FORCEINLINE USoundCue* GetFireSound() const { return FireSound; }
}
```

### 11-214 Adding Pistol Assets

![muti](https://user-images.githubusercontent.com/80055816/213619056-2b733db1-9aee-4c49-bb0c-41c456a6478e.PNG){: width="100%" height="100%"}{: .align-center}

### 11-215 Hide Bone By Name

![hide](https://user-images.githubusercontent.com/80055816/213631969-6237d8c0-4b09-4874-b4b5-239659097cb5.PNG){: width="100%" height="100%"}{: .align-center}

![dup](https://user-images.githubusercontent.com/80055816/213632002-c11216d7-001c-438b-9087-6c2dc935786c.PNG){: width="100%" height="100%"}{: .align-center}

![bone](https://user-images.githubusercontent.com/80055816/213632034-69dd7e65-b72a-424e-b6e2-ea9ab9803e11.PNG){: width="100%" height="100%"}{: .align-center}

![weapon](https://user-images.githubusercontent.com/80055816/213632076-49445bd4-2f34-464d-856c-3c4fa1379f3f.PNG){: width="100%" height="100%"}{: .align-center}

![add](https://user-images.githubusercontent.com/80055816/213632119-c572ce44-2479-4f41-b6b5-df9cba5ce252.PNG){: width="100%" height="100%"}{: .align-center}

![pistol](https://user-images.githubusercontent.com/80055816/213632148-60b34ec4-8836-40d8-8011-f25ef7e754a5.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
void AWeapon::BeginPlay()
{
	Super::BeginPlay();

	// 아래 코드에서 EPhysBodyOp::PBO_None를 세팅해준 이유는?
	// We don't care about physics for this
	if (BoneToHide != FName(""))
	{
		GetItemMesh()->HideBoneByName(BoneToHide, EPhysBodyOp::PBO_None);
	}
}
```

### 11-216 Pistol Icon

![data](https://user-images.githubusercontent.com/80055816/213636461-536504df-d1d2-4d2c-9863-e8305d2ad5b0.PNG){: width="100%" height="100%"}{: .align-center}

### 11-217 Pistol Data Table Properties

![value](https://user-images.githubusercontent.com/80055816/213642738-d786c6a0-495c-41ec-86f3-05961bbc8f93.PNG){: width="100%" height="100%"}{: .align-center}

### 11-218 Barrel Socket on the Pistol

![socket](https://user-images.githubusercontent.com/80055816/213646999-7c971379-e22c-4a48-aea3-c0254ef95b83.PNG){: width="100%" height="100%"}{: .align-center}

### 11-219 Pistol Reload Animation

![dup](https://user-images.githubusercontent.com/80055816/214074862-87c30980-2280-45f6-bb68-0cba2ab7fa72.PNG){: width="100%" height="100%"}{: .align-center}

![reload](https://user-images.githubusercontent.com/80055816/214074950-8e2de123-9f1a-411a-8b73-0bda052afe2b.PNG){: width="100%" height="100%"}{: .align-center}

![grab](https://user-images.githubusercontent.com/80055816/214075057-51060bad-bb5e-4781-8fe3-79d5d01714f3.PNG){: width="100%" height="100%"}{: .align-center}

![sound](https://user-images.githubusercontent.com/80055816/214075179-87eb362d-db90-4af1-9284-da32fca00650.PNG){: width="100%" height="100%"}{: .align-center}

![conclude](https://user-images.githubusercontent.com/80055816/214075240-212eb346-6127-40b5-a801-8d6d3f44d572.PNG){: width="100%" height="100%"}{: .align-center}

### 11-220 Pistol FABRIK IK

![new](https://user-images.githubusercontent.com/80055816/214092195-31942707-29c1-4f13-882d-069e0d8a001e.PNG){: width="100%" height="100%"}{: .align-center}

![pis](https://user-images.githubusercontent.com/80055816/214092292-f9989e0f-058f-42c5-80cb-eb880d9c4741.PNG){: width="100%" height="100%"}{: .align-center}

![add](https://user-images.githubusercontent.com/80055816/214092350-cb6f4c6e-d8ef-41b1-944c-cd84b0c68ab2.PNG){: width="100%" height="100%"}{: .align-center}

### 11-221 Pistol Aiming Pose

![re](https://user-images.githubusercontent.com/80055816/214100315-a38922cd-b295-4772-bd9c-4f7bf293a31a.PNG){: width="100%" height="100%"}{: .align-center}

![idle](https://user-images.githubusercontent.com/80055816/214100395-de7854f5-ad07-431e-9b59-80a5ee4cc109.PNG){: width="100%" height="100%"}{: .align-center}

![arm](https://user-images.githubusercontent.com/80055816/214100457-653a7ece-45bc-4402-b721-a567ccc3df11.PNG){: width="100%" height="100%"}{: .align-center}

![nice](https://user-images.githubusercontent.com/80055816/214100527-e360bf80-a241-4e8d-a1dc-785e7d4ead12.PNG){: width="100%" height="100%"}{: .align-center}

### 11-222 Aiming Pose for Each Weapon

![ar](https://user-images.githubusercontent.com/80055816/214110061-ea5b1465-0d42-43b4-9fb4-2f27c4f084f2.PNG){: width="100%" height="100%"}{: .align-center}

![rot](https://user-images.githubusercontent.com/80055816/214110109-cbd45cd5-750e-4801-ad9f-7bdc08ab287d.PNG){: width="100%" height="100%"}{: .align-center}

![how](https://user-images.githubusercontent.com/80055816/214110137-1e753253-4358-40df-b12e-9814c08e27fa.PNG){: width="100%" height="100%"}{: .align-center}

![ama](https://user-images.githubusercontent.com/80055816/214110209-9bb4d135-b76f-45d8-9785-5650dd8ef108.PNG){: width="100%" height="100%"}{: .align-center}

![ready](https://user-images.githubusercontent.com/80055816/214110256-ca1948c5-1d65-40ef-9b88-5090d6d3c3f1.PNG){: width="100%" height="100%"}{: .align-center}

### 11-223 Pistol Slide Curve

![zang](https://user-images.githubusercontent.com/80055816/214114313-67719d24-b44e-4241-ae9a-986bae7f67c3.PNG){: width="100%" height="100%"}{: .align-center}

![curve](https://user-images.githubusercontent.com/80055816/214114399-ccce178f-6025-45a4-8b6b-eb10c3e2df18.PNG){: width="100%" height="100%"}{: .align-center}

![base](https://user-images.githubusercontent.com/80055816/214114456-89feb15e-6e1b-4f86-aa74-b128265d9ec3.PNG){: width="100%" height="100%"}{: .align-center}

![auto](https://user-images.githubusercontent.com/80055816/214114504-9750a29c-a679-4d8a-b696-7a95c36022e2.PNG){: width="100%" height="100%"}{: .align-center}

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
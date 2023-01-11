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

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
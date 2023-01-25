---
title:  "Unreal Engine C++ The Ultimate Shooter Course (5)"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2023-01-25
---

## Chapter 12 Download Footsteps Assets

### 12-230 Download Footsteps Assets
- finish

### 12-231 Setup Assets for Footsteps

![check](https://user-images.githubusercontent.com/80055816/214517597-630aeb41-4291-4afe-b547-99a5c189a55c.PNG){: width="100%" height="100%"}{: .align-center}

![mat](https://user-images.githubusercontent.com/80055816/214517671-536d431f-167e-41fd-b906-5b6743020934.PNG){: width="100%" height="100%"}{: .align-center}

### 12-232 Define Physical Surface Types

![surface](https://user-images.githubusercontent.com/80055816/214520243-f5bbae46-aa27-4bd8-84db-68408ba8e587.PNG){: width="100%" height="100%"}{: .align-center}

```cpp
// shooter.h
#define EPS_Metal EPhysicalSurface::SurfaceType1
#define EPS_Stone EPhysicalSurface::SurfaceType2
#define EPS_Tile EPhysicalSurface::SurfaceType3
#define EPS_Grass EPhysicalSurface::SurfaceType4
#define EPS_Water EPhysicalSurface::SurfaceType5
```

### 12-233 Creating Physical Materials

![phy](https://user-images.githubusercontent.com/80055816/214529078-cedee02c-fd28-460a-9dfc-9de5b66ca256.PNG){: width="100%" height="100%"}{: .align-center}

![metal](https://user-images.githubusercontent.com/80055816/214529132-2327242c-c383-4ee7-9aba-7b076a15cfce.PNG){: width="100%" height="100%"}{: .align-center}

![stone](https://user-images.githubusercontent.com/80055816/214529182-29de8e4e-0108-4ef9-b8d0-0c59b8c9d8a4.PNG){: width="100%" height="100%"}{: .align-center}

![move](https://user-images.githubusercontent.com/80055816/214529239-b2fdea65-5e00-4057-84cc-3d87f9ae793e.PNG){: width="100%" height="100%"}{: .align-center}

![grass](https://user-images.githubusercontent.com/80055816/214529281-9d9f8b6a-1a2b-47a8-8173-843dfbdcf42a.PNG){: width="100%" height="100%"}{: .align-center}

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
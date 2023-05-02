---
title:  "Unreal C++ Multiplayer Master: Intermediate Game Development (1)"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2023-05-02
last_modified_at: 2023-05-02
---

## Chapter 1 Puzzle Platforms - Connecting Players

### 01-1 Course Promo
- So this course isn't for beginners Like a lot of our courses, it actually requires that you have experience making a couple of games with Unreal and C++

### 01-2 Introduction to Puzzle Platforms
- So, yeah, let's get started in to this section where we're going to be once again connecting between instances on the same machine or on the local network and getting a general overview of how the multiplayer space looks with unreal

### 01-3 Differences Between UE5 and UE4
- So in this vignette we've seen how Unreal four and Unreal five are actually more similar than they at first appear

### 01-4 Connecting Two Players

![two](https://user-images.githubusercontent.com/80055816/235619210-7668a04e-78b4-46be-afa8-fd4caf2e085c.PNG){: width="100%" height="100%"}{: .align-center}

![set](https://user-images.githubusercontent.com/80055816/235620178-5a129919-2493-4eef-b565-4d60a2c4e50c.PNG){: width="100%" height="100%"}{: .align-center}

- Under the Hood (1) Unreal loads the Map (2) The Map specifies a GameMode (3) The PlayerController joins the Map (4) It ask the GameMode to spawn a Pawn (5) The Pawn is linked th the PlayerController

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
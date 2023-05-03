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

- What happen Under the Hood? (1) Unreal loads the Map (2) The Map specifies a GameMode (3) The PlayerController joins the Map (4) It asks the GameMode to spawn a Pawn (5) The Pawn is linked th the PlayerController

### 01-5 How to Be an Active Student
- (1) Lecture Project Changes (2) This Lecture Discussions (3) Course Slides (4) Udemy QnA, Please use brace to attach the code (5) Discord

### 01-6 Surveying the Multiplayer Space

![type](https://user-images.githubusercontent.com/80055816/235854794-0dd6f1de-05ac-467e-853a-d9e216dd3687.PNG){: width="100%" height="100%"}{: .align-center}

- Session-Based Stages (1) Discovery (2) Connection (3) Synchronisation

### 01-7 Meet the Client-Server Model

![input](https://user-images.githubusercontent.com/80055816/235873417-649e869d-5994-4340-8467-69145db13918.PNG){: width="100%" height="100%"}{: .align-center}

![peer](https://user-images.githubusercontent.com/80055816/235873557-a4c6e08f-cfbe-4075-8cd3-caec7d19167e.PNG){: width="100%" height="100%"}{: .align-center}

![server](https://user-images.githubusercontent.com/80055816/235873638-0281ae10-9b0b-4399-8742-870cdde0a6e9.PNG){: width="100%" height="100%"}{: .align-center}

![person](https://user-images.githubusercontent.com/80055816/235876712-ae8d63a9-1885-4956-87fa-39ae518bdd87.PNG){: width="100%" height="100%"}{: .align-center}

![connect](https://user-images.githubusercontent.com/80055816/235873752-383563be-43d6-4c45-bff6-a1f9378a3ac4.PNG){: width="100%" height="100%"}{: .align-center}

![local](https://user-images.githubusercontent.com/80055816/235875974-1a42a4c9-24d4-40fa-b6b7-8227b09771cd.PNG){: width="100%" height="100%"}{: .align-center}

- Connect error solution, 16:23

### 01-8 Tips For Not Spawning
- Connect error solution

### 01-9 Detecting Where Code is Running
- 

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
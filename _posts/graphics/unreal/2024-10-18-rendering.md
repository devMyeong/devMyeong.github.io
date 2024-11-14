---
title:  "Matt Hoffman"

categories:
  - Unreal Engine
tags:
  - []

comments: true

toc: true
toc_sticky: true

date: 2024-10-18
last_modified_at: 2024-10-18
---

## Unreal Engine 4 Rendering Part 1: Introduction
- RenderDoc의 역할은? Lets you break down a frame draw call by draw call and inspect GPU data
- Engine\Config\ConsoleVariables.ini 에서 r.ShaderDevelopmentMode=1의 역할은? Make Unreal prompt you for a shader recompile when the default material fails to compile
- Engine\Config\BaseEngine.ini 에서 어떤 것을 할 수 있는가? bAllowAsynchronousShaderCompiling, It can be useful to turn these off if you are trying to debug a section of the C++ shader pipeline

## Unreal Engine 4 Rendering Part 2: Shaders and Vertex Data
- Vertex Factories의 역할은? Vertex Factory to control what data gets uploaded to the GPU for the vertex shader
- 모든 Shader는 무엇을 상속받는가? The base class that all shaders in Unreal derive from is FShader
- 언리얼에서 두가지 main shader는? FGlobalShader and FMaterialShader
- FShader는 FShaderResource와 쌍을 이루며 어떤 역할을 하는가? Keeps track of the resource on the GPU associated with that particular shader
- FShaderResource가 여러 FShader 에게 공유될 수 있는 조건은? If the compiled output from the FShader matches an already existing one
- Global Shader의 특징은? Only one instance
- FMaterialShader와 FMeshMaterialShader의 특징은? Allow multiple instances, each one associated with its own copy of the GPU resource

<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
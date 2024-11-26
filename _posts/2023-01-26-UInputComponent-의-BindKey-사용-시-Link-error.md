---
layout: post
title: UInputComponent 의 BindKey 사용 시 Link error
date: 2023-01-26
last_modified_at: 2023-01-26
tags:
  - UnrealEngine
---
# UInputComponent 의 BindKey 사용 시 Link error

## 발생 상황

UInputComponent 의 BindKey 사용 시 아래와 같은 링크 오류가 발생 함.

```text
1>[23/25] Link UnrealEditor-PracticeUnreal.lib
1>   D:\repos\MyProjects\PracticeUnreal\Intermediate\Build\Win64\UnrealEditor\Development\PracticeUnreal\UnrealEditor-PracticeUnreal.lib 라이브러리 및 D:\repos\MyProjects\PracticeUnreal\Intermediate\Build\Win64\UnrealEditor\Development\PracticeUnreal\UnrealEditor-PracticeUnreal.exp 개체를 생성하고 있습니다.
1>[24/25] Link UnrealEditor-PracticeUnreal.dll
1>   D:\repos\MyProjects\PracticeUnreal\Intermediate\Build\Win64\UnrealEditor\Development\PracticeUnreal\UnrealEditor-PracticeUnreal.suppressed.lib 라이브러리 및 D:\repos\MyProjects\PracticeUnreal\Intermediate\Build\Win64\UnrealEditor\Development\PracticeUnreal\UnrealEditor-PracticeUnreal.suppressed.exp 개체를 생성하고 있습니다.
1>SightCustomizingCameraPawn.cpp.obj : error LNK2019: "__declspec(dllimport) public: __cdecl FInputChord::FInputChord(void)" (__imp_??0FInputChord@@QEAA@XZ)"public: virtual void __cdecl ASightCustomizingCameraPawn::SetupPlayerInputComponent(class UInputComponent *)" (?SetupPlayerInputComponent@ASightCustomizingCameraPawn@@UEAAXPEAVUInputComponent@@@Z) 함수에서 참조되는 확인할 수 없는 외부 기호
1>SightCustomizingCameraPawn.cpp.obj : error LNK2019: "__declspec(dllimport) public: __cdecl FInputChord::FInputChord(struct FInputChord const &)" (__imp_??0FInputChord@@QEAA@AEBU0@@Z)"public: struct FInputKeyBinding & __cdecl UInputComponent::BindKey<class ASightCustomizingCameraPawn>(struct FInputChord,enum EInputEvent,class ASightCustomizingCameraPawn *,void (__cdecl ASightCustomizingCameraPawn::*)(void))" (??$BindKey@VASightCustomizingCameraPawn@@@UInputComponent@@QEAAAEAUFInputKeyBinding@@UFInputChord@@W4EInputEvent@@PEAVASightCustomizingCameraPawn@@P84@EAAXXZ@Z) 함수에서 참조되는 확인할 수 없는 외부 기호
1>SightCustomizingCameraPawn.cpp.obj : error LNK2019: "__declspec(dllimport) public: __cdecl FInputChord::~FInputChord(void)" (__imp_??1FInputChord@@QEAA@XZ)"public: struct FInputKeyBinding & __cdecl UInputComponent::BindKey<class ASightCustomizingCameraPawn>(struct FInputChord,enum EInputEvent,class ASightCustomizingCameraPawn *,void (__cdecl ASightCustomizingCameraPawn::*)(void))" (??$BindKey@VASightCustomizingCameraPawn@@@UInputComponent@@QEAAAEAUFInputKeyBinding@@UFInputChord@@W4EInputEvent@@PEAVASightCustomizingCameraPawn@@P84@EAAXXZ@Z) 함수에서 참조되는 확인할 수 없는 외부 기호
1>SightCustomizingCameraPawn.cpp.obj : error LNK2001: 확인할 수 없는 외부 기호 "public: __cdecl FInputChord::~FInputChord(void)" (??1FInputChord@@QEAA@XZ)
1>D:\repos\MyProjects\PracticeUnreal\Binaries\Win64\UnrealEditor-PracticeUnreal.dll : fatal error LNK1120: 4개의 확인할 수 없는 외부 참조입니다.
```

## 해결 방법

해당 코드가 있는 Module 의 Build.cs 코드에 "SlateCore", "Slate" 모듈 이름을 추가

```csharp
PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });
```

## 참고

* [Why does BindKey cause Link error? - Programming & Scripting / C++ - Epic Developer Community Forums (unrealengine.com)](https://forums.unrealengine.com/t/why-does-bindkey-cause-link-error/35447)

* [FInputChord - Unreal Engine Documentation](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Slate/Framework/Commands/FInputChord/)

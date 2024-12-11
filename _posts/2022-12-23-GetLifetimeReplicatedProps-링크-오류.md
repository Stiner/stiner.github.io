---
layout: post
title: GetLifetimeReplicatedProps 링크 오류
date: 2022-12-23
last_modified_at: 2022-12-23
tags:
  - UnrealEngine
---
# GetLifetimeReplicatedProps 링크 오류

## 문제 발생

```cpp
class AGameCharacter : public ACharacter
{
protected:
    UPROPERTY(Replicated)
    int isJumping;
public:
    void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const override;
}
```

위의 클래스를 상속받아 APlayerCharacter를 선언.

```cpp
class APlayerCharacter : public AGameCharacter
{
protected:
    UPROPERTY(Replicated)
    bool isWalking;
}
```

이렇게 하면 링크 과정에서 Symbol 을 찾을 수 없다고 오류 남.

```text
PlayerCharacter.gen.cpp.obj : error LNK2001: 확인할 수 없는 외부 기호 "public: virtual void __cdecl APlayerCharacter::GetLifetimeReplicatedProps(class TArray<class FLifetimeProperty,class TSizedDefaultAllocator<32> > &)const " (?GetLifetimeReplicatedProps@APlayerCharacter@@UEBAXAEAV?$TArray@VFLifetimeProperty@@V?$TSizedDefaultAllocator@$0CA@@@@@@Z)
PlayerCharacter.cpp.obj : error LNK2001: 확인할 수 없는 외부 기호 "public: virtual void __cdecl APlayerCharacter::GetLifetimeReplicatedProps(class TArray<class FLifetimeProperty,class TSizedDefaultAllocator<32> > &)const " (?GetLifetimeReplicatedProps@APlayerCharacter@@UEBAXAEAV?$TArray@VFLifetimeProperty@@V?$TSizedDefaultAllocator@$0CA@@@@@@Z)
```

## 원인

APlayerCharacter::isWalking 의 UPROPERTY에 Replicated 를 선언한 것 때문에, APlayerCharacter.generated.h 파일을 갱신할 때 자동으로 GetLifetimeReplicatedProps 선언을 추가 해 주게 된다. 그렇다 보니 APlayerCharacter::GetLifetimeReplicatedProps 의 구현을 찾을 수가 없게 되는 것 이였다.

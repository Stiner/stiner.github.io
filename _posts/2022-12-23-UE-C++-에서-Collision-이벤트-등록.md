---
layout: post
title: UE C++ 에서 Collision 이벤트 등록
date: 2022-12-23
last_modified_at: 2023-08-29
tags:
  - UnrealEngine
---
# UE C++ 에서 Collision 이벤트 등록

## Overlap 이나 Hit 이벤트를 C++ 코드로 바인딩 하는 방법

예시) SphereComponent 의 OnComponentBeginOverlap 에 이벤트 핸들러 바인딩

```cpp
UCLASS()
class AMyActor : AActor
{
public:
    virtual void BeginPlay() override;
    UFUNCTION()
    void OnBeginOverlap(UPrimitiveComponent* HitComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult);
}
void AMyActor::BeginPlay()
{
    Super::BeginPlay();
    SphereComponent->OnComponentBeginOverlap.AddDynamic(this, &AMyActor::OnBeginOverlap);
}
void AMyActor::OnBeginOverlap(UPrimitiveComponent* HitComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
    // TODO ...
}
```

## 참고 사항

* AddDynamic 은 매크로로 제작 되어 있어서 Visual Studio 의 Intellicense 에 보여지지 않을 수 있다.

* AddDynamic 호출을 Constructor 에서 하면 바인딩이 안된다.

* 바인딩 할 함수의 Signature 는 PrimitiveComponent.h 에서 확인 가능 하다.
  
  * Visual Studio 기준 바인딩 하려는 Property Type 의 "정의 탐색"의로 찾아갈 수 있다.
    
    ```cpp
    /** Delegate for notification of start of overlap with a specific component */
    DECLARE_DYNAMIC_MULTICAST_SPARSE_DELEGATE_SixParams( FComponentBeginOverlapSignature, UPrimitiveComponent, OnComponentBeginOverlap, UPrimitiveComponent*, OverlappedComponent, AActor*, OtherActor, UPrimitiveComponent*, OtherComp, int32, OtherBodyIndex, bool, bFromSweep, const FHitResult &, SweepResult);
    ```
    
    ```cpp
    /** 
     *    Event called when something starts to overlaps this component, for example a player walking into a trigger.
     *    For events when objects have a blocking collision, for example a player hitting a wall, see 'Hit' events.
     *
     *    @note Both this component and the other one must have GetGenerateOverlapEvents() set to true to generate overlap events.
     *    @note When receiving an overlap from another object's movement, the directions of 'Hit.Normal' and 'Hit.ImpactNormal'
     *    will be adjusted to indicate force from the other object against this object.
     */
    UPROPERTY(BlueprintAssignable, Category="Collision")
    FComponentBeginOverlapSignature OnComponentBeginOverlap;
    ```

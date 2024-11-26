---
layout: post
title: UPROPERTY UActorComponent Type 멤버 변수가 Nullptr이 되는 문제
date: 2023-01-30
last_modified_at: 2023-02-02
tags:
  - UnrealEngine
---
# UPROPERTY UActorComponent Type 멤버 변수가 Nullptr이 되는 문제

## 문제 발생

UActorCompont 를 Constructor 에서 생성 후 다른 곳에서 사용 시 Nullptr로 되어 있는 문제가 발생.

Constructor 에서

![image-2023-1-30_14-38-22.png](../attachments/604ef20a54e017e2a8b43cdda1a772c587b0b849.png)

BeginPlay 에서

![image-2023-1-30_14-38-45.png](../attachments/0755b11a67253da6995bc463bdc8d16033ee4379.png)

클래스를 상속받은 Blueprint 에서 Component의 Detail 패널에 아무것도 안보임.

![image-2023-1-30_14-39-33.png](../attachments/942f39686ced42c673e7c6643410b5b3f4eab6c7.png)

## 해결 방법

**변수 이름 변경으로 해결.**

Contructor 에서

![image-2023-1-30_14-41-58.png](../attachments/0838093cee705390b2db1dae7b065017de9f7a47.png)

BeginPlay 에서

![image-2023-1-30_14-42-37.png](../attachments/d47c4839e98477ef4dfc473d2f318a8a5debf987.png)

블루 프린트에서도 보여짐

![image-2023-1-30_14-44-6.png](../attachments/312ab5a6a7e6501c1c8d5d4dd96d5ca683ea378f.png)

## 원인 추측

Blueprint 에 이전에 사용했던 변수에 대한 정보가 남아있어 충돌이 나는 것이 아닐까 하는 추측.

## 관련 커뮤니티 Thread

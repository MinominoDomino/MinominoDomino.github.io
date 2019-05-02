---
layout: post
title:  "[IOS] 앱 생명주기(Application Life Cycle)"
subtitle:   "IOS 앱 생명주기"
categories: devlog
tags: ios

---

앱의 생명주기(Application Life Cycle)를 알아보자.

## UIApplication

모든 IOS 앱은 Main함수로 `UIApplicationMain 함수`를 실행하고 `UIApplication 인스턴스`와 `Appdelegate인스턴스`등의 앱의 핵심 인스턴스를 생성하고 스토리보드로 부터 사용가능한 사용자 인터페이스를 로드하고 초기 설정을 수행합니다.
UIApplication은 singleton형태로 생성되고 앱 전역에서 하나로 존재하고 사용한다.
또한 MainRunLoop를 실행하여 사용자로부터 발생하는 이벤트를 감지하고 Appdelegate로 알리게 됩니다.

![앱](https://MinominoDomino.github.io/assets/img/ios/app.png)

## 앱 상태 종류
앱의 상태는 크게 5개로 존재하고 각 상태에 따른 실행되는 함수가 있습니다.

|상태| 설명 |
|--------|--------|
|Not Running| 앱이 시작되지 않거나 종료된 상태        |
|Inactive| 앱이 포그라운 상태이고 이벤트 수신이 불가능한 상태|
|Active| 앱이 포그라운드 상태이고 이벤트 수신이 가능한 상태|
|Background| 앱이 백그라운드 상태이고 실행되는 코드가 있는 상태| 
|Suspended| 앱이 백그라운드 상태이고 실행되는 코드가 없는 상태|

![앱 상태](https://MinominoDomino.github.io/assets/img/ios/app-status.png)

각 상태에 따른 전환은 `AppDelegate.swift` 파일에서 메서드로 제공합니다.
각 상태에 따라 해당 메서드들이 동작하게됩니다. 따라서, 상태에 따른 제어나 원하는 동작을 할 수 있습니다.

- application(_:didFinishLaunching:) : 앱이 실행되면 호출 

- applicationWillResignActive(): : 앱이 active 에서 inactive로 상태가 변할 때 호출

- applicationDidEnterBackground(): - 앱이 background 상태가 될 때 호출

- applicationWillEnterForeground(): - 앱이 background에서 foreground로 상태로 갈 때 호출

- applicationDidBecomeActive(): - 앱이 active 상태가 되어 실행될 때 호출

- applicationWillTerminate(): - 앱이 종료될 때 실행


![앱 델리게이트](https://MinominoDomino.github.io/assets/img/ios/appdelegate.png)

직접 print()로 각 메서드에 정의하여 하나씩 출력해보면 각 상태에 따른 메서드 호출을 볼 수 있습니다.



## 참조

[애플 가이드 문서 - The App Life Cycle, App Programming Guide for iOS](https://developer.apple.com/documentation/uikit/core_app/managing_your_app_s_life_cycle)

[애플 개발 문서 - Managing Your App's Life Cycle](https://developer.apple.com/library/archive/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW1)










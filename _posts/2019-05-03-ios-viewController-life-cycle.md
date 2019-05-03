---
layout: post
title:  "[IOS] 뷰컨트롤러 생명주기(ViewController Life Cycle)"
subtitle:   "IOS 뷰컨트롤러 생명주기"
categories: devlog
tags: ios

---

뷰컨트롤러 생명주기(ViewController Life Cycle)를 알아보자.

## ViewController
모든 IOS 앱은 하나의 화면에 하나의 ViewController를 가지고 있습니다.
그리고 여러 화면으로 앱이 구성되어 있으니 뷰컨트롤러도 여러개 일거에요!
이러한 여러 뷰컨트롤러들은 각자 자신들의 생명주기를 가지고 있습니다.
뷰컨트롤러의 주요 역활로는 다음과 같습니다.

- 데이터 변경에 따른 응답으로 뷰들의 컨텐츠를 업데이트
- 뷰와 사용자의 상호 작용에 반응
- 뷰 관리와 인터페이스의 레이아웃을 관리
- 앱에서 다른 뷰컨트롤러와 다른 오브젝트를 관리

## 뷰컨트롤러 상태 종류
뷰컨트롤러는 4가지의 상태를 가지고 있습니다.

|상태| 설명 |
|--------|--------|
|Appearing| 뷰가 사라져있다가 나타날려는 상태|
|Appeared| 뷰가 나타난 상태|
|Disappearing| 뷰가 나타나있다가 사라질려는 상태|
|Disappeared| 뷰가 사라진 상태|

![뷰컨트롤러](https://MinominoDomino.github.io/assets/img/ios/viewController.png)

## 뷰컨트롤러 함수
뷰컨트롤러는 상태 진입에 따른 제어 함수를 제공합니다.

|함수| 시점 |
|--------|--------|
|loadView| 뷰를 메모리에 올릴때 호출 |
|viewDidLoad| 뷰가 메모리에 올라간 후 호출 |
|viewWillAppear| 뷰가 화면으로 표시되기 전에 호출 |
|viewDidAppear| 뷰가 화면으로 표시되면 호출 |
|viewWillDisappear| 뷰가 화면에서 사라지기 전에 호출 |
|viewDidDisappear| 뷰가 화면에서 사라지면 호출 |

loadView는 왠만하면 직접 호출 하지말고 viewDidLoad에서 해주시면됩니다.
viewWillAppear는 viewDidLoad와 같은 것 처럼 보이지만, 다른 뷰로 이동하고 올때마다 호출되며 화면이 나타날때 수행할 작업을 하면된다. 직접 예로 작성해서 뷰를 이동하면 viewDidLoad는 처음에만 실행되고 viewWillAppear만 실행되는 것을 볼 수 있다.
직접 뷰컨트롤러가 제공하는 함수를 오버라이딩하여 print로 확인하면서 익히는게 빠릅니다.

![뷰컨트롤러실습](https://MinominoDomino.github.io/assets/img/ios/viewController-ex.png)


## 참조

[애플 개발 문서 - UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)









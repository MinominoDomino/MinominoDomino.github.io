---
layout: post
title:  "[iOS] 스토리보드 레퍼런스(Storyboard Reference)"
subtitle:   "iOS 스토리보드 레퍼런스"
categories: devlog
tags: ios

---

스토리보드 레퍼런스(Storyboard Reference)를 알아보자.

오늘은 스토리보드 레퍼런스에 대해 알아보겠습니다.
- 스토리보드 구조
- initial View Controller
- Storyboard ID
- Storyboard Reference

## 스토리보드
스토리보드는 화면에 표시는 화면에 대한 캔버스입니다.
그리고 각 스토리보드에 표시되는 화면 하나를 씬이라고 부릅니다.
스토리보드에는 각 화면들의 상호 연관관계나 앱에서 보여주는 화면을 한눈에 볼 수 있도록 제공해줍니다.
전체 흐름을 한눈에 볼 수 있고 화면사이의 전환을 자동으로 해주는 장점이 있습니다.
단점으로 화면에 그림이 많아서 xcode가 버벅이는 현상이 있고 팀단위 작업에서 머지하는데에 어려움이 있습니다.
이런 문제는 스토리보드를 분할하여 스토리보드 레퍼런스로 해결 할 수 있습니다.

## 스토리보드 해보기

먼저 아래 그림처럼 화면을 구성해줍니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/1.png)

세그웨이로 연결을 할때에는 시작 뷰 컨트롤러에서 이동 할 뷰 컨트롤러로 마우스 오른쪽 클릭후 드래그하면 아래 그림이 나옵니다.
그리고 show을 선택해주면 됩니다.
![2.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/2.png)

코드로 뷰 컨트롤러를 연결해보겠습니다.
이동 할 뷰 컨트롤러에서 Identity Inspector에 Identity탭을 보면 Storyboard ID가 있습니다.
secondViewController로 적어줍니다.
스토리보드ID는 해당 스토리보드에서 각 뷰를 식별할 수 있는 식별값이라고 생각하면됩니다.
현재 스토리보드는 Main이고 Main스토리보드에서 secondVIewController라는 ID를 가진 뷰 컨트롤러를 지정하게됩니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/3.png)

첫번째 씬에서 Move Second버튼에 액션을 걸어주겠습니다.
여기에서는 코드로 두번째 씬을 모달로 띄우도록 하겠습니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/4.png)

`instantiateViewController()`를 이용하여 스토리보드ID로 해당 뷰컨트롤러를 찾고 `present()`로 화면에 모달로 표시해줍니다.
```swift
 @IBAction func moveSecondButton(_ sender: Any) {
       guard let second = storyboard?.instantiateViewController(withIdentifier: "secondViewController") else { return }
       present(second, animated: true, completion: nil)
    }
```
이제 실행해서 세그웨이로 연결한 방법과 코드로 연결한 방법이 정상 동작하는지 확인해봅니다~

## 스토리보드 레퍼런스 해보기
하나의 스토리보드에 여러 씬이 존재하면 몇가지 단점을 알려드렸습니다.
이런 방법을 해결하기 위해서 스토리보드를 분할 할 수 있는데 이때 스토리보드 레퍼런스를 사용합니다.
스토리보드 레퍼런스는 조각난 스토리보드를 연결해주는 메타데이터라고 생각하면 됩니다.

두번째 씬을 선택하고 Editor-> Refacter to Storyboard를 클릭합니다.
그러면 스토리보드 이름을 설정하는데 Sub.storyboard로 하고 저장하겠습니다.
프로젝트 네비게이션에 Sub.storyboard파일이 생성되고 기존 스토리보드에서 두번째 씬이 그림과 같이 작아진것을 볼 수 있습니다.
이게 스토리보드 레퍼런스입니다. 
눌러서 속성 인스펙터를 보면 스토리보드는 Sub, 레퍼런스 ID는 secondViewController로 되어있는 것을 확인할 수 있습니다.
![5.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/5.png)

그리고 sub스토리보드로 이동하면 두번째 씬이 그대로 옮겨와있습니다.
이대로 실행하면 에러를 뱉습니다. 
initial View Controller 설정을 해줘야합니다.
initial View Controller는 해당 스토리보드의 첫 진입 뷰 컨트롤러를 지정합니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/6.png)

코드로 이동 할때에 스토리보드가 바뀌었기 때문에 조금 수정이 필요합니다.
첫번째 씬에서 Code버튼에 연결한 액션으로 이동하여 아래처럼 수정해줍니다.
```swift
\ @IBAction func moveSecondButton(_ sender: Any) {
      //  guard let second = storyboard?.instantiateViewController(withIdentifier: "secondViewController") else { return }
      //  present(second, animated: true, completion: nil)
        
        let sub = UIStoryboard(name: "Sub", bundle: nil)
        let second = sub.instantiateViewController(withIdentifier: "secondViewController")
        present(second, animated: true, completion: nil)
    }
```
기존에는 같은 스토리보드에서 뷰 컨트롤러를 찾았습니다. 
storyboard 속성은 Main스토리보드의 속성이기에 Sub스토리보드 객체를 만들어야합니다.
`UIStoryboard()`는 네임과 번들에 맞는 스토리보드를 찾아줍니다. 
네임에 확장자를 제외한 이름을 적어주겠습니다 번들은 현재 프로젝트 번들이 나눠져있지 않기때문에 nil을 적습니다.
그리고 sub객체에서 기존과 같은 방법으로 뷰 컨트롤러를 찾아주고 present로 화면에 모달로 띄어주면 되겠습니다.
이제 실행해서 확인해보면 잘 실행되는것을 확인 할 수 있습니다.

## 스토리보드 만들고 레퍼런스로 연결하기
위에서는 이미 세그웨이로 연결된 뷰 컨트롤러를 바로 레퍼런스로 만들었습니다.
이번에는 스토리보드를 만들고 이를 레퍼런스로 만드는 방법을 알아보겠습니다.

첫번째 씬에 새로운 버튼을 추가하고 Move Third라고 적겠습니다.
새로운 스토리보드를 생성합니다. New에 File를 클릭하여 스토리보드를 선택해줍니다. 이름을 Third.storyboard로 하겠습니다.
![7.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/7.png)

라이브러리 오브젝트에서 스토리보드 레퍼런스를 찾아서 추가해줍니다.
그럼 화면에 스토리보드 레퍼런스가 추가되고 세그웨이로 연결을 해줍니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/8.png)

스토리보드와 레퍼런스ID를 적어줍니다.
스토리보드는 Third, 레퍼런스ID는 뷰컨트롤러의 스토리보드ID인 thirdViewController를 적어줍니다.
![9.png](https://MinominoDomino.github.io/assets/img/ios/StoryboardReference/9.png)

코드로 하는 방법은 위에서 코드로 하는 방법과 동일합니다.
단지, 스토리보드만 바뀌었으니 Third스토리보드 객체를 만들고 thirdViewCOntroller객체를 찾아 화면에 모달로 띄어주면되겠습니다.
실행하고 확인하면 잘되는 것을 볼 수 있습니다.
이렇게 스토리보드 레퍼런스에 대해 알아 봤습니다.

## 참조
[Apple Developer document - UIStoryboard](https://developer.apple.com/documentation/uikit/uistoryboard)
[Apple Developer document - UIViewContoller](https://developer.apple.com/documentation/uikit/uiviewcontroller)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store/tree/master/kxcoding/StoryboardReference)

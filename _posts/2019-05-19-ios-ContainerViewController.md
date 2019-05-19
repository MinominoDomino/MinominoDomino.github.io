---
layout: post
title:  "[iOS] 컨테이너 뷰 컨트롤러(Container View Controller)"
subtitle:   "iOS 컨테이너 뷰 컨트롤러"
categories: devlog
tags: ios

---

컨테이너 뷰 컨트롤러(Container View Controller)를 알아보자.

오늘은 컨테이너 뷰 컨트롤러에 대해 알아보겠습니다.
- Container View Contoller생성 
- Custom COntainer View Controller구현 
- 스토리보드에서 Child VC지정 
- 뷰 컨트롤러 계층 관리 
- 계층 이동 이벤트 처리

## 컨텐츠 뷰 컨트롤러와 컨테이너 뷰 컨트롤러
뷰 컨트롤러는 `컨텐츠 뷰 컨트롤러`와 `컨테이너 뷰 컨트롤러`가 있습니다.
화면을 구성하는 뷰를 직접 구현하고 관련된 이벤트를 처리하는 뷰 컨트롤러를 컨텐츠 뷰 컨트롤러라고 합니다.
반대로 컨테이너 뷰 컨트롤러는 하나이상의 뷰컨트롤러를 지니고 이 뷰컨트롤러를 차일드 뷰 컨트롤러라고 합니다.
이런 하나 이상의 차일드 뷰 컨트롤러를 관리하고 레이아웃과 트랜지션을 담당합니다. 화면 구성과 이벤트 관리는 차일드 뷰가 합니다.
컨테이너 뷰 컨트롤러는 대표적으로 다음과 같은 종류가 있습니다.
- 네비게이션 컨트롤러
- 탭바 컨트롤러
- 시스템 컨테이너 뷰 컨트롤러

![1.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/1.png)

## 컨테이너 뷰 컨트롤러 해보기
다음 그림 처럼 뷰 컨트롤러 작업을 해주겠습니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/2.png)

오브젝트 라이브러리에서 `Container View`를 찾아서 ViewContoller화면에 놓습니다.
그리고 `View`를 찾아서 ViewContoller화면에 놓습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/3.png)

컨테이너 뷰를 놓으면 자동으로 뷰 컨트롤러를 생성해주는데, 여기서는 TOPViewContoller와 BottomViewContoller를 ViewContoller의 위 아래로 배치 하도록 하겠습니다. 그래서 자동으로 생긴 뷰는 삭제 해주세요.
컨테이너뷰에서 오른쪽 클릭하고 탑뷰컨트롤러를 선택하면 Embed가 나오는데 누르게 되면 탑뷰컨트롤러가 컨테이너뷰의 루트뷰가 됩니다.
세그웨이를 이용하여 가장 쉽게 컨테이너뷰를 만드는 방법입니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/4.png)

오토레이아웃이나 수동으로 버튼을 화면 맨위에 두고 남은 공간을 컨테이너뷰와 뷰가 나눠갖도록 화면 구성을 합니다.
그리고 그림처럼 버튼의 액션과 뷰의 아웃렛을 연결해 줍니다.
![5.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/5.png)

viewDidLoad()에서 bottom컨트롤러의 ID를 이용하여 해당 뷰 컨트롤러를 찾은 다음 `addChild()`로 컨테이너의 child로 추가해줍니다.
단순히 addchild로 화면에 나타나는 것은 아니기에 직접 프레임을 설정하고 뷰 계층에 추가해줘야 합니다. 현재 View의 사이즈를 bottomView에 맞춰주고 `addSubView()`로 bottomView의 실제 뷰를 붙여줍니다.

```swift
	guard let bottomView = storyboard?.instantiateViewController(withIdentifier: "BottomContainerView") 
    	else { return }
        addChild(bottomView)
        
        bottomView.view.frame = bottomContainerView.bounds
        bottomContainerView.addSubview(bottomView.view)
```
위 같이 작업을 해주고 실행을 하면 처음 만든 두개의 뷰가 메인인 뷰컨트롤러에 위 아래로 붙여져서 나오는걸 볼 수 있습니다.
이렇게 컨테이너 뷰 컨트롤러를 커스텀하여 만들 수 있습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/6.png)

## 컨테이너 뷰 컨트롤러에서 뷰 삭제하기
이번엔 컨테이너 뷰 컨트롤러가 관리하는 뷰를 삭제 해보겠습니다.
보통 차일드 뷰를 제거할때에는 차일드 클래스에서 제거를 합니다. 하지만, 컨테이너에서도 제거를 할 수 있습니다.
바텀뷰컨트롤러에서 버튼의 액션을 잡아주겠습니다.
![7.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/7.png)

버튼을 클릭하면 컨테이너에서 해당 뷰를 삭제하겠습니다. 삭제를 할때에는 먼저 루트뷰를 제거하고 컨테이너에서 제거합니다.
```swift
 @IBAction func removeViewButton(_ sender: Any) {
        view.removeFromSuperview()
        removeFromParent()
    }
```

이번엔 컨테이너에서 뷰를 삭제해보겠습니다. 
컨테이너뷰는 차일드뷰를 배열로 관리하고있습니다. 해당 배열을 순회하면서 차일드뷰의 루트뷰를 제거하고 컨테이너에서 제거합니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/8.png)
```swift
 @IBAction func childRemoveButton(_ sender: Any) {
        for vc in children {
            vc.view.removeFromSuperview()
            vc.removeFromParent()
        }
    }
```

실행을 해보면 뷰가 컨테이너 뷰에서 제거됨을 볼 수 있습니다.

## 메서드 호출 차이

`willMove()`나 `didMove()`는 차일드 뷰가 새로 생기거나 삭제될 때 불리는 메서드입니다.
세그웨이로 연결한 차일드뷰와 코드로 연결한 차일드뷰의 메서드 호출 차이를 보기 위해 top컨트롤러와 bottom컨트롤러에 각 메서드를 오버라이드 합니다.
그리고 print()로 로그를 남겨보겠습니다.

TopViewContoller.swift
```swift
 override func willMove(toParent parent: UIViewController?) {
        super.willMove(toParent: parent)
        print("Will Move in Top")
    }
 override func didMove(toParent parent: UIViewController?) {
        super.didMove(toParent: parent)
        print("did Move in Top")
    }
```

BottomViewContoller.swift
```swift
 override func willMove(toParent parent: UIViewController?) {
        super.willMove(toParent: parent)
        print("Will Move in BottomView")
    }
 override func didMove(toParent parent: UIViewController?) {
        super.didMove(toParent: parent)
        print("did Move in BottomView")
    }
```

실행을 해서 로그를 확인해봅니다.
![9.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/9.png)

세그웨이로 연결한 top뷰에서는 willMove와 didMove가 전부 호출되지만, bottom뷰에서는 didMove가 호출되지 않습니다.
코드로 작업한 경우 didMove를 직접 호출 해줘야합니다.
```swift
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        guard let bottomView = storyboard?.instantiateViewController(withIdentifier: "BottomContainerView") 
         else { return }
        addChild(bottomView)
        
        bottomView.didMove(toParent: self)	// didMove를 호출해 줍니다.
        bottomView.view.frame = bottomContainerView.bounds
        bottomContainerView.addSubview(bottomView.view)
    }
```

직접 호출하고 결과를 보겠습니다.
![10.png](https://MinominoDomino.github.io/assets/img/ios/ContainerViewController/10.png)

didMove()가 정상 호출 됩니다.
이렇게 컨테이너 뷰 컨트롤러를 구성하는데 있어 보이는 메서드 콜의 차이점 까지 알아 봤습니다.


## 참조
[Apple Developer document - UIViewContoller](https://developer.apple.com/documentation/uikit/uiviewcontroller)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store/tree/master/kxcoding/ContainerViewController)

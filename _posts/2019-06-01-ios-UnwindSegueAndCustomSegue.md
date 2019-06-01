---
layout: post
title:  "[iOS] Unwind세그웨이와 Custom세그웨이(Unwind Segue And Custom Segue)"
subtitle:   "iOS Unwind세그웨이, Custom세그웨이"
categories: devlog
tags: ios

---

Unwind세그웨이와 Custom세그웨이(Unwind Segue And Custom Segue)를 알아보자.

오늘은 Unwind세그웨이와 Custom세그웨이에 대해 알아보겠습니다.
- Unwind Segue 연결
- Unwind Segue 제어


## Unwind Segue
이전 세그웨이 포스트에서 간단하게 Unwind Segue에 대해 적었습니다.
Unwind Segue는 이전 화면으로 돌아갈때 쓴다고 했는데요.
기본적인 세그웨이는 Source에서 Destination으로 전환을 제공하지만 Destination에서 Source로는 불가능합니다.
이를 가능하게 하기위해 Unwind Segue를 제공합니다.
이 세그웨이는 기본적인 세그웨이랑은 구현하는 방식이 다릅니다.

뷰 컨트롤러와 연결하지 않고 EXIT랑 연결을 하게됩니다.
Source 뷰 컨트롤러에서 특정 메서드를 구현합니다.

Unwind Segue의 과정을 살펴보겠습니다.
먼저 Destination에서 `shouldPerformSegue()`를 호출하여 세그웨이 실행 여부를 확인합니다.
보통 shouldPerformSegue()에서는 특정 화면 전환에 대한 제어를 하게되고 상태에 따른 실행 여부를 컨트롤합니다.
이후 Source의 `canPerformUnwindSegueAction()`이 호출되고 unwind되는 뷰를 찾아서 unwind 액션에 대한 응답 여부를 리턴하게 됩니다.
만일 false를 리턴하면 unwind가 되지 않습니다.
그리고 Destination에서 `prepare()`메서드가 실행되고 Unwind의 액션이 실행됩니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/1.png)

## Unwind Segue 연결 및 제어
먼저 그림과 같이 3개의 뷰 컨트롤러를 만들어 줍니다.
각 뷰의 버튼으로 세그웨이 전환을 시켜주고 View 3에서는 Unwind로 View 1과 VIew 2로 이동하겠습니다.
![2.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/2.png)

View1 Controller와 View2 Controller에서 Unwind라고 치면 소스 블록이 나오게됩니다.
해당 메서드를 전부 오버라이드 시켜주겠습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/3.png)
![4.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/4.png)

다시 View 3에서 Unwind to View1 버튼과 Unwind to View2 버튼에 Unwind Action을 연결해주겠습니다.
이때 Sence1과 Sence2에 연결을 하면 결과적으로는 동일하게 보일 수 있지만, 새로운 뷰를 위에 다시 띄우는 것이기때문에 연결을 할때 뷰컨트롤러에 있는 EXIT에 연결을 해줘야 합니다.
연결을 하면 Sence1과 Sence2에서 오버라이드한 Unwind메서드가 보이게되고 버튼에 맞게 연결을 해줍니다.
정상 연결이 되면 왼쪽 뷰화면에 Unwind 액션이 보이게됩니다. 
![5.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/5.png)

실행 후 확인해보면 정상적으로 Unwind액션이 되는 것을 볼 수 있습니다.

이번엔 각 메서드 콜을 확인해보겠습니다.
Source 뷰 컨트롤러에서 canPerformUnwindSegueAction()를 오버라이드하고 로그를 남기겠습니다.
Unwind 액션에도 로그를 남기겠습니다.
```swift
 @IBAction func unwindToView2(_ unwindSegue: UIStoryboardSegue) {
         print(#function, type(of: unwindSegue.source), "->", type(of: unwindSegue.destination))
    }
    
    override func canPerformUnwindSegueAction(_ action: Selector, from fromViewController: UIViewController, withSender sender: Any) -> Bool {
        print(#function, type(of: self))
        return true
    }
```

Destination 뷰 컨트롤러에서 shouldPerformSegue()와 prepare()를 오버라이드하고 로그를 남기겠습니다.
```swift
 override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
        print(#function, type(of: self))
        return true
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        print(#function, type(of: self))
    }
```

실행을 하고 출력창을 확인해보겠습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/6.png)

Unwind Segue를 설명할때 말한대로 View3Controller에서 shouldPerformSegue()를 호출하고 View2Controller에서 canPerformUnwindSegueAction()이 호출됩니다.
이후, View3Controller에서 prepare()가 실행되고 화면이 View3에서 View2로 unwind되는 것을 볼 수 있습니다.




## 참조
[Apple Developer document - UIViewcontroller](https://developer.apple.com/documentation/uikit/uiviewcontroller)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store/tree/master/kxcoding/UnwindAndCustomSegue)

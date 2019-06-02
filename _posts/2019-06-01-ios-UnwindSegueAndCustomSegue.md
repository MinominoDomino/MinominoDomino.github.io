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
- Custom Segue 연결
- Unwind Custom Segue 연결


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

## Custom Segue
커스텀 세그웨이는 앞서 말한 일반적인 세그웨이와 Unwind 세그웨이는 일반적인 트랜지션을 지원합니다.
사용자가 원하는 트랜지션을 하는 세그웨이는 Custom으로 구현해야합니다.
Custom Segue는 `UIStoryboardSegue클래스`에 구현되어있습니다.
해당 클래스를 서브클래싱하여 `perform()`를 오버라이드하여 구현합니다.

## Custom Segue 및 Unwind Custom Segue 연결
먼저 커스텀 세그웨이를 구현할 클래스를 만들어 줍니다.
코코아 프레임워크에서 UIStoryboardSegue를 서브클래싱하는 CustomSegue클래스를 만들겠습니다.
![7.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/7.png)

이후 그림과 같이 화면을 구성하겠습니다.
새로운 뷰 컨트롤러를 추가하고 3번째 씬의 버튼과 세그웨이로 연결합니다.
속성 인스펙터에 클래스를 CustomSegue로 변경하고 kind에 세그웨이 종류를 Custom으로 변경합니다.
커스텀 세그웨이의 세그웨이 아이콘이 변경되는 것을 볼 수 있습니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/8.png)

커스텀 세그웨이를 위해서 두개의 메서드를 오버라이드 합니다.
첫번째, `init()`입니다. 
이 메서드에서는 초기화를 위한 오버라이드 메서드이며, 정상적인 세그웨이 호출을 위하여 상위 클래스의 init()를 호출해야 합니다. 

두번째, `perform()`입니다.
이 메서드에서는 실제 커스텀 세그웨이의 트랜지션을 구현합니다.
화면 전환의 애니메이션이 등 모든 것을 직접 구현해야합니다.

아래 처럼 init과 perform을 오버라이드하여 예제소스를 사용하겠습니다.

```swift
    override init(identifier: String?, source: UIViewController, destination: UIViewController) {
        super.init(identifier: identifier, source: source, destination: destination)
    }
    
    override func perform() {
        var frame = source.view.bounds
        frame.origin.y = frame.height
        frame.size.height = frame.height / 2
        
        source.view.addSubview(destination.view)
        destination.view.frame = frame
        destination.view.alpha = 0.0
        
        source.addChild(destination)
        
        frame.origin.y = source.view.bounds.height / 2
        
        UIView.animate(withDuration: 0.3) {
            self.destination.view.frame = frame
            self.destination.view.alpha = 1.0
        }
    }
```

실행하여 확인하면 화면의 반을 새로운 뷰가 출력됩니다.
![9.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/9.png)

하지만 내리기 버튼을 통한 Unwind Segue를 위해서는 Custom에서는 직접 구현을 해줘야합니다.
Custom Unwind Segue를 위해 CustomSegue클래스를 서브클래싱하는 CustomSegueUnwind클래스를 생성합니다.
![10.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/10.png)

`perform()`를 오버라이드하고 트랜지션을 구현합니다.
예제소스를 따라하겠습니다.

```swift
override func perform() {
        var frame = source.view.frame
        frame = frame.offsetBy(dx: 0, dy: frame.height)
        
        UIView.animate(withDuration: 0.3, animations: {
            self.source.view.frame = frame
            self.source.view.alpha = 0.0
        }, completion: { finished in
            self.source.view.removeFromSuperview()
            self.source.removeFromParent()
        })
    }
```

View3에 Unwind 세그웨이 액션을 만들고 내리기 버튼과 연결해줍니다.
클래스를 CustomSegueUnwind로 변경하고 실행하여 확인하겠습니다.
![11.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/11.png)


정상적으로 커스텀 세그웨이로 화면이 전환되는 것을 볼 수 있습니다.
![12.png](https://MinominoDomino.github.io/assets/img/ios/UnwindAndCustomSegue/12.png)



## 참조
[Apple Developer document - UIViewcontroller](https://developer.apple.com/documentation/uikit/uiviewcontroller)
[Apple Developer document - UIStoryboardSegue](https://developer.apple.com/documentation/uikit/uistoryboardsegue)



###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store/tree/master/kxcoding/UnwindAndCustomSegue)

---
layout: post
title:  "[iOS] 탭바 컨트롤러(TabBar Controller)"
subtitle:   "iOS 탭바 컨트롤러"
categories: devlog
tags: ios

---

탭바 컨트롤러(TabBar Controller)를 알아보자.

오늘은 컨테이너 뷰 컨트롤러에 대해 알아보겠습니다.
- 탭바 컨트롤러 구조
- 탭 바 아이템
- 탭 선택 제어
- 탭 선택 이벤트 처리

## 탭바 컨트롤러와 탭 바 아이템
`탭바 컨트롤러`는 탭으로 뷰컨트롤러를 차일드로 설정하여 관리하는 컨테이너 컨트롤러입니다.
탭바 컨트롤러에는 루트뷰와 탭 바가 표시되고 차일드 뷰 갯수 만큼 `탭 바`에 `탭 바 아이템`이 생성됩니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/1.png)

## 탭바 만들기
화면과 같이 스토리보드에서 뷰를 먼저 만들어 주겠습니다.
![2.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/2.png)

기본적으로 탭바 컨트롤러가 루트뷰를 생성해주는데 해당 루트 뷰는 삭제해주세요.
네비게이션 컨트롤러에서 마우스오른쪽 클릭 후 드래그하여 Modal로 탭바 컨트롤러가 화면에 출력되도록 연결해주세요.
![3.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/3.png)

마찬가지 방법으로 탭바 컨트롤러에서 마우스 오른쪽 클릭 후 드래그하여 각 탭으로 사용할 뷰 컨트롤러에 두면 RelationShip Segue에 View Controller를 클릭합니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/4.png)

나머지 뷰들도 동일하게 작업하면 탭바 컨트롤러에 추가한 만큼 탭바 아이템이 생성되고 각 뷰 컨트롤러에 탭바가 생성되는 것을 확인 할 수 있습니다.
![5.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/5.png)

실행하고 확인해봅시다.
각 탭을 누르면 설정한 화면이 루트뷰로 설정되는 것을 볼 수 있습니다.
간단하게 segue를 이용하여 탭바 컨트롤러를 만들었습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/6.png)

이번엔 코드로 탭바를 만들어보겠습니다.
메인 뷰로 이동하여 Code버튼에 액션을 걸어줍니다.
`storyboard?.instantiateViewController()` 로 각 탭에 쓰일 차일드 뷰를 만들어 줍니다.
탭바 컨트롤러 객체를 생성하고 `setViewControllers()`로 차일드 뷰를 붙여줍니다.
탭바 컨트롤러는 차일드 뷰를 배열로 관리하고 있습니다.
그리고 `present()`로 화면에 출력해줍니다.
![7.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/7.png)

```swift
    @IBAction func codeButton(_ sender: Any) {
        guard let first = storyboard?.instantiateViewController(withIdentifier: "FirstViewController") else { return }
        guard let second = storyboard?.instantiateViewController(withIdentifier: "SecondViewController") else { return }
        guard let third = storyboard?.instantiateViewController(withIdentifier: "ThirdViewController") else { return }
        guard let fourth = storyboard?.instantiateViewController(withIdentifier: "FourthViewController") else { return }
        
        let tb = UITabBarController()
        tb.setViewControllers([first, second, third, fourth], animated: true)
        present(tb, animated: true, completion: nil)
    }
```

실행하고 확인해봅시다.
처음 segue로 만든것과 동일하고 작동하고있습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/6.png)

## 탭바 아이템 바꾸기
각 탭바의 아이템들의 타이틀이나 색상, 이미지등은 차일드 뷰에 탭바에서 설정해줍니다.
변경을 원하는 차일드 뷰로 이동하여 탭바를 클릭합니다.
그리고 속성 인스펙터를 보면 기본적으로 시스템 아이템이 `Custom`으로 설정되어 있습니다.
애플에서 제공하는 기본 아이템을 사용 할 수도 있지만 속성 값을 변경하면 다시 Custom으로 돌아갑니다.
예제로 기본으로 제공하는 아이템을 선택해보겠습니다.
그리고 `badge`는 보통 메시지에서 많이 볼 수 있습니다. 읽지 않은 메시지가 있을 경우 해당 숫자만큼 화면에 표시해주는데
이 속성이 badge입니다.
직접 탭바의 여러 속성을 눌러보면서 결과를 보는게 빠릅니다!
![9.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/9.png)


## 탭바 선택 제어
탭바의 아이템을 누르면 선택된 탭바 아이템의 루트뷰를 보여줍니다.
이 기능을 코드로 구현해보겠습니다.
FirstViewController로 이동하여 각 버튼의 액션을 잡아줍니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/8.png)

탭바 컨트롤러는 차일드뷰를 배열로 관리하고 있습니다.
첫번째 방법으로 뷰 선택을 직접 할 수 있습니다.
먼저 내가 이동할 뷰를 하나 만들어줍니다. 이때에는 탭바컨트롤러에서 관리하고 있는 차일드 뷰에 직접 접근해보겠습니다.
tabBarController.viewControllers는 차일드뷰의 배열을 나타냅니다. 이 배열의 1번째 요소는 2번쨰 화면입니다.
그리고 `selectedViewController`에 직접 찾은 뷰를 할당해 줍니다.

두번째 방법으로 뷰의 인덱스를 지정해주는 방법입니다.
탭바 컨트롤러가 선택된 뷰의 인덱스번호를 직접 지정해주면 해당 인덱스를 가진 뷰를 선택해줍니다.
`selectdIndex`에 인덱스요소를 직접 주겠습니다.

```swift
	@IBAction func moveSecondButton(_ sender: Any) {
        guard let second = tabBarController?.viewControllers?[1] else { return }
        tabBarController?.selectedViewController = second
    }
    
    @IBAction func moveThirdButton(_ sender: Any) {
        tabBarController?.selectedIndex = 2
    }
```

실행하고 확인해보면 두번째 화면과 세번째 화면이 표시되는 것을 볼 수 있습니다.
이렇게 코드로 탭바 컨트롤러가 관리하는 차일드 뷰를 직접 제어할 수 있습니다.


## 탭바 이벤트 처리
탭바 컨트롤러는 델리게이트를 제공합니다.
새로운 코코아클래스파일을 생성하겠습니다. 그리고 탭바 컨트롤러의 클래스를 생성한 클래스 컨트롤러로 변경해줍니다.
![10.png](https://MinominoDomino.github.io/assets/img/ios/TabBarController/10.png)

CustomTabBarController.swift에서 extension으로 `UITabBarControllerDelegate`를 채택해줍니다.
`tabBarController(tabBarController:, shouldSelect viewController:)`는 탭을 터치하는 순간 발생하고 이 메서드를 이용하여 지정한 탭의 활성 여부를 동적으로 결정 할 수 있습니다.
기본적으로 True가 반환되고 True는 활성화, False는 비 활성화입니다.

마지막 탭을 비활성화 하겠습니다. 네번째 차일드를 직접찾아서 매개변수로 넘어오는 뷰와 같지 않으면 true 같으면 false를 리턴합니다.
```swift
	extension CustomTabBarController: UITabBarControllerDelegate {
    func tabBarController(_ tabBarController: UITabBarController, shouldSelect viewController: UIViewController) -> Bool {
        if let fourth = tabBarController.viewControllers?[3] {
            return viewController != fourth
        }
        return true
    }
    
    func tabBarController(_ tabBarController: UITabBarController, didSelect viewController: UIViewController) {
    	print("tab selected")    
    }
}
```
tabBarController(_ tabBarController:, didSelect viewController:) 는 탭이 선택되면 메서드가 호출됩니다.
간단히 선택되었다는 로그만 찍겠습니다.
실행하고 확인해보면 마지막 탭은 선택이 되지않습니다.

간단하게 탭바 컨트롤러에 대해 알아봤습니다.

## 참조
[Apple Developer document - UITabBarController](https://developer.apple.com/documentation/uikit/uitabbarcontroller)
[Apple Developer document - UITabBarItem](https://developer.apple.com/documentation/uikit/uitabbaritem)
[Apple Developer document - UITabBarControllerDelegate](https://developer.apple.com/documentation/uikit/uitabbarcontrollerdelegate)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store/tree/master/kxcoding/TabBarController)

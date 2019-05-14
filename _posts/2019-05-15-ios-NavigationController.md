---
layout: post
title:  "[iOS] 네이버 검색API 사용하기(NaverAPI)"
subtitle:   "iOS NaverAPI"
categories: devlog
tags: ios

---

네이버 검색API 사용하기(NaverAPI)를 알아보자.

오늘은 네비게이션 컨트롤러 사용하는법을 알아보겠습니다.
네비게이션 컨트롤러는 뷰컨트롤러의 일종으로 `네이게이션 바`가 같이 쓰입니다.
네비게이션은 직접적으로 화면을 구성하지 않고 `루트 뷰 컨트롤러` 라는 뷰 컨트롤러를 시작점으로 하여 표시합니다.
따라서, 네비게이션 컨트롤러는 여러 뷰 컨트롤러를 계층적으로 관리만 하는 컨트롤러입니다.
그리고 관리되는 모든 뷰 컨트롤러에 네비게이션 바가 생성됩니다.
관리되는 모든 뷰를 `스택 구조`로 관리 합니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/1.png)

## 네비게이션 컨트롤러의 뷰 관리
네비게이션 컨트롤러는 스택형식으로 뷰를 관리한다고 했습니다.
내부적으로 배열에 뷰를 관리를 하게되는데 스택 맨 처음에 들어가는 뷰가 루트 뷰 컨트롤러입니다.
그리고 그화면에서 다음 화면이 추가되면 루트 뷰 컨트롤러 위에 쌓이게 되고 반대로 뷰 컨트롤러가 배열에서 나가면서
이전 뷰를 보여주는 형식입니다.
그림으로 보겠습니다.
![2.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/2.png)

실제 화면에 출력되는 뷰는 스택의 가장 위에 있는 뷰가 화면에 출력됩니다.
뷰 컨트롤러를 네비게이션 컨트롤러 스택에 넣는 메서드는 `pushViewController()` 빼는 메서드는 `popViewContoller()`입니다.
처음 화면1을 넣게되면 루트뷰 컨트롤러이고, 다른 뷰 컨트롤러를 푸쉬하면서 화면2가 출력됩니다.
그리고 뷰 컨트롤러를 푸쉬하면서 화면3이 나올겁니다.
반대로, 뷰컨트롤러를 pop하면서 화면3이 스택에서 나와서 화면2가 출력되고 또 pop을 하면서 화면1이 출력될 것 입니다.

## 따라하기
실제로 네비게이션 컨트롤러를 간단한 예제로 써보겠습니다.
소스로 뷰 컨트롤러를 제어하는것과 세그웨이를 이용한 방법이 혼합되어 있으니 자세히 봐주세요!
먼저 새로운 프로젝트를 만들고 다음과 같이 스토리보드를 꾸며보겠습니다.
뷰와 뷰 사이에 연결된 선과 다른것들은 이후에 작업되니 신경쓰지않으셔도됩니다.
UIBarButtonItem을 제외하고 뷰컨트롤러와 레이블, 버튼만 배치해주시고 클래스 지정과 이름만 하시면됩니다.
뷰 컨트롤러가 화면에 3개가 더 들어가게되니, cacoa class로 UIViewContoller.swift 파일을 3개 더 추가해주셔야합니다.
이름은 마음대로 짓고 각 `화면별로 꼭 컨트롤러 지정` 을 해주세요.
그리고 예제에서 화면3이라는 뷰컨트롤러에서 `Identity에서 Storyboard ID` 를 설정해주겠습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/3.png)

먼저, 네비게이션 컨트롤러에서 화면을 어떻게 관리하는지 보겠습니다.
첫번째 뷰컨트롤러를 선택하고 `Editor->Embed in->Navigation Controller` 가 있습니다.
누르게되면 그림처럼 선택한 뷰 컨트롤러에 앞에 네비게이션 컨트롤러가 붙게됩니다.
다른 방법으로 캔버스 하단에 그림과 같은 아이콘을 누르면 네비게이션 컨트롤러를 추가할 수 있습니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/4.png)
![5.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/5.png)

네비게이션 컨트롤러가 추가되면 화면1은 네비게이션 컨트롤러의 루트뷰컨트롤러가 됩니다.
그리고 화면 상단에 자동으로 `네비게이션 바`를 추가해줍니다.
네비게이션 바안에 이제 `UIBarButtonItem`을 추가하겠습니다.
추가한 바아이템을 잡고 오른쪽 클릭하여 화면2로 드래그합니다. `ActionSegue`에 show와 present modally가 보입니다.
show로 한번 추가해보겠습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/6.png)

화면2에도 화면1과 같이 네비게이션 바가 생기고 자동으로 <화면1 이라는 아이템이 생겼습니다.
이건 네비게이션 컨트롤러가 관리하는 뷰 컨트롤러라고 인식하여 자동으로 네비게이션바를 추가해준 모습니다.
그럼 이번엔 present modally로 해보겠습니다.
이번엔 네비게이션 바가 없네요.
![7.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/7.png)

실제로 시뮬레이터에서 두가지를 비교해보면 show는 네비게이션 바가 있어서 이전화면으로 되돌아 갈 수 있지만, present modally는 이전화면으로 돌아갈수가 없을거에요.
그럼 무조건 네비게이션 바에 바버튼아이템을 넣어서해야할까요?
아닙니다. 화면2에 추가한 두 버튼의 Action을 추가하겠습니다. 
![8.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/8.png)

첫 번째 버튼의 액션안에 다음의 코드를 입력합니다.
현재 스토리보드에서 NextView라는 아이디를 가진 뷰를 찾아 nextView변수에 할당합니다. 그럼 nextView는 우리가 이동할 뷰가 되겠죠?
여기에서 뷰를 찾기위해 `identifier`를 사용하는데 화면 디자인 할때 아이디를 넣어달라고 했죠?? 그때 적은 아이디를 넣으시면됩니다.
그리고 네비게이션컨트롤에 `pushViewContoller()`로 찾은 뷰 컨트롤러를 넘겨주면 끝입니다.
```swift
@IBAction func moveNextViewWithNavigation(_ sender: Any) {
        guard let nextView = self.storyboard?.instantiateViewController(withIdentifier: "NextView") else {
            return
        }
        
        self.navigationController?.pushViewController(nextView, animated: true)
    }
```

두 번째 버튼은 네비게이션 컨트롤러를 사용하지 않고 다음 화면으로 넘거가겠습니다.
똑같이 넘어갈 뷰를 찾아주시고요
`present()`로 찾은 뷰 컨트롤러를 넘겨줍니다.
```swift
@IBAction func moveNextView(_ sender: Any) {
	uard let nextView = self.storyboard?.instantiateViewController(withIdentifier: "NextView") else {
		return
	}
	present(nextView, animated: true, completion: nil)
}
```

이제 실행을 해보게되면 첫 번쨰 버튼에서는 네이게이션 바가 있어서 뒤로 갈 수 있지만, 두 번쨰 버튼에서는 뒤로 갈 수가 없을거에요.
이렇게 네비게이션 컨트롤러가 관리해주는 뷰 컨트롤러들은 네비게이션 스택에서 넣고 빠질 수 있도록 자동으로 해줍니다.
중요한 것은 화면을 띄우는 방식을 뷰 컨트롤러을 통해서 하냐? 아니냐 차이입니다.
이번엔 화면3에 있는 back버튼을 알아볼게요.
![9.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/9.png)

버튼에 액션을 걸어줍니다.
다음과 같이 코딩해주겠습니다.
```swift
@IBAction func movePreview(_ sender: Any) {
    dismiss(animated: true, completion: nil)
    
    //self.navigationController?.popViewController(animated: true)
}
```

화면2에서 화면3으로 네비게이션 컨트롤러 없이 이동하면 뒤로 갈 수가 없었죠.
back버튼을 통해 현재 화면을 없애도록 dimiss로 현재 뷰를 없애줍니다.
실행 해보면 화면3이 사라지고 화면2가 나타날거에요.

그리고 네비게이션 컨트롤러로 화면3으로 이동했다면 back버튼을 눌러도 아무 효과가 없을거에요.
왜그럴까요? 당연히 해당 뷰가 네비게이션 컨트롤러의 스택안에 있기 때문이에요.
빼낼래면 `popViewContoller()`를 호출해야한다고 했습니다.
dismiss를 주석하고 popViewContoller()를 적용하여 실행하면 네비게이션 바의 back버튼과 동일한 결과를 보일거에요.

이렇게 코드로 네비게이션뷰 컨트롤러 유/무에 따른 뷰를 추가하고 빼는 방법를 알아봤습니다.

가장 간단한 segue를 사용해볼게요.
이미 barbuttonItem을 하면서 해봤죠 동일하게 버튼을 잡고 SegueScene로 드래그합니다.
show로 하면 네비게이션 컨트롤러에 추가되고 네비게이션 바가 생긴다고 했죠?
"엥!? 저는 안보이는데요?"
원래 안보이는게 맞습니다! 실행을 하면 뷰를 그리면서 네비게이션 컨트롤러가 자동으로 넣어줄거에요. 
단, 무조건 show로 한다고 네비게이션에 붙는건아니에요. 
네비게이션 컨트롤러를 사용하고 있으니!! 네비게이션 뷰에 추가되는 컨트롤러라고 생각하는거에요
네비게이션을 사용하지 않는 뷰에서 show로 이어주면 기본적으로 modal로 띄어지게 됩니다.
![10.png](https://MinominoDomino.github.io/assets/img/ios/NavigationContoller/10.png)

그리고 아래버튼은 modal로 해주겠습니다.
여기서 눈치있으신분들은 segue로 연결된 선이 다르다는걸 눈치채셨을거에요.
그리고 SegueScene에서 modal로 전환했을때 뒤로 갈 수 있는 back버튼을 작성해줍니다.
```swift
    @IBAction func BackButton(_ sender: Any) {
        dismiss(animated: true, completion: nil)
    }
```

이제 실행해서 우리가 생각한대로 동작하는지 확인해보죠!
저는 잘 동작하네요.
이렇게 기본적으로 네비게이션 컨트롤러에 대해 알았고 화면 전환에 대해서도 알아봤습니다.

## 참조
[Apple Developer document - UINavigationContoller](https://developer.apple.com/documentation/uikit/uinavigationcontroller)
[Apple Developer document - UINavigationBar](https://developer.apple.com/documentation/uikit/uinavigationbar)
[Apple Developer document - UIbarButtonItem](https://developer.apple.com/documentation/uikit/uibarbuttonitem)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store)

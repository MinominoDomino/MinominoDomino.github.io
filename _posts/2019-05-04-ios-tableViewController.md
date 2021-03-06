---
layout: post
title:  "[IOS] 테이블 뷰 컨트롤러(TableViewController)"
subtitle:   "IOS 테이블 뷰 컨트롤러"
categories: devlog
tags: ios

---

테이블 뷰 컨트롤러(TableViewController)를 알아보자.

## 테이블 뷰 컨트롤러(TableViewController)
테이블 뷰는 여러 데이터를 리스트화하여 사용자에게 보여 주는 뷰입니다.
우리 생활 곳곳에 사용되고 있죠!

가장 쉽게 지금 아이폰을 켜서 설정을 누르면 나오는 화면이 테이블 뷰로 이루워져 있습니다.

<img src="https://MinominoDomino.github.io/assets/img/ios/viewController/16.PNG" width="20%">

이러한 테이블 화면을 만드는데 IOS에서 두가지 방법이 있습니다.
1. UITableViewController를 이용한 방법
2. UIViewController를 이용한 방법

차이는 테이블뷰 컨트롤러냐 뷰 컨트롤러냐 차이죠???
근데 테이블뷰 컨트롤러를 이용하면 간단하게 할 수 있지만 화면을 커스텀하기가 힘들어요.
하지만 뷰 컨트롤러를 이용하면 화면을 마음대로 커스텀할수 있습니다.
두 방법 모두 가장 쉽게 예로 설명해볼게요.

## 테이블뷰 컨트롤러 이용한 방법
먼저 새로운 프로젝트를 만들어보자.
예제로 `TableViewEx`라는 프로젝트를 생성했습니다.
처음에 이런 화면을 만나게 되죠?
![1.png](https://MinominoDomino.github.io/assets/img/ios/viewController/1.png)

여기에서 먼저 데이터를 가진 모델은 아주 간단하게 만들겠습니다.
File -> New -> File에서 Swift를 선택하고 `StoreData.swift`파일을 생성합니다.
아래 처럼 케릭터 이름은 가진 배열을 멤버로 지닌 DataStore 클래스를 만들고 인스턴스를 만들어 둘게요.
나중에 테이블에 보여줄 데이터를 테이블 컨트롤러가 여기서 뽑아 갈거에요!!!!
```swift
import Foundation

let storeData: StoreData = StoreData()

class StoreData {
    var FriendsList: [String] = ["프로도", "콘", "무지", "라이언", "어피치", "네오", "제이지", "튜브"]
}
```

따라해봅시다.
![2.png](https://MinominoDomino.github.io/assets/img/ios/viewController/2.png)

![3.png](https://MinominoDomino.github.io/assets/img/ios/viewController/3.png)

다음으로 기본적으로 만들어진 `ViewController.swift`가 있는데 이를 과감이 삭제해버립니다!!!
delete키를 눌러서 삭제해버리기~

![4.png](https://MinominoDomino.github.io/assets/img/ios/viewController/4.png)

File -> New -> File에서 이번엔 `Cocoa Touch Class`를 선택하고 Next를 눌러봅니다.

![5.png](https://MinominoDomino.github.io/assets/img/ios/viewController/5.png)

Class에 TableViewControllerEx라고 입력하고 서브클래스를 `UITableViewController`로 바꿔주고 
클래스를 새로 생성합니다!!
![6.png](https://MinominoDomino.github.io/assets/img/ios/viewController/6.png)

짜잔!!! 코코아 프레임워크에서 제공해주는 `UITableViewController` 클래스를 만들었습니다.
![7.png](https://MinominoDomino.github.io/assets/img/ios/viewController/7.png)

오버라이드 되어 만들어진 `numberOfSections` 함수를 테이블의 `섹션 갯수`를 리턴해 달라고 하네요?
테이블의 섹션이 무엇일까요? 맨 처음에 보여드렸던 아이폰 설정에 보면 중간에 회색바가 들어가있는게 보일텐데요
그게 하나의 섹션을 나타냅니다. 이렇게 테이블 뷰에서 범위를 구분 지어줄때 세셕을 나눠주면 좋겠죠
우리는 테이블 하나에 열거할테니 일단은 상수로 1을 리턴해 주겠습니다.

![8.png](https://MinominoDomino.github.io/assets/img/ios/viewController/8.png)

바로 밑에 `tableView(_ tableView: UITableView, numberOfRowsInSection section: Int)` 함수가 있네요
이 함수는 `행의 갯수`를 리턴해 달라고하네요?
우리는 프렌즈 이름을 8개 넣어놨기때문에 8이라고 적어주면 되겠네요!! BUT!!!!!
이름이 추가되거나 삭제될때 마다 저기를 게속 바꿔줄순 없겠죠 그래서 프렌즈 이름이 들어있는 배열의 갯수를 행의 갯수라고 알려주면 자동으로 행의 갯수를 리턴해 줄 수 있을거 같네요.
데이터를 가진 스토어데이터에서 프렌즈 리스트에 갯수를 리턴해줍시다.

![9.png](https://MinominoDomino.github.io/assets/img/ios/viewController/9.png)

바로 밑에 주석으로 가려진 `tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)` 함수가 보입니다. 이 함수의 주석을 풀어줍시다.
이 함수는 `셀을 설정`하라고 하네요
위에서 우리는 섹션의 갯수와 들어갈 아이템의 수만 알려줬어요
테이블을 만들기위해서는 각 아이템 하나의 정보도 알려줘야겠죠? 각 행하나를 `테이블 셀`이라고 부릅니다.
이 함수에서 테이블 셀 하나의 정보를 설정해주면됩니다.
기본으로 cell이라는 상수가 만들어져있는데, tableView의 `dequeueReusableCell()`함수를 호출해서 받네요?
dequeueReusableCell()이 뭔지는 모르겠지만 재사용셀을 큐에서 빼나봐요.
이건 나중에 알아보고 여기서 `withIdentifier`의 값만 원하는 식별자로 바꿔줄게요.
[dequeueReusableCell() 포스팅 이동!](https://minominodomino.github.io/devlog/2019/05/06/ios-DequeueReusableCell/)
그리고 셀의 텍스트를 스토어데이터의 프렌즈리스트의 indexPath.row 번째로 세팅해줍니다.

indexPath.row는 뭘까요? `indexPath는 테이블의 섹션번호와 행번호`가 들어있어요
그중 .row는 행번호를 가지고 있습니다.

뭔소리인가!!??

아까 테이블뷰는 섹션을 가지고 있다고 했죠. 우리 예제에서는 1로 줬지만 2로 가정해볼게요.
그리고 행의 수까지 알려줬습니다. 3이라고 가정해볼게요.
그럼 indexPath는 [0, 0], [0, 1], [0, 2], [1, 0], [1, 1], [1, 2] 이런식으로 데이터를 가지고 있을거에요

그리고 행의 갯수만큼 `tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)` 함수가 불려집니다. 그때마다 indexPath의 값을 가져오게되죠, 그러니 우리는 indexPath.row의 값이 0, 1, 2, 3.... 순으로 불리게 될테니 프렌즈리스트의 인덱스로 사용할 수 있죠??

![10.png](https://MinominoDomino.github.io/assets/img/ios/viewController/10.png)

자... 이제 코딩은 이걸로 끝났고 몇가지 설정만 해주면됩니다.
Main.Storyboard로 이동해서 라이브러리 영역에서 `TableViewController`를 가져다가 놓습니다.

![11.png](https://MinominoDomino.github.io/assets/img/ios/viewController/11.png)

그리고 인스펙터 class를 우리가 만들었던 TableViewControllerEX로 바꿔줍시다!

![12.png](https://MinominoDomino.github.io/assets/img/ios/viewController/12.png)

속성 인스펙터에서 `Identifier`를 dequeueReusableCell()함수에서 `withIdentifier에 설정한 값`으로 넣어줍니다.s
여기서 식별자에 맞는 셀을 가져와 뷰에 할당하게 되요. 그러니 식별자 값이 다르면 안되겠죠?

![13.png](https://MinominoDomino.github.io/assets/img/ios/viewController/13.png)

이제 실행하면 짜잔!!!!
할 줄 알았는데 흰 화면이 반겨주죠??? 왜이럴까요?

처음 로드되는 화면이 기존에 만들어진 뷰컨트롤러에 붙어있어서 그 화면이 보여진 거에요
밑에 보이는 화살표가 엔트리 포인트라고 해서 앱의 가장 처음 화면을 설정해주는 화살표입니다.
이 화살표를 우리가 만든 컨트롤러로 직접 옮기거나 Is Initial View Controller에 체크를 해주면 됩니다.

![14.png](https://MinominoDomino.github.io/assets/img/ios/viewController/14.png)

이제 실행을 해보면! 잘나오네요 ㅎㅎㅎㅎ 이렇게 가장 기본적인 테이블 뷰를 만들어봤습니다.

<img src="https://MinominoDomino.github.io/assets/img/ios/viewController/15.PNG" width="50%">

## 뷰 컨트롤러에 테이블뷰 넣어서 하기
기본 뷰 컨트롤러에 테이블뷰를 넣어서 만들수 있어요.
이렇게 하면 뷰를 내 마음대로 배치 할 수 있어서 화면 커스텀 하는데 편하겠죠?
따라해봅시다.

위 예제에서 기본 ViewController를 과감히 삭제해 버렸기떄문에 새롭게 하나 만들어 볼게요
File -> New -> File에서 cocoa touch class를 선택하고 서브클래스는 UIViewController, 이름은 MyViewCOntroller로 만들어볼게요.
![17.png](https://MinominoDomino.github.io/assets/img/ios/viewController/17.png)
![18.png](https://MinominoDomino.github.io/assets/img/ios/viewController/18.png)
![19.png](https://MinominoDomino.github.io/assets/img/ios/viewController/19.png)


`TableView`와 `TableViewCell`를 라이브러리에서 찾아서 넣어둘게요.
그리고 TableViewCell의 `Identifier`를 FriendsCell이라고 넣겠습니다.
![20.png](https://MinominoDomino.github.io/assets/img/ios/viewController/20.png)

이제 MyViewController.swift로 이동합니다.
테이블뷰 컨트롤러에서 섹션의 수, 행의 수, 셀의 정보를 기본적으로 받는 함수가 오버라이드 되어있었는데 
여기에는 그런게 없네요??? 그럼 직접 오버라이드를 해주죠
`UITableViewDelegate`와 `UITableViewDataSource`를 채택해줍니다.
이 둘의 내용은 따로 포스팅할게요! 일단은 그대로 따라해봅시다.  
[델리게이트와 데이터소스 포스팅 이동!](https://minominodomino.github.io/devlog/2019/05/06/ios-tableViewDataSourceAndDelegate/)

![21.png](https://MinominoDomino.github.io/assets/img/ios/viewController/21.png)
그럼 경고와 함께 프로토콜을 추가 할 거냐고 물어보네요? fix버튼을 눌러봅시다.

![22.png](https://MinominoDomino.github.io/assets/img/ios/viewController/22.png)
어디서 많이 보던 함수 두개가 새로 생겼어요.
테이블뷰 컨트롤러에서 행의 갯수와 셀의 정보를 넣어주는 함수가 자동으로 오버라이드 되었네요?
그럼 전처럼 행의 갯수정보와 셀의 정보를 직접 코딩해 줍니다.
`Identifier` 값을 바꾸는거 잊지마세요!!!

![23.png](https://MinominoDomino.github.io/assets/img/ios/viewController/23.png)

자 그럼 코딩은 다 끝났어요
스토리보드 파일로 이동해서 새로운 뷰를 하나 넣어줄게요. 테이블 뷰 밑에 넣어서 여러 뷰로 화면을 커스텀 할 수 있다는 걸 보여줄게요. 그리고 백그라운드 색상도 바꿔봅시다.
![24.png](https://MinominoDomino.github.io/assets/img/ios/viewController/24.png)


그리고 뷰 컨트롤러의 클래스를 MyViewController로 바꿔줍니다.
![25.png](https://MinominoDomino.github.io/assets/img/ios/viewController/25.png)

와 그럼 우리는 코딩으로 행의 수와 셀의 정보를 알려줬고 컨트롤러의 클래스도 설정했고 셀의 식별자 까지 설정해줬으니 
테이블뷰 컨트롤러에서 했던거랑 똑같이 모든 작업을 다 해줬네요! 현기증 날 거 같으니 빨리 실행해보죠!
![26.png](https://MinominoDomino.github.io/assets/img/ios/viewController/26.png)


음..????
테이블에 왜 정보가 출력되지 않을까요???
아까 UITableViewDelegate와 UITableViewDataSource를 채택해준거 기억나죠?
델리게이트와 데이터소스를 지정해줘야하는데 빠져있어서 그래요.

그래서 우리는 테이블뷰의 아웃렛을 만들어서 테이블뷰의 델리게이트와 데이터소스를 뷰 컨트롤러로 지정해줄게요.

뷰에서 마우스 우클릭해서 드래그하거나 컨트롤를 누르고 드래그하여 테이블뷰의 아웃렛을 만들어줍니다.
![27.png](https://MinominoDomino.github.io/assets/img/ios/viewController/27.png)

뷰 컨트롤러의 ViewDidLoad()에서 테이블뷰 아웃렛의 델리게이트와 데이터소스를 self로 입력합니다.
![28.png](https://MinominoDomino.github.io/assets/img/ios/viewController/28.png)

그리고 실행을 해보면 우리가 원하던 화면이 나오네요!!!
![29.png](https://MinominoDomino.github.io/assets/img/ios/viewController/29.png)


















---
layout: post
title:  "[IOS] 테이블 뷰의 데이터소스와 델리게이트(DataSource and Delegate)"
subtitle:   "IOS 테이블 뷰 데이터소스와 델리게이트"
categories: devlog
tags: ios

---

테이블 뷰의 데이터소스와 델리게이트(DataSource and Delegate)를 알아보자.

## MVC 패턴 디자인
IOS는 기본적으로 `MVC패턴으로 디자인`되어있습니다.
데이터관리나 처리는 M(Model)에서 처리하고 사용자에게 보이는 화면은 V(View)에서 관리를 합니다.
그리고 모델과 뷰 사이에 C(Controller)가 다리 역활을 하면서 관리를 하고있죠?

스토리보드에 사용되는 모든 뷰들은 여기서 V에 해당합니다.
TableViewContollerEX라는 예제에서 ViewContoller에 UITableView를 넣어 예제를 만들어봤죠?
이 예제에서 MVC를 분리해볼게요.

리스트에 보여줄 정보를 배열로 .swift 파일로 따로 만들었습니다. -> 모델(M)에 해당
UITableView로 화면에 테이블을 만들었습니다. -> 뷰(V)에 해당
ViewContoller로 화면 관리를 하는 메서드를 만들었습니다. -> 컨트롤러(C)에 해당


## 테이블 뷰 데이터소스(TableView DataSource)
테이블 뷰는 V에 해당한다고 했습니다. 이 뷰에 넣어줄 데이터는 M이 가지고있고 뷰와 모델은 직접 통신이 불가능하죠?
그래서 컨트롤러인 C가 이 사이를 중계해주고있어요.

그래서 테이블뷰는 데이터와 변화를 감지하기 위해서 `DataSource`와 `Delegate`를 제공합니다.
컨트롤러가 이 둘의 프로토콜을 채택하게 되는거죠.

데이터소스는 뷰의 데이터에 관련한 메서드를 제공합니다.
그래서 무조건 오버라이드 해줘야하던 두개의 메서드가 있었죠?

`func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int)` 섹션별 행에 해당하는 데이터가 몇개냐!?

`func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)` 그 행의 셀은 어떤 데이터를 가졌냐!?

이렇게 뷰의 데이터에 관련된 정보를 관리하게됩니다.

데이터 소스에는 애플이 정성껏 만들어놓은 여러 메서드가 있습니다. 

너무 많아서 상황에 필요할때마다 직접 찾아서 보시면되요. 
애플 개발자 문서를 보셔도 되고 아래 그림처럼 xcode에서 채택한 프로토콜의 정의 부분으로 가서 직접 찾아서 사용해도됩니다.

UITableViewDataSource에서 커맨드+왼쪽클릭 후 Jump to Definition
![1.png](https://MinominoDomino.github.io/assets/img/ios/dataSourceAndDelegate/1.png)

화면 상단 클릭
![2.png](https://MinominoDomino.github.io/assets/img/ios/dataSourceAndDelegate/2.png)
![3.png](https://MinominoDomino.github.io/assets/img/ios/dataSourceAndDelegate/3.png)



## 테이블 뷰 델리게이트(TableView Delegate)

델리게이트는 그럼 왜 필요한가요?
델리게이트는 대리자죠? 뷰의 액션을 감지해주기 위해서 있습니다.

테이블뷰의 셀이 선택되면 어떤일을 해야하는지, 해당 셀이 편집모드가 되었을 때 혹은 나갔을 때 등
동작에 관련된 메서드를 제공합니다.

`func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath)` 행이 눌렸을 때 뭐할래?


그럼 확실히 구분이 되셨나요?
간단하게 데이터소스는 말 그대로 데이터의 관련된 것이고 델리게이트는 그 뷰의 액션에 관한거라고 생각하시면됩니다.

마지막으로 해당 뷰의 데이터소스와 델리게이트는 어떤 컨트롤러가 한다! 라고 알려줘야 한다고 했어요 
예제에서는 viewDidLoad()에서 코드로 알려줬지만, 스토리보드에서 처리해 줄 수도 있어요.
스토리보드에서 아웃렛 연결하듯 테이블뷰를 오른쪽 클릭해서 뷰 컨트롤러에 갖다놓으면 됩니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/dataSourceAndDelegate/4.png)
![5.png](https://MinominoDomino.github.io/assets/img/ios/dataSourceAndDelegate/5.png)



















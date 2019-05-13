---
layout: post
title:  "[iOS] Alert와 ActionSheet(UIAlertController)"
subtitle:   "iOS Alert와 ActionSheet"
categories: devlog
tags: ios

---

Alert와 ActionSheet(UIAlertController)를 알아보자.


## UIAlertController
앱을 사용하다보면 팝업형식으로 화면에 뭔가가 출력되는 걸 본적이 있을거에요.

왼쪽이 `ActionSheet`이고 오른쪽이 `Alert`라고합니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/UIAlertController/1.png)

IOS8으로 넘어가면서 그 이상부터는 둘을 따로 쓰지 않고 `AlertController`로 합쳐서 쓰는것 같아요.
![2.png](https://MinominoDomino.github.io/assets/img/ios/UIAlertController/2.png)

AlertController의 주요 프로퍼티와 메서드를 볼게요.

주요 프로퍼티
`title: String?` : Alert의 Tilte을 넣습니다.
`message: String?` : Alert의 추가 메시지를 넣습니다.
`preferredStyle: UIAlertController.Style`: AlertController가 Alert인지 ActionSheet인지 구분합니다.

주요 메서드
`init(title:message:preferredStyle:)` : Alert컨트롤러를 초기화합니다.
`addAction(UIAlertAction)` : Alert컨트롤러에 Action을 추가합니다.
`addTextField(configurationHandler : ((UITextField) -> Void)? = nil)` : Alert에서 텍스트 필드를 추가하여 글자를 입력받습니다.

## UIAlertAction
AlertController에 추가되는 액션을 지닌 객체입니다. 
단순하게 컨트롤러에 밑에 붙게되는 버튼이라고 생각하면 됩니다. 

주요 프로퍼티
`title: String?` : 해당 액션 버튼의 제목을 지정합니다.
`style: UIAlertAction.Style` : 해당 액션 버튼의 스타일을 지정합니다. (스타일 종류는 아래와 같습니다.)
 - default
 - destructive
 - cancel

주요 메서드
 `UIAlertAction(title: String?, style: UIAlertAction.Style, handler: ((UIAlertAction) -> Void)?)` : AlertAction객체를 초기화합니다.
 
## 따라하기
위에서 `AlertController`와 `AlertAction`을 봤는데요.
간단히 정리하면 alertController에 alertAction을 붙히고 viewController에서 해당 alertController을 modal로 띄우준다고 생각하면 됩니다. 아주 쉽죠?

그림처럼 화면을 구성할게요.
각 버튼은 Alert의 스타일을 구분하고 Action을 통해 각 스타일에 맞는 화면을 띄우겠습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/UIAlertController/3.png)

먼저 ActionSheet입니다.
UIAlertController를 만들어주고 타이틀, 메시지, 스타일을 지정해줍니다.
그리고 UIAlertAction을 만들고  타이틀, 스타일, 핸들러를 지정해줍니다.
핸들러는 해당 버튼을 눌렀을때 하게될 일을 지정해줍니다.
`addAction()`을 통해 alertAction을 alertController에 붙혀줍니다.

이렇게 하면 우리가 원하는 팝업창이 다 만들어집니다.
이 팝업창을 메인 화면에 띄어야 겠죠?
`present()`를 이용합니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/UIAlertController/4.png)

위 내용을 Alert에도 동일하게 하는데 컨트롤러의 스타일만 다르게 해주시면됩니다.

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func tuchUpActionSheetExcuteButton(_ sender: Any) {
        let actionSheet = UIAlertController(title: "공유하기", message: "어디로 공유할까요?", preferredStyle: .actionSheet)
        let actionSheetDefault = UIAlertAction(title: "가가오톡", style: .default, handler: { (action) in
            print("ActionSheet Default Pressed")
        })
        let actionSheetDestructive = UIAlertAction(title: "라잉", style: .destructive, handler: { (action) in
            print("ActionSheet Destructive Pressed")
        })
        let actionSheetCancel = UIAlertAction(title: "공유 취소", style: .cancel, handler: nil)
        
        actionSheet.addAction(actionSheetDefault)
        actionSheet.addAction(actionSheetDestructive)
        actionSheet.addAction(actionSheetCancel)
        
        self.present(actionSheet, animated: true, completion: nil)
    }
    
    @IBAction func TouchUpAlertExcuteButton(_ sender: Any) {
        let alert = UIAlertController(title: "상태경고", message: "네트워크가 불안정합니다.", preferredStyle: .alert)
        
        
        let actionCancel = UIAlertAction(title: "Cancel", style: .cancel, handler: { (ACTION) in
            print("Alert Cancel Pressed")
        })
        
        let actionDefault = UIAlertAction(title: "다시시도", style: .default, handler: { (action) in
            print("Alert Default Pressed")
        })
        let actionDestructive = UIAlertAction(title: "재시작", style: .destructive, handler: {(action) in
            print("Alert Destructive Pressed")
        })
        
        alert.addAction(actionDefault)
        alert.addAction(actionDestructive)
        alert.addAction(actionCancel)
        
        self.present(alert, animated: true, completion: {
            print("Present UI")
        })
    }
}
```

## 참조
[Apple Developer document - UIAlertController](https://developer.apple.com/documentation/uikit/uialertcontroller)
[Apple Developer document - UIAlertAction](https://developer.apple.com/documentation/uikit/uialertaction)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/ios-sample-store)


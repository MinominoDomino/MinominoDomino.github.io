---
layout: post
title:  "[iOS] 스플래시 화면(Splash Screen)"
subtitle:   "iOS Splash Screen"
categories: devlog
tags: ios

---

스플래시 화면(Splash Screen)를 알아보자.

오늘은 스플래시 화면을 만들어보겠습니다.
스플래시 화면은 앱이 처음 구동할때 잠깐 나오는 인트로 화면입니다.
런치 스크린이라고 부르기도 합니다.
iOS에서는 프로젝트에 런치 스크린이라고 써있어서 더 그렇게 부르는거 같아요 (제 생각...)

기본적으로 런치 스크린을 만드는건 매우 쉬워요.
프로젝트 내비게이터영역에 `LaunchScreen.storyboard` 라는 파일이 만들어져 있죠.
여기로 이동해서 ImageView를 올리고 사진을 넣어주면 끝이랍니다.
정말 쉽죠?
![1.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/1.png)

그리고 실행을 해보면 방금 설정한 화면이 잠깐 표시되고 첫번째 뷰로 이동할거에요.

그럼, 이렇게 간단한데 왜 글을 쓸까요??
보통 우리가 접하는 앱에서는 단순히 화면만 보여주고 끝나는게 아니라 여러 작업을 실제로 하고 있어요.
데이터를 미리 받아온다거나 화면 구성을 한다거나 등등 가끔씩 로딩화면에서 업데이트를 해달라거나 네트워크 상태를 확인해달라는 메시지 뜨는걸 보신적 있을거에요.

![2.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/2.png)
이런그림이죠?

이게 궁굼해서 직접 해봤습니다.
근데 궁굼증이 생긴게 런치 스크린은 이미지만 띄우는데 어디서하는걸까....?
그리고 저런 팝업은 뷰가 로드된 이후에 할 수 있을텐데 어떻게 메인뷰가 생기기전에 런치 스크린에서 작업하는걸까?
곰곰히 생각을 해봤습니다.

그렇게 개발자 가이드 문서도 확인하던 찰나에 애플 문서에서 힌트를 얻었죠!!
![3.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/3.png)
(포스팅 맨밑 참조에 URL올라가니 직접 확인해보세요!)

저런 문구와 함께 그림을 보니 스크린 런치 화면이랑 처음 화면은 거의 비슷하게 디자인 하라고하네요?
아??? 그럼 그냥 런치화면이랑 첫화면은 똑같은 화면으로 해버리면 모르지 않을까?
그럼 이미 뷰도 있는 상태이니 팝업도 띄울 수 있겠다 생각했어요
(지인의 찬스도 있었습니다 ㅎㅎ)
밑에 그림으로 정리해볼게요.
![4.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/4.png)

런치 스크린과 똑같이 스토리보드1를 만들고 거기서 원하는 작업을 하고 맞다 싶으면 스토리보드2로 보내는거죠
결국 스토리보드2의 내용이 메인이라고 생각하면되요!
이해되시나요? 유레카 더라고요... 전혀 저도 몰랐던건데..... 

그럼 문제가 다 해결되죠? 
직접한번 해보겠습니다.
예제는 런치화면에서 네트워크 상태를 확인하고 네트워크 사용이 가능 할때만 스토리보드2(이제 메인이라고 부를게요)
화면으로 넘겨주는 예제를 해볼게요.

## 예제 따라하기
먼저 런치 스크린을 설정해 주겠습니다.
런치 스토리보드 뷰에 ImageView를 올리고 원하는 이미지로 설정을 할게요.
이건 위에서 했었죠?

그리고 처음만들어져 있던 뷰컨트롤러와 메인스토리보드를 헷갈리지 않게 폴더를 만들어서 넣어둘게요.
(파일 이름을 바꿔서 하셔도됩니다.)
그리고 이 뷰를 런치 스크린처럼 보이게 속일테니 이미지뷰를 넣고 똑같이 작업해줍니다.
![5.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/5.png)

새로운 뷰컨트롤러와 스토리보드를 만들어줍니다.
여기서는 FirstViewController와 FirstStoryboard라고 할게요.
이게 우리가 메인으로 쓸 스토리보드2 입니다.
뷰에 레이블을 하나 넣어서 메인 화면임을 보이게 할게요.
그리고 뷰컨트롤러를 FirstViewController로 지정해줍니다.
스토리보드ID도 넣어줍니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/6.png)

또 새로 스위프트 파일을 만들게요.
DeviceConfig.swift를 만들고 여기서는 디바이스의 네트워크 상태를 지니는 클래스를 하나 만들고
전역에서 쓸 수 있도록 싱글톤으로 객체를 만들게요.
checkDeviceNetworkStatus 메서드 내용은 추후에 네트워크 쪽 하면서 다시 포스팅 하겠습니다.
일단 복붙해주세요. 저도 구글링으로 가져온 내용입니다.
그럼 싱클톤 객체에서 프로퍼티로 네트워크 상태를 true, false로 가져올 수 있네요.

```swift
private func checkDeviceNetworkStatus() -> Bool {
        print("Check to Device Natwork Status....")
        var zeroAddress = sockaddr_in(sin_len: 0, sin_family: 0, sin_port: 0, sin_addr: in_addr(s_addr: 0), sin_zero: (0, 0, 0, 0, 0, 0, 0, 0))
        zeroAddress.sin_len = UInt8(MemoryLayout.size(ofValue: zeroAddress))
        zeroAddress.sin_family = sa_family_t(AF_INET)
        
        let defaultRouteReachability = withUnsafePointer(to: &zeroAddress) {
            $0.withMemoryRebound(to: sockaddr.self, capacity: 1) {zeroSockAddress in
                SCNetworkReachabilityCreateWithAddress(nil, zeroSockAddress)
            }
        }
        
        var flags: SCNetworkReachabilityFlags = SCNetworkReachabilityFlags(rawValue: 0)
        if SCNetworkReachabilityGetFlags(defaultRouteReachability!, &flags) == false {
            return false
        }
        
        // Working for Cellular and WIFI
        let isReachable = (flags.rawValue & UInt32(kSCNetworkFlagsReachable)) != 0
        let needsConnection = (flags.rawValue & UInt32(kSCNetworkFlagsConnectionRequired)) != 0
        let ret = (isReachable && !needsConnection)
        return ret
    }
```
![7.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/7.png)

스토리보드1의 ViewController파일로 이동합니다.
런치스크린이 끝나면 여기 뷰컨트롤의 `viewDidLoad`를 호출하겠죠?
근데 여기에서 UI작업을 하면안되죠?
그러니 view가 전부다 나온다음에 하도록 `viewDidAppear`를 오버라이드 합니다.
뷰가 다 나타나면 여기에서 호출할 메서드를 하나 만들게요.
![8.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/8.png)

checkDeviceNetworkStatus() 메서드입니다.
이 안에서 if else로 네트워크 상태 싱크톤 객체에서 가져와서 분기해줍시다.
if안쪽은 네트워크가 가능할때, else쪽은 불가능할때입니다.

먼저 else를 볼게요.
네트워크가 안되면 팝업창을 띄어줘야죠? 팝업은 `UIAlertController`를 사용합니다.
타이틀과 메시지와 스타일을 지정해줍니다. 
그럼 단순한 메시지창만 출력됩니다.
밑에 버튼을 붙여주는 객체는 `UIAlertAction`입니다.
액션에도 타이틀과 스타일을 지정해주는데 뒤에 핸들러가 있습니다. 핸들러에서는 이 버튼을 눌렀을때 할일을 알려주면되요. 없으면 그냥 nil를 주시면됩니다. 저는 다시시도하는 버튼이니까 네트워크 상태를 다시 확인해보겠습니다.
그리고 만든 Alert객체에 Action을 `addAction()`해줍니다.
만들어진 팝업창을 이제 화면에 뿌려줘야죠?
`present()`함수로 화면에 뿌려줍시다.
![9.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/9.png)

다음은 if안쪽입니다.
성공하면 우리가 메인으로 쓸 스토리보드2로 이동시켜주면됩니다.
어떻게 할까요?
`UIStoryboard()`로 우리가 쓸 스토리보드를 찾아줍니다.
예제에서는 First라고 스토리보드ID를 넣었습니다.
그리고 그 보드에서 첫 화면의 뷰컨트롤러를 `instantiateViewController()`로 찾아서 UIViewContoller객체를 만들어줍니다.
그리고 `present()`로 화면에 뿌려주면되요.
![10.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/10.png)

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
    
    override func viewDidAppear(_ animated: Bool) {
       checkDeviceNetworkStatus()
    }
    
    func checkDeviceNetworkStatus() {
        if(DeviceManager.shared.networkStatus) {
            let firstVC = UIStoryboard(name: "First", bundle: nil).instantiateViewController(withIdentifier: "FirstViewController")
            present(firstVC, animated: true, completion: nil)
            
        } else {
            let alert: UIAlertController = UIAlertController(title: "네트워크 상태 확인", message: "네트워크가 불안정 합니다.", preferredStyle: .alert)
            let action: UIAlertAction = UIAlertAction(title: "다시 시도", style: .default, handler: { (ACTION) in
                self.checkDeviceNetworkStatus()
            })
            alert.addAction(action)
            present(alert, animated: true, completion: nil)
        }
    }
}
```

자 이제 실행해보겠습니다.
시뮬레이터는 맥의 와이파이를 써요.
네트워크가 끊긴환경을 먼저 볼테니 와이파이를 끊고 실행해봅니다.
![11.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/11.png)

스플래시 화면이 먼저 나오죠?
그리고 네트워크 상태를 확인하다가 끊겨있음을 확인하고 팝업을 띄어줍니다.
![12.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/12.png)

다시 시도를 게속눌러봅시다. 그래도 안넘어가고 게속 있네요!
이 상태로 와이파이를 다시 연결하고 다시 시도를 게속 눌러봅시다.
연결이 되어서 상태가 확인되는 순간 우리가 쓸 메인 화면으로 이동하는 걸 볼 수 있습니다.
![13.png](https://MinominoDomino.github.io/assets/img/ios/SplashScreenEx/13.png)



## 참조
[Apple Developer document - UIStoryboard](https://developer.apple.com/documentation/uikit/uistoryboard)

[Apple Developer document - UIAlertcontroller](https://developer.apple.com/documentation/uikit/uialertcontroller)

[Apple Developer document - UIAlertaction](https://developer.apple.com/documentation/uikit/uialertaction)

###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/ios-sample-store)

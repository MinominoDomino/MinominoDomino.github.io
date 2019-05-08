---
layout: post
title:  "[IOS] 오퍼레이션큐의 addOperation()(Operation Queue)"
subtitle:   "SWIFT Operation Queue"
categories: devlog
tags: ios

---

오퍼레이션큐의 addOperation()(Operation Queue)를 알아보자.

오퍼레이션은 실행하는 작업과 관련된 코드와 데이터를 객체지향적으로 나타내는 추상 클래스로 오브젝트씨를 기반으로 설계되었고 오퍼레이션 오브젝트는 코코아 객체로 코코아 프레임워크 기반앱에서 일반적으로 사용됩니다.
주로, `실행하는 작업을 비동적으로 수행`하며 오퍼레이션 큐에 작업을 추가합니다.
오퍼레이션 큐는 오퍼레이션의 실행과 작업 스케쥴링을 관리합니다.
오퍼레이션의 주 기능은 다음과 같습니다.
 - 작업의 의존성 설정
 - 작업 취소
 - 완료 블록
 - KVC 호환, KVO를 통해 상태 변화 감시
 - 우선 순위 설정 

내부적으로 `GCD`를 사용합니다.
오퍼레이션은 작업이 종료되기 전까지 큐에 남아 있고 추가된 작업은 직접 제거할 수 없습니다.
오퍼레이션이 취소를 하거나 오퍼레이션 큐의 모든 작업을 취소하는 방법이 있습니다.

## OperationQueue의 주요 프로퍼티와 메서드
### 오퍼레이션 큐
```swift
var name: String? { get set }
class var current: OperationQueue? { get }
class var main: OperationQueue { get }
```
| 이름 | 설명 |
|--------|--------|
|name|오퍼레이션 큐의 이름|
|current|현재 작업을 시작한 오퍼레이션 큐를 반환|
|main|메인 스레드와 연결된 오퍼레이션 큐를 반환|

### 오퍼레이션큐의 오퍼레이션 관리
```swift
func addOperation(_ op: Operation)
func addOperations(_ ops: [Operation], waitUntilFinished wait: Bool)
func addOperation(_ block: @escaping () -> Void)
func cancelAllOperations()
func waitUntilAllOperationsAreFinished()
```
| 이름 | 설명 |
|--------|--------|
|addOperation|오퍼레이션 오브젝트를 큐에 추가|
|addOperations|오퍼레이션 오브젝트 배열을 큐에 추가|
|addOperation|전달한 클로저를 오퍼레이션 오브젝트에 감싸서 큐에 추가|
|cancelAllOperations|모든 오퍼레이션을 취소|
|waitUntilAllOperationsAreFinished|모든 오퍼레이션이 완료될때 까지 스레드로 접근 차단|

### 오퍼레이션 관리
```swift
var maxConcurrentOperationCount: Int { get set }
var qualityOfService: QualityOfService { get set }
var isSuspended: Bool { get set }
```
| 이름 | 설명 |
|--------|--------|
|maxConcurrentOperationCount|동시에 실행가능한 오퍼레이션의 최대 수|
|qualityOfService|우선순위 옵션|
|isSuspended|오퍼레이션의 수행여부를 나타냄, true면 대기중인 오퍼레이션은 실행하지 않음 실행된 오퍼레이션은 실행, false는 대기중인 오퍼레이션을 실행|



## OperationQueue.addOperation과 OperationQueue.main
오퍼레이션은 비동기적으로 작업을 수행할때 사용됩니다.
오퍼레이션큐에 작업을 추가해주면되는데 큐에 들어간 작업은 메인쓰레드가 아닌 다른 쓰레드에서 작업을 합니다.
오퍼레이션 사용 방법 중 가장 간단한 방법을 보겠습니다.
예제는 용량이 큰 이미지를 다운받아 이미지뷰에 표시하는 간단한 기능을 수행합니다.
하지만 다운로드 받는 동안 메인쓰레드가 다른일을 할 수 없어 오퍼레이션큐를 이용하여 다운로드 작업을 메인쓰레드가 아닌곳에서 실행하겠습니다.

먼저, 새로운 프로젝트를 만들고 다음 그림처럼 뷰를 꾸며주세요.
![1.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/1.png)

뷰 컨트롤러로 이동해서 코딩을 해줍시다.
```swift
@IBAction func downloadBtnTouchUp(_ sender: UIButton) {
	imageView.image = nil
	if let imageUrl: URL = URL(string: "https://upload.wikimedia.org/wikipedia/commons/3/3d/LARGE_elevation.jpg") {
		do {
			let imageData: Data = try Data(contentsOf: imageUrl)
			if let image: UIImage = UIImage(data: imageData) {
				self.imageView.image = image
			}
		}  catch {
			print(error.localizedDescription)
			}
	}
}

@IBAction func addPlusBtnTouchUp(_ sender: Any) {
	var count: Int = 0
	if let text = countLabel.text{
		if let value = Int(text) {
			count = value+1
		} else { return }
	} else { return }
	countLabel.text = String(count)
}
```
![2.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/2.png)

다운로드 버튼 액션에서 용량이 큰 이미지를 찾아서 `URL객체`로 만들고 `Data객체`로 받아 `UIImage`를 만듭니다.
그리고 이미지뷰에 이미지로 넣는 기능을 구현합니다.
에드플러스버튼은 단순히 레이블의 값을 받아 +1하여 레이블에 표시해주는 기능을 구현합니다.

이제 실행을 해보겠습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/3.png)

다운로드 버튼을 누르고 +1버튼을 게속 눌러보겠습니다.
버튼을 아무리 눌러봐도 레이블의 값이 변하질 않습니다.
게속게속 눌러보겠습니다. 똑같군요.......
이때, 이미지 다운로드가 완료되어 레이블의 값이 변하는군요???
![4.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/4.png)

왜 이럴까요??
URL로부터 데이터를 가져오는 `Data메서드는 동기 메서드`입니다.
따라서, 데이터를 가져오는 작업이 완료되기 전까지는 `메인쓰레드를 점유`하게 되고 다른 동작들은 대기를 하게됩니다.
그래서 화면이 멈춘것 처럼보이고 다운로드가 완료되는 시점에 대기하던 동작들이 수행하게 되기에 이같은 현상을 보이게 된거죠.

그럼 `오퍼레이션 큐를 이용`하여 데이터를 가져오는 메서드를 메인 쓰레드가 아닌 곳에서 실행을 시켜볼게요.
downloadBtnTouchUp()액션을 아래처럼 바꿔보겠습니다.
```swift
@IBAction func downloadBtnTouchUp(_ sender: UIButton) {
	imageView.image = nil
    if let imageUrl: URL = URL(string: "https://upload.wikimedia.org/wikipedia/commons/3/3d/LARGE_elevation.jpg") {
    	OperationQueue().addOperation {
        	do {
            	let imageData: Data = try Data(contentsOf: imageUrl)
                if let image: UIImage = UIImage(data: imageData) {
                	OperationQueue.main.addOperation {
                    	self.imageView.image = image
                    }
                } else { return }
            } catch { print(error.localizedDescription) }
        }
    } else { return }
}
```
![5.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/5.png)

중간에 보시면 `OperationQueue().addOperation { }` 으로 데이터를 가져오는 메서드와 이미지뷰에 추가하는 작업을 감싸줬습니다.
이렇게 감싸진 곳의 작업은 메인쓰레드가 아닌 곳에서 실행이 됩니다.

어?? 근데 중간에 `OperationQueue.main.addOperation { }`는 뭔가요??
OperationQueue().addOperation는 메인쓰레드가 아닌 곳에서 작업을 수행한다고 했죠? 
하지만, `UI오브젝트를 제어하는 작업은 무조건 메인 UI쓰레드에서 작업이 이루워져야합니다.`
그래서 중간에 데이터 다운로드는 다른 쓰레드에서 작업하다가 이미지뷰에 이미지를 붙히는 작업은 main쓰레드에서 하도록 설정을 해준거죠.
이렇게 OperationQueue.main.addOperation는 메인쓰레드 큐에 오퍼레이션을 넣어주게 됩니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/8.png)

한번 실행을 해볼까요?
![6.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/6.png)

다운로드 버튼을 누르고 +1버튼을 눌러보니 레이블이 잘 갱신이 되네요.

![7.png](https://MinominoDomino.github.io/assets/img/ios/oprationqueue/7.png)

## 참조
[Apple Developer document - Operation](https://developer.apple.com/documentation/foundation/operation)
[Apple Developer document - OperationQueue](https://developer.apple.com/documentation/foundation/operationqueue)
[Apple Developer Guideline - OperationQueue](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html)

###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/ios-sample-store)





---
layout: post
title:  "[SWIFT] 클로져(Closure)"
subtitle:   "SWIFT 클로져"
categories: devlog
tags: swift

---

클로져(Closure)를 알아보자.

클로져는 스위프트에서 한번만 사용하는 일회용 함수입니다.
코드의 명확성과 의도를 잃지 않으면서 문법을 축약하여 사용할 수 있는 방법을 제공합니다.
자바에서 람다함수라고 불리는것이 스위프트에서는 클러져입니다.
기본적으로 함수와 비슷하게 생겼지만 축약표현이기에 축약되는 키워드와 값이 있습니다.
클로저는 다음과 같은 기본식으로 구성되어있습니다.

- `{ (매개변수) -> 반환형 in 실행구문 }`

[Apple swift document - Closure](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)

## 클로져 표현식
가장 기본적인 표현으로 변수에 클로져를 할당하여 쓸 수 있습니다.
```swift
let someClosure  = { () in
    print("someClosure Excute")
}
let someClosureInputString = { (str: String) in
    print("\(str) SomeClosure Excute")
}
let someClosureInputOutString = { (str: String) -> String in
    return "\(str) someClosure Excute"
}
someClosure()	//someClosure Excute
someClosureInputString("test")	//test SomeClosure Excute
print(someClosureInputOutString("main"))	//main someClosure Excute
```
두 번쨰로 변수를 없애고 바로 실행을 시킬수 있습니다.
클로져를 `( )`로 감싸고 함수호출을 하면됩니다.

```swift
({ () in
    print("Direct Excute")	//Direct Excute
})()
```
클로져는 괄호유/무에 따라 컴파일러가 오류를 발생시킬 수 있으니 주의해야합니다.
또한 클로져에 들어가는 `in` 키워드에서 보통 줄을 바꾸고 실행구문을 작성합니다.

## 클로져 실사용
클로저는 함수의 매개변수로 값을 넘길때 많이 사용됩니다.
보통 클로져를 매개변수로 넘겨 함수의 동작이 완료된 후 원하는 동작을 실행시키는 등 콜백처럼 사용이 많이되요.

기본적인 예를 하나 볼게요.
먼저 가독성을 위해서 plusValue라는 클로져가 보이시나요? 간단하게 값 두개를 받아서 더한 다음 리턴해주는 클로져입니다. 그리고 calculator라는 함수의 마지막 매개변수가 클로져 형태로 받네요.
이 함수를 쓸때 그럼 같은 형태로 생긴 클로져가 있으니 그대로 매개변수로 줘볼게요.
`calculator(data1: 10, data2: 20, process: plusValue)`
이렇게 하면 plusValue라는 클로져가 calculator에서는 process가 되면서 process(10, 20)이 plusValue(10, 20)과 동일해지죠. 이번엔 평소 많이 쓰는대로 상수로 안하고 직접 매개변수에 해볼게요.
전부다 기대한 값이 나오고 있네요.
저도 첨에 알겠다고 이해했지만... 게속 게속 봤습니다 정말....
```swift
func calculator(data1: Int, data2: Int, process: (Int, Int) -> Int) -> Int {
    return process(data1, data2)
}

let plusValue = { (num1: Int, num2: Int) -> Int in
    return num1+num2
}

print("\(plusValue(10,20))")    //30
print("\(calculator(data1: 10, data2: 20, process: plusValue))")    //30

let ret = calculator(data1: 10, data2: 20, process: { (num1, num2) in
    return num1+num2
})
print("\(ret)") //30
```

그럼 cocoa에서 진짜 이게 쓰이는 곳이 있어요.
제 포스팅 중 AlertController라는 포스팅이 있는데, 여기에서 handler개념으로 쓰이더라고요
코드를 한번 볼게요.

```swift
 let alert: UIAlertController = UIAlertController(title: "네트워크 상태 확인", message: "네트워크가 불안정 합니다.", preferredStyle: .alert)
            let action: UIAlertAction = UIAlertAction(title: "다시 시도", style: .default, handler: { (action) in
                self.checkDeviceNetworkStatus()
            })
            alert.addAction(action)
            present(alert, animated: true, completion: nil)
```
이 코드에서 UIAlertAction의 init()에 handler라는 매개변수가 보이시나요? 형식이 딱 클로져죠? 
그럼 저 핸들러의 원형도 볼게요.
![1.png](https://MinominoDomino.github.io/assets/img/swift-closure/1.png)

네, 위 예제랑 타입만 다르지 똑같네요.
이렇게 클로저는 함수의 매개변수로 넘기는걸 자주 볼 수 있습니다.
이제 함수 원형에서 `(() ->())` 이렇게 생긴 타입이 뭘 의미하고 어떻게 매개변수를 넘겨줘야하는지 알것같네요.


##예제소스
###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동!](https://github.com/MinominoDomino/swift-grammar-house)







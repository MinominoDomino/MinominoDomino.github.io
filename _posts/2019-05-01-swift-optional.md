---
layout: post
title:  "[SWIFT] 옵셔널(Optional)"
subtitle:   "SWIFT 옵셔널"
categories: devlog
tags: swift

---

옵셔널(Optional)를 알아보자.

옵셔널은 스위프트에 있는 특별한 문법입니다. 변수의 값이 `nil`일 수 있다고 알리는 키워드입니다.
옵셔널은 변수타입에 `?` 키워드를 사용합니다.
스위프트에서 변수는 일반 변수와 옵셔널 변수가 있습니다.
두 변수의 차이점은 옵셔널이냐 아니냐 차이이며, 기본적으로 스위프트의 변수는 옵셔널이 아닌 값이여야 합니다.
예제를 보면서 기본적인 옵셔널의 특징을 알아볼게요.

[Apple swift document - Optional](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)

## 옵셔널 변수 특징

먼저 일반 변수를 만들어 `nil`이라는 값을 넣게되면 오류를 내는 것을 볼 수 있습니다.
하지만 변수 타입에 `?`를 붙혀 옵셔널 변수로 만들면 nil이라는 값이 들어가네요.
두 타입을 비교해 보면 옵셔널 변수에 `[타입]?`로 표현되어있는걸 볼 수 있습니다.
 
```swift
var nomal: Int = nil  //error

var nomal: Int? = nil

var nonOptionalRet: Int = 10

var optionalRet: Int? = 10
```

일반 변수와 옵셔널 변수는 타입이 다르기 때문에 상호 연산이나 값 대입을 할 수 없어요
```swift
var ret: Int = 0
ret = nonOptionalRet
ret = optionalRet //error
ret = nonOptionalRet + optionalRet    //error
```

## 언래핑

그럼 옵셔널 자료형과 아닌 것 끼리는 어떻게 연산을 하거나 대입을 할 수 있나요?
이 경우에는 `!` 키워드를 사용하여 강제적으로 옵셔널을 벗기고 값을 가져와 연산을 할 수 있습니다.
```swift
var result: Int = 0
result = optionalRet!
print(result)   //10
result = nonOptionalRet + optionalRet!
print(result)   //20
```

## 옵셔널 바인딩

하지만 강제로 언래핑을 하는 경우, 옵셔널 변수에 값이 실제 nil이면 연산을 할 수 없기때문에 에러가 발생하게됩니다.
따라서 강제로 언래핑을 하면 코드의 안정성이 떨어지게되죠. 그래서!!!!
옵셔널 바인딩을 통하여 언래핑이 가능한지 불가능한지 확인하여 동작을 수행하게 되는데 이것이 옵셔널 바인딩입니다.
옵셔널 바인딩은 `if let`을 사용합니다.

```swift
var someValueA: Int = 5
var someValueB: Int? = nil
var someValueRet: Int = 0

someValueRet = someValueA + someValueB!   //error
if let someValue = someValueB {
    someValueRet = someValueA + someValue
}
print(someValueRet)
```


## 옵셔널은 왜쓰나?
개발자가 설계하지 않은 환경에서의 오류에 대한 대비를 할 수 있어 안전함을 제공 해 줄 수 있습니다.



###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)







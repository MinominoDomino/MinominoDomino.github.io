---
layout: post
title:  "[SWIFT] 딕셔너리(Dictionary)"
subtitle:   "SWIFT 딕셔너리"
categories: devlog
tags: swift

---

딕셔너리를 알아보자.

딕셔너리는 요소의 순서에 상관 없이 `<key:value>`로 구성되는 컬섹션 입니다.
키워드는 `Dictionary`를 사용합니다.
key는 value와 1:1로 쌍을 이루고 key는 딕셔너리에서 유일한 값이여야 해요!
배열에서는 존재 하지않는 인덱스를 접근하면 에러가 나오지만 딕셔너리는 오류 없이 `nil`를 반환합니다.

원형은 다음과 같이 생겼지요?
`var [배열명]: Dictionary<T, T> = Array<T,T>()`

## 선언 방법

딕셔너리 선언에는 다음과 같은 여러 방법이 있어요.
본인이 편한대로 쓰면 되지만, 축약형 표현이 많이 쓰는 것 같아요
그대로 따라하면서 하나씩 직접 해보죠!

- 원형 표현 `Dictionary<T, T> = Dictionary<T, T>()`
- 축약 표현 `[T: T] = [:]`

```swift
var responseMessage: Dictionary<Int, String> = Dictionary<Int, String>()		// 빈 딕셔너리
var responseMessage: Dictionary<Int, String> = [200: "OK"]		 // [200: "OK"]

var responseMessage: [Int: String] = [:]					      // 빈 딕셔너리
var responseMessage: [Int: String] = [200: "OK"]			  //[200, "OK"]
```

참고로 저는 `타입추론`를 쓰지 않겠습니다!

## 배열 메서드

딕셔너리에서 제공하는 기능이 여러개 있지만 상황에 따라 찾아보면서 쓰시면됩니다.
우리 모두 ++++API에서 찾아보는 습관++++을 기르는게 좋아요!
그래서 애플에서 제공하는 공식 문서를 링크 했어요 모든 내용은 여기서 보시면 될 것 같네요.
[Apple swift document - Dictionary](https://developer.apple.com/documentation/swift/dictionary)

여기서는 기본적인 몇개만 추려서 해보겠습니다.
- `isEmpty`는 딕셔너리가 비어 있는지 확인합니다.
```swift
print(responseMessage.isEmpty)		//false
```

- `count`는 딕셔너리에 저장된 갯수를 알려줍니다.
```swift
print(responseMessage.count)		//1
```

- `변수명[key] = "value"`는 딕셔너리에 새로운 값을 추가합니다.
```swift
responseMessage[403] = "Access forbidden"
responseMessage[404] = "File not found"
responseMessage[500] = "Internal server error"
print(responseMessage)	//[500: "Internal server error", 200: "ok", 403: "Access forbidden", 404: "File not found"]
```

- `변수명[key]`는 딕셔너리에서 해당 key의 value를 보여줍니다.
```swift
print(responseMessage[200])	//OK
print(responseMessage[444])	//nil (444는 딕셔너리에 존재하지않는 key죠?)
```

- `removeValue`는 딕셔너리에서 해당 key의 **값을 삭제** 합니다.
```swift
responseMessage.removeValue(forKey: 403)
print(responseMessage)	//[500: "Internal server error", 200: "ok", 404: "File not found"]
```


## 별칭 사용하기

모든 딕셔너리 타입의 변수에 타입을 게속 적어주기엔 너무 길죠? 소드 코드도 게속 길어지게 됩니다.
`typealias` 키워드를 사용해서 딕셔너리를 타입처럼 사용할게요.
다음과 같이 별칭을 사용하면 딕셔너리는 한번만 선언하고 별칭으로 게속 쓸 수 있고 한번 수정으로 모든 변수에 적용이 가능 하겠지요!
```swift
typealias responseMessageInfo = [Int: String]

var responseMessage: responseMessageInfo = [:]
responseMessage[200] = "OK"
print(responseMessage)     //[200:"OK"] 
```

###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)




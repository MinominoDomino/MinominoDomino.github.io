---
layout: post
title:  "[SWIFT] 배열(Array)"
subtitle:   "SWIFT 배열"
categories: devlog
tags: swift

---

배열을 알아보자.

배열은 같은 타입의 데이터들이 묶여있는 컬섹션 타입입니다.
키워드는 `Array`를 사용하고 가장 일반적인 데이터 유형입니다.
스위프트에서 배열은 고정된 크기가 아닌 ArrayList같은 데이터를 동적으로 추가/삭제 할 수 있어요.
그래서 자유롭게 늘어나고 줄어들고 할 수 있어요!


원형은 다음과 같이 생겼지요?
`var [배열명]: Array<T> = Array<T>()`

## 여러가지 선언 방법

배열 선언에는 다음과 같은 여러 방법이 있어요.
본인이 편한대로 쓰면 되지만, 축약형 표현이 많이 쓰는 것 같아요
그대로 따라하면서 하나씩 직접 해보죠!

- 원형 표현 	Array<T> = Array<T>()
- 축약 표현[T] = []

```swift
var friendsName: Array<String> = Array<String>()		// 빈 배열
var friendsName: Array<String> = ["KIM", "LEE"]		 // ["KIM", "LEE"]

var friendsName: [String] = []					      // 빈 배열
var friendsName: [String] = ["KIM", "LEE"]			  //["KIM", "LEE"]
```

참고로 저는 `타입추론`를 쓰지 않겠습니다!

## 배열 메서드

배열에서 제공하는 기능이 여러개 있지만 상황에 따라 찾아보면서 쓰시면됩니다.
우리 모두 **API에서 찾아보는 습관**을 기르는게 좋아요!
그래서 애플에서 제공하는 공식 문서를 링크 했어요 모든 내용은 여기서 보시면 될 것 같네요.
[Apple swift document - Array 클릭!](https://developer.apple.com/documentation/swift/array)

여기서는 기본적인 몇개만 추려서 해보겠습니다.
- `isEmpty`는 배열이 비어 있는지 확인합니다.
```swift
print(friendsName.isEmpty)		//false
```
- `count`는 배열의 갯수를 알려줍니다.
```swift
print(friendsName.count)		//2
```
- `first`는 배열의 가장 첫 번째 값에 접근합니다.
```swift
print(friendsName.first)		//KIM
```
- `last`는 배열의 가장 마지막 값에 접근합니다.
```swift
print(friendsName.last)		//LEE
```
- `index(of:)`는 해당 값의 배열 인덱스를 알려줍니다.
```swift
print(friendsName.index(of: "LEE"))		//1
```
- `append(_:)`는 배열의 마지막에 값을 추가합니다.
```swift
friendsName.append("PARK")
print(friendsName)      //["KIM", "LEE", "PARK"]
```
- `insert(_:at:)`는 특정 인덱스에 값을 추가합니다.
```swift
friendsName.insert("YOUN", at: 2)
print(friendsName)     //["KIM", "LEE", "YOUN", "PARK"] 
```
- `remove(_:)`는 특정 인덱스의 값을 삭제합니다.
```swift
friendsName.remove(ar: 1)
print(friendsName)     //["KIM", "YOUN", "PARK"]  
```
- `removeFirst()`는 배열의 가장 첫 번째 값을 삭제합니다.
```swift
friendsName.removeFirst()
print(friendsName)     //[YOUN", "PARK"] 
```
- `removeLast`는 배열의 가장 마지막 값을 삭제합니다.
```swift
friendsName.removeLast()
print(friendsName)     //[YOUN"] 
```

###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)



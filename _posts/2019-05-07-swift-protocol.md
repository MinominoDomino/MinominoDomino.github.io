---
layout: post
title:  "[SWIFT] 프로토콜(Protocol)"
subtitle:   "SWIFT 프로토콜"
categories: devlog
tags: swift

---

프로토콜(Protocol)를 알아보자.

프로토콜은 특정 기능에 있어서 수행에 필요한 요구 사항(프로퍼티, 메서드)등을 명시한 청사진을 정의하는 것 입니다.
구조체, 클래스는 프로토콜을 채택하면 그 프로토콜의 요구사항을 정의해야합니다.
특정 프로토콜를 따르는 것을 채택한다 라고하며 이러한 프로토콜을 채택한 타입은 프로토콜을 따른다라고 합니다.
 ex) A프로토콜을 채택하여 A프로토콜을 따르는 B타입

키워드로 `protocol`를 사용합니다.
프로토콜의 이름의 시작은 대문자로 시작합니다.
- `protocol [프로토콜이름] { }`


## 프로토콜 선언과 채택
기본적으로 프로토콜 하나를 생성하고 이를 채택하는 예제입니다.
만약 특정 프로토콜을 채택하는 클래스가 상속을 받고있다면, 상속받을 클래스를 먼저 써줍니다.
프로토콜을 여러개 채택하는 경우 `,`로 구분하여 나열합니다.
 
```swift
protocol SomeProtocol {
    
}

class MyClass: SomeProtocol {   //SomeProtocol을 채택한다.
    
}

// 서브 클래스에서 프로토콜 채택하기
class RootClass {

}

class SubClass: RootClass, SomeProtocol {   //슈퍼클래스 상속 이후 SomeProtocol을 채택한다.
  
}
```


## 프로토콜 프로퍼티 요구사항

해당 프로토콜을 채택하는 타입이 구현해야하는 프로퍼티를 요구합니다.
프로토콜에서 요구하는 프로퍼티는 항상 `var` 변수이며, `gettable`과 `settable`을 명시해 줍니다.
타입 프로퍼티를 요구할땐 `static` 키워드를 사용합니다.
 - `var [프로퍼티명] { get set }`
 
프로퍼티를 채택하여 구현할때에는 프로퍼티의 요구사항이 읽기/쓰기라면 게터와 세터 둘다 구현을 해줘야합니다.
하지만 읽기 요구사항이라면 구현할때 세터를 구현해도 상관이 없습니다.

```swift
protocol GoodProtocol {
    var readWriteProperty: Int { get set }
    var readOnlyProperty: Int { get }
}

protocol GoodTypeProtocol {
    static var readWriteTypeProperty: Int { get set }
    static var readOnlyTypeProperty: Int { get set }
}

class someClass: GoodProtocol {
    private var someTemp: Int = 0
    var readWriteProperty: Int {
        get {
            return someTemp
        }
        set {
            someTemp = newValue
        }
    }
    var readOnlyProperty: Int {
        get {
            return someTemp
        }
        set {   // 구현에 따라 결정
            someTemp = newValue
        }
    }
    
    func printSomeTemp() {
        print("\(someTemp)")
    }
}

let test: someClass = someClass()
test.readWriteProperty = 10 //10으로 값 셋팅
test.printSomeTemp()    //10

test.readOnlyProperty = 20  //settable를 구현하여 값 셋팅
test.printSomeTemp()    //20
```

## 프로토콜 메서드 요구사항

해당 프로토콜을 채택하는 타입이 구현해야하는 메서드를 요구합니다.
프로토콜의 메서드 요구에서는 매개변수 기본값을 지정할 수 없습니다.
타입 메서드를 요구할땐 `static` 키워드를 사용합니다.
 - `func [메서드명] { 매개변수 }`

```swift
protocol someOneProtocol {
    func shouldMakeFunc(someValue: String)
}

class someOneClass: someOneProtocol {
    func shouldMakeFunc(someValue: String) {    //채택한 프로토콜의 요구하는 메서드를 구현한다.
        print("function loop [\(someValue)]")
    }
}

let someInstanc: someOneClass = someOneClass()
someInstanc.shouldMakeFunc(someValue: "test")   //function loop [test]
```

## 참조
[Apple swift document - Protocol](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html)

###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)






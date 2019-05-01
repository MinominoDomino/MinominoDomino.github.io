---
layout: post
title:  "[SWIFT] 함수(Function)"
subtitle:   "SWIFT 함수"
categories: devlog
tags: swift

---

함수(Fionction)를 알아보자.

함수는 하나의 작은 기능을 가진 단위입니다.
키워드로 `func`를 사용합니다.
함수는 기본적으로 함수이름, 매개변수, 반환 타입, 반환값으로 구성되어있습니다.
 - `func [함수이름] ([매개변수]) -> [반환 타입] { return [반환값] }`

[Apple swift document - Function](https://docs.swift.org/swift-book/LanguageGuide/Functions.html)

## 함수의 정의와 호출

함수를 정의하고 호출하는 방법에 대해 가장 기본적인 사용자의 이름을 출력하는 예제를 만들어 볼게요.
 
```swift
func helloMyname(name: String) -> String {
    return "hello, \(name)"
}
print(helloMyname(name: "MINOMI"))  //hello, MINOMI
```

## 함수의 여러가지 정의 방법
함수는 어떻게 정의하냐에 따라서 여러 방법이 존재합니다.
- 매개변수가 없는 함수
- 매개변수가 여러 개인 함수
- 반환 값이 없는 함수
- 반환 값이 여러 개인 함수
- 매개변수에 레이블이 있는 함수
- 매개변수에 기본값이 있는 함수
- 매개변수 인자값 생략 함수
- 매개변수 타입이 집합형인 함수
- 매개변수에 in-out키워드 함수

하나씩 예제를 통해 알아볼게요.

### 매개변수가 없는 함수
매개변수로 아무것도 받지않는 helloPrint함수입니다.
```swift
func helloPrint() -> String {
    return "hello"
}
print(helloPrint()) //hello
```

### 매개변수가 여러 개인 함수
매개변수로 String형 name과 Int형 age를 받는 helloPrint함수입니다.
```swift
func helloPrint(name: String, age: Int) -> String {
    return "hello my name is \(name) and \(age)"
}
print(helloPrint(name: "MINOMI", age: 20))  //hello my name is MINOMI and 20
```

### 반환 값이 없는 함수
반환 값이 없는 helloPrint함수입니다.
**정확하게는 반환 값을 선언하지 않았지만 반환 값이 실제로는 Void라는 형태를 반환받습니다.**
```swift
func helloPrint(name: String) {
    print("hello, \(name)") //hello, MINOMI
}
helloPrint(name: "MINOMI")
```

### 반환 값이 여러 개인 함수
Int형 배열을 입력받아 배열 내부의 값중 가장 큰값과 가장 작은값을 반환하는 minMax함수입니다.
```swift
func minMax(arr: [Int]) -> (max: Int, min: Int) {
    var max = arr[0]
    var min = arr[0]
    
    for value in arr {
        if(value > max) {
            max = value
        } else if(value < min) {
            min = value
        }
    }
    return (max, min)
}
var result = minMax(arr: [5, 6, 34, 3, 4, 4, 1])
print("max is \(result.max) and min is \(result.min)")  //max is 34 and min is 1
```

### 매개변수에 레이블이 있는 함수
시작에서 부터 끝까지 정수를 더하여 반환하는 maxFromTo함수입니다. 
매개변수에 레이블이 있어 함수를 사용할때 레이블의 값이 노출됩니다.
```swift
func maxFromTo(From start: Int, To end: Int) -> Int {
    var ret: Int = 0
    for value in start...end {
        ret += value
    }
    return ret
}
print(maxFromTo(From: 0, To: 10))   //55
```

### 매개변수에 기본값이 있는 함수
위 maxFromTo함수의 매개변수에 start의 기본값은 0, end의 기본값은 2로 설정하여 함수 호출 시 매개변수가 없으면 설정한 기본값으로 동작하는 함수입니다.
```swift
func maxFromTo(start: Int = 0, end: Int = 2) -> Int {
    var ret: Int = 0
    for value in start...end {
        ret += value
    }
    return ret
}
print(maxFromTo())  //3
```

### 매개변수 인자값 생략 함수
_ 를 매개변수 앞에 붙혀 함수 호출시 인자값의 이름이 없어도 호출되도록 합니다.
```swift
func maxFromTo2(_ start: Int = 0, _ end: Int = 2) -> Int {
    var ret: Int = 0
    for value in start...end {
        ret += value
    }
    return ret
}
print(maxFromTo2(0, 5))  //15
```

### 매개변수 타입이 집합형인 함수
Int범위의 특정 값들을 받아 더한 값을 주는 maxValue함수입니다.
```swift
func maxValue(_ values: Int...) -> Int {
    var ret: Int = 0
    for value in values {
        ret += value
    }
    return ret
}
print(maxValue(1, 2, 3, 4, 5, 6))   //21
```

### 매개변수에 in-out키워드 함수
기본적으로 매개변수로 넘어온 변수값은 함수블록 내부에서 제어할 수 없도록 let으로 할당되어 있습니다.
 하지만, 특정 상황에서 매개 변수의 값이 필요할때 `in-out`키워드로 변수의 값을 제어할 수 있습니다.
 여기서 `call-by-value`와 `call-by-reference`개념이 나옵니다.
 
 시작부터 끝까지의 인덱스 만큼 숫자를 순차적으로 더하는 예제입니다.
 여기서, 시작의 값을 가진 startNumber의 값은 0 그리고 끝의 값을 가진 endNumber의 값은 3이고
 maxFromTo3()함수에 매개변수로 0과 3을 가진 변수를 넣어줍니다.
 함수 선언에는 end에만 inout키워드가 있고 함수 내부에서 end의 값을 7로 바꿔버립니다.
 실행을 해보면 함수매개 변수로는 3을 넘겼지만 블록 내부에서 값이 7로 바뀌고 결과적으로 28이 출력되는 것을 볼 수 있습니다.
 왜그런가요??
 
 기본적으로 매개변수는 call-by-value로 값을 넘기게 되는데 이때는 새로운 메모리에 값을 복사하여 넘겨줍니다.
 이유는, inout키워드를 쓰면서 매개변수end가 call-by-refernce로 값을 넘기기 때문입니다.
 그래서 함수 호출에 보면 `&`가 붙어 end변수의 주소 자체를 넘기게 되고 함수 내부에서 변수 주소로 직접적으로 바꿔버리게 되죠.
 따라서 endNumber변수를 출력하면 3이아닌 7인것을 볼 수 있습니다.
```swift
func maxFromTo3(start: Int, end: inout Int) -> Int {
    end = 7 //값 변경
    var ret: Int = 0
    for value in start...end {
        ret += value
    }
    return ret
}
var startNumber:Int = 0
var endNumber:Int = 3
print(maxFromTo3(start: startNumber, end: &endNumber))  //28
print(endNumber)    //7
```


###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)







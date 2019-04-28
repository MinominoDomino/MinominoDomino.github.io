---
layout: post
title:  "[SWIFT] 제어문(Control Flow) 1부"
subtitle:   "SWIFT 제어문 1부"
categories: devlog
tags: swift

---

제어문을 알아보자.

제어문은 특정 조건에 따라 기능을 제어하는 기능입니다.
제어문에는 `for-in`, `while`, `switch`, `if guard`가 있습니다.
1부에서는 for in과 while를 다뤄볼게요.
하나씩 알아봅시다.

[Apple swift document - Control Flow](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)

## for-in 루프

가장 기본적으로 배열에 담긴 자료를 순차적으로 순회를 합니다.

- `for [임시변수명] In [배열명] { }`

```swift
// "KIM", "LEE", "PARK"이 들어있는 배열 names입니다.
var names: [String] = ["KIM", "LEE", "PARK"]

for name in names {
	print("name is \(name)")
}
//name is KIM
//name is LEE
//name is PARK
```

이번에는 딕셔너리에 담긴 자료를 순차적으로 순회합니다.
- `for ([key임시변수명],[value임시변수명]) In [딕셔너리명] { }`

```swift
// ["KIM":10, "LEE":20, "PARK":30]이 들어있는 딕셔너리 타입의 personInfo입니다.
var personInfo: [String: Int] = ["KIM":10, "LEE":20, "PARK":30]

for (name, age) in personInfo {
	print("name is \(name) and age is \(age)")
}
//name is KIM and age is 10
//name is LEE and age is 20
//name is PARK and age is 30
```

이번에는 숫자 범위를 정해서 순회를 합니다.
- `for [임시변수명] In [from...to]`
```swift
for number in 5...10 {
    print(number)   //5 6 7 8 9 10
}
```

이번에는 범위 연산자를 사용해서 순회를 합니다.
- `for [임시변수명] In [from..[범위연산자]to]`
```swift
var clock: Int = 13
for time in 1..<clock {
    print(time) //1 2 3 4 5 6 7 8 9 10 11 12
}
```

 stride(from:to:through) 함수를 사용해서 순회를 할 수 있습니다.
stride함수는 from값에서 to값까지 through만큼 떨어지면서 시퀀스를 만드는 함수입니다.
```swift
for value in stride(from: 0, through: 6, by: 2) {
    print(value)    //0 2 4 6
}
```

값 제어없이 단순 반복경우, 임시변수를 `_`로 생략할 수 있고 성능을 더 높일 수 있습니다.
- `for _ In [from...to] `
```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")  //3 to the power of 10 is 59049
```

## While 루프
while은 특정 조건이 만족하는 동안 루프를 실행하고 조건이 만족되지 않으면 루프를 빠져나와요.
 기본 while과 repeat-while 두 종류의 루프가 있습니다.
 repeat-while은 기본 while과 다르게 먼저 루프를 실행하고 조건 검사가 이루워집니다.
 가장 기본적인 예제를 해볼게요.
 - 원형 표현 `while [조건] { }`
 - 원형 표현 `repeat { } while [조건]`

```swift
ar startTime: Int = 0
var endTime: Int = 10

while startTime < endTime {
    startTime += 1
}
print("startTime is \(startTime)")  //startTime is 10

repeat {
    startTime += 1
} while startTime < endTime
print("startTime is \(startTime)")  //startTime is 11
// repeat-while은 무조건 루프를 먼저 실행하고 검사를 하게되어 startTime이 11로 증가했어요.
```




###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)







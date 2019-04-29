---
layout: post
title:  "[SWIFT] 제어문(Control Flow) 2부"
subtitle:   "SWIFT 제어문 2부"
categories: devlog
tags: swift

---

제어문을 알아보자.

제어문은 특정 조건에 따라 기능을 제어하는 기능입니다.
제어문에는 `for-in`, `while`, `switch`, `if`가 있습니다.
2부에서는 switch와 if를 다뤄볼게요.
하나씩 알아봅시다.

[Apple swift document - Control Flow](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)

## if else 

 if else는 특정 조건이 만족하는지 확인하여 조건이 참이라면 블록을 실행하고 거짓이라면 실행하지 않습니다.
 **swift에서 조건식의 타입은 bool형이여야 합니다.**
 아래 4가지 표현에 대한 간단한 예제를 보겠습니다.
 
 - `if [조건식] { }`
 - `if [조건식] { } else { }`
 - `if [조건식] { } else if [조건식] { }`
 - `if [조건식] { } else if [조건식] { } else { }`

```swift
var testSuccess: Int = 60
var myScore: Int = 55
var friendScore: Int = 70

// 표현 1
if myScore < testSuccess {
    print("test Fail")	//출력
}
// 표현 2
if myScore > testSuccess {
    print("test Success")
} else {
    print("test Fail")	//출력
}
// 표현 3
if myScore > testSuccess {
    print("test Success")
} else if myScore < testSuccess {
    print("test Fail")	//출력
}
// 표현 4
myScore = 60
if myScore > testSuccess {
    print("test Success")
} else if myScore < testSuccess {
    print("test Fail")
} else {
    print("same Score")	//출력
}
```

## switch case
 switch case는 입력 값에 대해 각 case별로 비교하여 조건에 만족하는 case의 블록을 실행합니다.
 다른 언어의 switch문에서 break키워드로 구문을 종료했지만, swift에서는 break키워드 없이도 case 내부의 코드를 모두 실행하면 구문이 종료됩니다.
 이로인해 break 키워드가 없어 다른 case를 의도하지 않게 실행되는 것을 방지할 수 있습니다.
 단, case의 블록 기능은 작성해주셔야합니다.
 간단한 예제를 보겠습니다.
 - `switch [조건변수] { case [조건식]: 실행블록 }`

```swift
var score: Int = 55
switch score {
case 100:   //100점 이라면
    print("A Grade")
case 80...99:    //80~99점 이라면
    print("B Grade")
case 60..<80:   //60~79점 이라면
    print("C Grade")
case 55,56,57,58,59:    //55~59점 이라면
    print("D Grade")	//출력
default:
    print("unknow")
}
```

첫 번째, case처럼 **하나의 값**을 지정
두 번째, case처럼 **범위 값**을 지정
세 번째, case처럼 **범위연산자**를 지정
네 번째, case처럼 **여러 값**을 지정

또한, 다음과 같이 튜플과 값을 바인딩할수있어요.
```swift
var currentPoint = (1, 20)
switch currentPoint {
case (0, 0):
    print("my current point is 0, 0")
case (0...2, 0...5):
    print("my current point is 0~2, 0~5")
case (_, 0):
    print("my current point is N, 0")
case (0, _):
    print("my current point is 0, N")
case (let x, 1):
    print("my current point is \(x), 1")
case (1, let y):
    print("my current point is 1, \(y)")	//출력
case (_, _):
    print("notting")
}
```

case에 `where` 조건을 사용할 수 있어요.
```swift
var myPoint = (1, 2)
switch myPoint {
case let (x, y) where x == y:
    print("x and y is same value")
case let (x, y) where x != y:
    print("x and y is not same value")	//출력
case (_, _):
    print("notting")
}
```

제어문에는 실행되는 코드의 흐름을 바꾸기 위해 제어 전송 구문을 제공해요.
하나씩 알아보죠.

- `continue`

continue는 현재 블록을 종료하고 다음 블록을 실행합니다.
알 수 없는 문자열에서 특정 문자를 제거하여 해독하는 예제입니다.
```swift
var quizString: String = "s12u4cc3e9s11s"
var quizAnswer: String = ""
var removeChar: [Character] = ["1", "2", "3", "4", "9"]

for char in quizString {
    if(removeChar.contains(char)) {
        continue
    } else {
        quizAnswer.append(char)
    }
}
print(quizAnswer)	//success
```

- `break`

break는 제어문의 실행을 종료합니다.
위 예제에서 'c'라는 문자는 위험하여 'c'를 제외하고 이어나갑니다.
```swift
quizAnswer.removeAll()  //quizAnswer를 초기화합니다.
for char in quizString {
    switch char {
    case "c":
        break
    case "1"..."9":
        continue
    case _:
        quizAnswer.append(char)
    }
}
print(quizAnswer)	//suess
```

- `fallthrough`

fallthrough는 자동으로 break되는걸 무시하고 다음 case문을 게속 실행합니다.
다음 예제는 선택한 숫자를 알려줍니다.5를 선택했지만 case 6도 같이 실행됩니다.
```swift
var selectedNumber: Int = 5
switch selectedNumber {
case 1...4:
    print("selected number is within 1~4")
case 5:
    print("selected number is 5")
    fallthrough
case 6:
    print("selected number is 6")
default:
    print("unknow")
}
//selected number is 5
//selected number is 6
```



###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)







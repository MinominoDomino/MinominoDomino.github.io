---
layout: post
title:  "[SWIFT] 클래스와 구조체(Class and Struct)"
subtitle:   "SWIFT 클래스와 구조체"
categories: devlog
tags: swift

---

클래스와 구조체(Class and Struct)를 알아보자.

클래스는 공통적인 프로퍼티나 메소드를 묶어 하나로 제공하는 타입입니다.
키워드로 `class`를 사용합니다.
클래스 이름의 시작은 대문자로 시작합니다.
 - `class [클래스이름] { }`

[Apple swift document - Class](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html)

## 클래스 선언과 인스턴스

기본적인 프로퍼티와 메소드를 가진 클래스를 선언하고 인스턴스를 생성하는 예제를 해볼게요.
클래스의 프로퍼티는 초기값을 넣어줘야 함, 옵셔널 프로퍼티 혹은 생성자에서 초기값을 설정해 줍니다.
생성자는 `init`키워드를 사용합니다.
소멸자는 `deinit`키워드를 사용합니다.
또한 메서드에서 클래스 프로퍼티와 동일한 이름을 구분해주기 위해 자바의 this키워드가 스위프트에서는 `self`입니다.
그럼 사람 정보를 관리하는 클래스 예제입니다.
 
```swift
class PersonInfo {
    //클래스 프로퍼티
    var name: String
    var age: Int
    
    //생성자
    init() {
        name = ""
        age = 0
    }
    deinit {
        //nothing to do at now
    }
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    //클래스 메서드
    func setInfo(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    func getInfo() -> (name: String, age: Int) {
        return (self.name, self.age)
    }
}

//var someOne = PersonInfo()
var someOne = PersonInfo(name: "Lee", age: 25)
print(someOne.getInfo())    //(name: "Lee", age: 25)

someOne.setInfo(name: "KIM", age: 20)
print(someOne.getInfo())    //(name: "KIM", age: 20)
```

## 클래스 상속

클래스 상속은 자식클래스가 부모클래스의 프로퍼티와 메소드를 그대로 이어받는 행위입니다.
따라서 자식클래스가 동일한 프로퍼티와 메소드를 똑같이 작성할 필요가 없습니다.
이것이 OOP의 특징중 상속(Inheritance)에 해당합니다.
자식클래스는 서브클래스, 부모클래스는 슈퍼클래스라고 불리기도 합니다.
 - class [서브클래스] : [슈퍼클래스] { }

[Apple swift document - Inheritance](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html)

다음 예제는 스마트폰 클래스를 상속받는 애플폰 클래스를 구현하고 애플폰 클래스로 아이폰을 만드는 예제입니다.
 
```swift
class SmartPhone {
    var model: String = ""
    var modelNumber: String = ""
    init(model: String, modelNumber: String){
        self.model = model
        self.modelNumber = modelNumber
    }
    
    func powerOff() {
        print("핸드폰을 종료합니다")
    }
    func callSomeone(number: String) {
        print("\(number)로 전화를 걸고있습니다.")
    }
}

class ApplePhone: SmartPhone {
    var brand: String = "Apple"
    var date: String = "2019-05-01"
    init(){ super.init(model: "AAA23-34", modelNumber: "1-AA-BBE443") }
    
    func phoneInfo() {
        print("모델명(\(model)), 모델번호(\(modelNumber)), 제조사(\(brand)), 제조년월(\(date)) 입니다.")
    }
}

var iphone = ApplePhone()
iphone.phoneInfo()  //모델명(AB-123-AA), 모델번호(ABCC1AA), 제조사(Apple), 제조년월(2019-05-01) 입니다.
iphone.callSomeone(number: "010-2234-4545") //010-2234-4545로 전화를 걸고있습니다.
iphone.powerOff()   //핸드폰을 종료합니다
```

## 오버라이드

오버라이드는 부모클래스 메서드의 기능을 재정의하는 기능입니다.
이것이 OOP의 특징중 오버라이딩[Overriding]에 해당합니다.
재정의하는 메서드 앞에 `override`키워드를 사용합니다.
   - override func [메서드이름] { }

`super`키워드는 부모클래스로 접근을 의미합니다.

또한, `final` 키워드를 사용하여 서브클래스에서 오버라이드를 방지 할 수 있습니다.
final로 선언된 메서드나 프로퍼티를 오버라이드 하면 컴파일에서 에러가 발생합니다.

[Apple swift document - Overried](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html)
다음 예제는 스마트폰 클래스를 상속받는 삼성폰 클래스를 구현하고 스마트폰 클래스의 powerOff메서드를 재정의하는 예제입니다.
 
```swift
class SamsungPhone: SmartPhone {
    var brand: String = "Samsung"
    var date: String = "2019-05-02"
    init(){ super.init(model: "BB24-22", modelNumber: "2-BB-BEA543") }
    
    override func powerOff() {
        print(brand)
        super.powerOff()
    }
}

var galaxy = SamsungPhone()
galaxy.powerOff()   //Samsung
                  //핸드폰을 종료합니다
```

## 구조체 선언과 인스턴스
구조체는 공통적인 프로퍼티나 메소드를 묶어 하나로 제공하는 타입입니다.
키워드로 `struct`를 사용합니다.
구조체의 이름은 대문자로 시작합니다.
- `struct [구조체이름] { }`

```swift
struct NoteBook {
    var cpu: String = ""
    var ram: String = ""
    var ssd: String = ""
    
    init(cpu: String, ram: String, ssd: String) {
        self.cpu = cpu
        self.ram = ram
        self.ssd = ssd
    }
    
    func noteBookInfo() {
        print("cpu: \(cpu), ram: \(ram), ssd: \(ssd)")
    }
}

var mac = NoteBook(cpu: "i5", ram: "16", ssd: "256")
mac.noteBookInfo()  //cpu: i5, ram: 16, ssd: 256
```

## 클래스와 구조체의 비교

클래스와 구조체는 다음과 같은 공통점을 지닙니다.
  - 프로퍼티 정의
  - 메서드 정의
  - 생성자를 통한 초기화
  - extension, protocol 사용 가능

반대로 차이점으로, 클래스만 지니는 사항은 다음과 같습니다.
 - 상속
 - 타입 캐스팅
 - 소멸자를 통한 리소스 정리
 - 참조 타입

클래스와 구조체의 가장 큰 차이점으로 클래스는 `참조타입`이고 구조체는 `값타입`입니다.
참조타입은 클래스 인스턴스를 동일하게 사용하게되고 참조 카운트가 참조된 횟수만큼 증가하게됩니다.
 
값타입은 구조체 인스턴스를 하나더 복사하게됩니다.
자료형으로 할당을 하거나 전달을 할때 새롭게 복사가 되기때문에 스위프트의 자료형들은 구조체로 구현이 되어있습니다.
 
이를 예제를 보겠습니다.
동일한 프로퍼티와 메서드를 지닌 ClassCar클래스와 StructCar구조체를 구현하고 각 인스턴스를 다른 변수에 전달하여 값을 변경하는 예제입니다.
결과를 보면 클래스 인스턴스의 값이 변경되었지만, 구조체 인스턴스는 값이 그대로인것을 볼 수있습니다.

```swift
class ClassCar {
    var brand = ""
    init(brand: String){ self.brand = brand }
    func brandName() { print(brand) }
}

struct StructCar {
    var brand = ""
    init(brand: String){ self.brand = brand }
    func brandName() { print(brand) }
}

var kia = ClassCar(brand: "KIA")
kia.brandName() //KIA
var kiaSub = kia
kiaSub.brand = "KIASUB"
kiaSub.brandName()  //KIASUB
kia.brandName() //KIASUB

var hyundai = StructCar(brand: "HYUNDAI")
hyundai.brandName() //HYUNDAI
var hyundaiSub = hyundai
hyundaiSub.brand = "HYUNDAISUB"
hyundaiSub.brandName()  //HYUNDAISUB
hyundai.brandName() //HYUNDAI
```

###### 해당 챕터의 playground 테스트 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/swift-grammar-house)







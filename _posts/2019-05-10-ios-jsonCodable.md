---
layout: post
title:  "[iOS] JSON 파싱해보기(JSON Encode, Decode)"
subtitle:   "iOS JSON Encode, Decode"
categories: devlog
tags: ios

---

JSON 파싱해보기(JSON Encode, Decode)를 알아보자.


## Codable은?
`Codable`은 `Decodable`와 `Encodable` 타입을 합쳐놓은 타입입니다.
Decodable와 Encodable은 프로토콜입니다.
그럼 클래스나 구조체에서 채택을 해서 쓰겠죠??
말로 설명보다는 직접 인코딩과 디코딩을 하는 예제를 보겠습니다.

## JSON Encode
먼저 JSON형식으로 바꿀 데이터 구조를 만들고 Codable를 채택해줍니다.
예제에서는 영화정보를 지닌 객체를 JSON으로 변환하겠습니다.
영화정보는 구조체로 name, year, count를 가지고있습니다.
```swift
struct MovieInfo: Codable {	//Codable채택
    var name: String?
    var year: String?
    var count: Int?
}
```

그리고 MoviewInfo객체를 만들어줍니다.
```swift
let ones = MovieInfo(name: "엑스맨", year: "2000", count: 3)
```

JSONEncoder객체를 만들고 인코더의 포멧을 정해줍니다.
```swift
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
```

JSONEncoder의 encode메서드로 인코딩을 시작합니다.
encode메서드는 `throws` 이니 try를 사용하고 do catch문으로 감싸줘야합니다,
```swift
do {
    let data = try encoder.encode(ones)
    print(String(data: data, encoding: .utf8)!)
} catch {
    print(error)
}
```
이렇게 실행을 해보면 다음 그림처럼 이쁘게 변환을 해주네요.
![1.png](https://MinominoDomino.github.io/assets/img/ios/jsonCodable/1.png)

## JSON Decode
이번엔 변환한 JSON을 객체로 만드는 작업을 해볼게요.
매우 간단해요. 일단 테스트를 위한 json스트링을 만들게요.

그리고 디코더 객체를 만들고 decode메서드를 실행하면 끝입니다. 엄청 간단하죠?
매개변수의 type은 변환할 데이터구조 타입를 넣어주고 from에는 json스트링을 넣어주면되요.
```swift
/*
let json = """
{
  "name" : "아이언맨",
  "year" : "2008",
  "count" : 100
}
""".data(using: .utf8)!
*/
let decoder = JSONDecoder()

do {
    let movieData = try decoder.decode(MovieInfo.self, from: json)
    print("\(movieData.name!), \(movieData.year!), \(movieData.count!)")	//아이언맨, 2008, 100
} catch {
    print(error)
}
```

## CodingKey
JSON에서 사용하는 Key이름과 데이터 구조의 이름이 다를 경우가 있어요.
이럴 때는 어느 한쪽을 바꾸라고 할 순 없죠?
이 경우에 codingkey를 채택해서 쓰면되는데 얘는 enum에 채택해주면됩니다.

`enum CodingKeys: String, CodingKey { case [데이터구조 key] = "[JSON key]" }`
이렇게 사용하게되고 case에서 서로 다른 이름이 맵핑이됩니다.
예제로 직접 보겠습니다.

데이터 구조는 name, age로 쓰지만 JSON에서는 heroName, heroAge를 사용하는 예제로,
enum에 codingkey를 채택해서 이름 매핑시켜 json포맷으로 인코딩 해보는 예제입니다.

```swift
struct HeroInfo: Codable {
    var name: String = ""
    var age: Int = 0
    var ability: String = ""
    
    enum CodingKeys: String, CodingKey {
        case name = "heroName"
        case age = "heroAge"
        case ability
    }
}


var ironMan = HeroInfo(name: "Tony", age: 40, ability: "nop")

let jsonEncoder = JSONEncoder()
jsonEncoder.outputFormatting = .prettyPrinted

do {
    let txt = try jsonEncoder.encode(ironMan)
    print(String(data: txt, encoding: .utf8)!)
} catch {
    print(error)
}
```

실행해보면 json포맷에서 key가 바뀐걸 확인할 수 있습니다.
반대로 디코딩도 직접 해보세요. 
소스에 적어놨습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/jsonCodable/3.png)

## 직접 해보기
너무 간단해서 이를 이용해서 우리 배운 테이블 뷰에 데이터를 넣는 예제를 해보세요!
해당 예제는 부스트코스 IOS강좌의 내용을 가져왔습니다.
아래 그림처럼 테이블에 파싱한 데이터를 보여주는 예제를 만들어봅시다~
결과는 밑에 링크에 있습니다.
TEST용으로 쓰는 JSON형태입니다. 데이터를 더 추가해서 사용하세요.
[
{
"name":"하나",
"age":22,
"address_info": {
"country":"대한민국",
"city":"울산"
}
},
{
"name":"주현",
"age":34,
"address_info": {
"country":"대한민국",
"city":"김해"
}
}
]
![2.png](https://MinominoDomino.github.io/assets/img/ios/jsonCodable/2.png)


## 참조
[Apple Developer document - Codable](https://developer.apple.com/documentation/swift/codable)
[Apple Developer document - Encodable](https://developer.apple.com/documentation/swift/encodable)
[Apple Developer document - Decodable](https://developer.apple.com/documentation/swift/decodable)
[Apple Developer document - Codingkey](https://developer.apple.com/documentation/swift/codingkey)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ (플레이그라운드)](https://github.com/MinominoDomino/swift-grammar-house)
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ (직접 해보기 예제)](https://github.com/MinominoDomino/ios-sample-store)


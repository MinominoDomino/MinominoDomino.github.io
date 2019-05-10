---
layout: post
title:  "[iOS] 네이버 검색API 사용하기(NaverAPI)"
subtitle:   "iOS NaverAPI"
categories: devlog
tags: ios

---

네이버 검색API 사용하기(NaverAPI)를 알아보자.

오늘은 네이버 검색API를 사용하는법을 알아보겠습니다.

구글, 네이버, 카카오 등 여러 IT서비스 기업에서는 자사 서비스를 이용 할 수 있는 API를 제공하고있습니다.
이런 API서비스들은 ` HTTP요청과 응답` 으로 데이터를 제공해줍니다.

데이터의 포맷은 보통 ` JSON` 과 ` XML` 로 제공합니다.

IOS JSON 디코딩과 인코딩을 제공하는 codabel 클래스를 알아봤으니 실제로 써보죠.

이번 예제에서는 네이버의 검색API를 이용하여 영화정보를 요청하여 클래스로 변환해보겠습니다.

## API 이용 신청하기
먼저 네이버 개발자 센터로 구경가볼까요?
[네이버 개발자 센터 구경가기](https://developers.naver.com/main/)

네이버는 아래와 같은 여러 API를 제공해주네요.
우리는 검색API를 사용해보겠습니다.
![1.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/1.png)

일단 개발자 가이드를 볼까요?
![2.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/2.png)

검색 API안에는 많은 검색서비스를 지원해주네요
우리는 영화 정보를 받아볼테니 영화 카테고리를 눌러보겠습니다.
원하는대로 네이버 영화 검색 결과를 출력해주는 `REST API`라고하네요 좋습니다.
저는 일단 다짜고짜 API신청을 먼저 해보겠습니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/3.png)

애플리케이션 이름을 설정하고 환경설정에서 iOS 설정으로 하면 iOS Bundle ID를 물어보네요.
이 API서비스를 사용할 프로젝트의 번들아이디를 넣어주고 등록하기를 누릅니다.
![4.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/4.png)

그럼 가장중요한 API를 사용할 수 있는 클라이언트ID와 키를 줍니다.
이 아이디와 키가 있어야 API서비스를 이용할 수 있는 인증을 할 수 있어요!
![5.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/5.png)

## API 정보 확인하기
개발자 가이드에는 각 API별 요청과 응답에 대한 정보와 예제 샘플이 있습니다.
먼저 우리가 쓸 API가 어떻게 생겼나볼게요.
`GET메서드로 요청할 URL`이 있네요. 응답 포맷으로는 `XML`과 `JSON`을 주고 각 URL이 다르죠?
저는 codable를 쓸테니 JSON으로 받게 요청할게요.
![6.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/6.png)

밑에 더 내려가보니 요청 변수가 있네요.
우리가 API요청을 보낼때 URL뒤에 요청할수있는 변수리스트를 보여주고있습니다.
영화 검색API에는 query, display, start, genre, country, yearfrom, yearto가 있네요.
그리고 각 요청변수마다 설명이 써있습니다.
중요한 내용으로 필수 여부항목이 있습니다. 여기에 Y라고 써있는 항목은 요청할때 무조건 들어가있어야 하는 변수입니다. 따라서 꼭 같이 보내주셔야해요. N은 옵셔널이니 상황에 맞게 사용하면 되겠네요.
밑에는 요청에 대한 출력 형태를 알려주고있습니다.

여기까지는 우리가 서비스를 사용하는데 있어 알아야할 `URL주소`와 `요청 변수`, `출력 포멧` 내용입니다.
밑에보면 `에러코드`를 보여주네요 여기는 서비스 요청에 있어 응답으로 오는 에러에 대한 내용을 알려줍니다.
그리고 예시가 있네요.

추가로, API 사용을 신청하고 만들어진 애플리케이션 목록으로 가면 각 요청한 애플리케이션마다 Playground 를 제공하는데요. 이는 실제 요청 변수들을 채워서 보내면, 응답값을 보여줍니다 여기서 확인해보고 써도 되겠죠?
![7.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/7.png)



## REST API 요청하기
프로젝트에서 먼저 swift파일을 새로 만들겠습니다.
이 파일에서는 API 출력 포멧에 맞는 데이터 구조와 json디코딩을 할 수 있도록 준비하고
변환된 객체를 배열에 저장해둘게요.
우리는 json으로 오는 데이터의 구조를 알고 있으니까 거기에 맞게 구조체로 만들어 줄게요.
`Codable 키워드 잊지마세요~`
그리고 싱글톤 객체로 데이터 지닐 데이터 매니저를 만들겠습니다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/8.png)

이제 스토리보드로 이동해서 화면 배치를 해주겠습니다.
아래처럼 만들어주세요.
![9.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/9.png)

뷰컨트롤러 파일로 이동하겠습니다.
네이버로 API를 요청하는 메서드 requestAPIToNaver()를 정의합니다.
여기서는 URL를 만들고 요청하는 작업을 하는 메서드입니다.

먼저 쿼리를 만들어 줍니다.
우리는 요청할 주소를 알고있으니 쿼리뒤에 요청 변수만 설정해줍니다.
TextField에서 검색할 영화이름을 받아와서 쿼리스트링을 만들어줍시다.
쿼리스트링은 단순한 문자열이기때문에 이를 URL형식에 맞게 인코딩을 한번 해줍시다.
`addingPercentEncoding(withAllowedCharacters: NSCharacterSet.urlQueryAllowed)`를 이용합니다.
그리고 쿼리스트링을 URL객체로 만들어 줍시다.
![10.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/10.png)

이제 URL를 hTTP요청하면 되는데요.
호출 예시에 보면 HTTP헤더로 우리가 발급받은 아이디와 시크릿을 붙여줘야해요.
![11.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/11.png)
`URLRequest`에서 `addvalue()`로 붙여줄게요.
![12.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/12.png)

`URLSession.shared.dataTask(with: requestURL)`를 이용해서 HTTP요청을 보내줄게요.
![13.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/13.png)
Task에 대해서는 이후에 자세히 포스팅해드릴게요.
일단 이렇게하면 data에 응답이 오는구나 생각하시면됩니다.
그리고 do catch문을 볼게요. 우리가 배웠던 json디코딩이랑 똑같습니다.
객체를 만들어서 dataManager에게 넘겨주네요.
마지막에 `urlTaskDone()`이라는 함수를 호출하는데 이는 콜백함수라고 합니다.
Task는 비동기 메서드로 메인UI와 분리되어 돌아가요. 그래서 데이터 받는게 다 끝나면 urlTaskDone이라는 함수를 부르라고 해주는거죠.
이렇게 requestAPIToNaver 메서드 작성은 완료됩니다.

이번엔 urlTaskDone 메서드를 작성합니다.
이 메서드에서는 불리게되면 영화 포스터를 다운받고 이미지뷰에 연결해주거나 타이틀 정보를 설정 하는 등
우리가 만든 뷰에 데이터를 붙여주는 역활을 합니다.
전에 배운대로 이미지 다운로드는 다른 쓰레드에서 하고 완료되면 UI의 설정 작업은 전부 메인쓰레드에서 작동합니다.
![14.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/14.png)

검색 버튼 액션에서는 타이틀에 입력된 데이터를 requestAPIToNaver 에 넘겨주면 되겠네요.
아래는 뷰컨트롤의 전체 소스입니다.
```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var searchTextField: UITextField!
    @IBOutlet weak var posterImageView: UIImageView!
    @IBOutlet weak var movieTitleLabel: UILabel!
    let jsconDecoder: JSONDecoder = JSONDecoder()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
    @IBAction func touchUpSearchUpButton(_ sender: Any) {
        let queryValue: String = searchTextField.text!
        requestAPIToNaver(queryValue: queryValue)
    }
    
    func urlTaskDone() {
        let item = dataManager.shared.searchResult?.items[0]
        
        do {
            let imageURL = URL(string: item!.image)
            let imageData = try Data(contentsOf: imageURL!)
            let posterImage = UIImage(data: imageData)
            OperationQueue.main.addOperation {
                self.posterImageView.image = posterImage
                self.movieTitleLabel.text = item?.title
            }
            
        } catch { }
    }
    
    func requestAPIToNaver(queryValue: String) {
        let clientID: String = "3DOMN3s7MnQABHxtjVLO"
        let clientKEY: String = "3dREeFGVdw"
        
        let query: String  = "https://openapi.naver.com/v1/search/movie.json?query=\(queryValue)"
        let encodedQuery: String = query.addingPercentEncoding(withAllowedCharacters: NSCharacterSet.urlQueryAllowed)!
        let queryURL: URL = URL(string: encodedQuery)!
       
        var requestURL = URLRequest(url: queryURL)
        requestURL.addValue(clientID, forHTTPHeaderField: "X-Naver-Client-Id")
        requestURL.addValue(clientKEY, forHTTPHeaderField: "X-Naver-Client-Secret")
        
        let task = URLSession.shared.dataTask(with: requestURL) { data, response, error in
            guard error == nil else { print(error); return }
            guard let data = data else { print(error); return }
            
            do {
                let searchInfo: SearchResult = try self.jsconDecoder.decode(SearchResult.self, from: data)
                dataManager.shared.searchResult = searchInfo
                self.urlTaskDone()
            } catch {
                print(fatalError())
            }
        }
        task.resume()
    }
}
```

이제 실행해서 데이터를 받아오는지 확인해 봅시다.
요새 최고의 인기 영화 "어벤져스 엔드게임"을 검색해볼게요.
잘 가져오는군요!!!
이렇게 네이버 API사용과 JSON디코딩에 대해 알아봤습니다.
![15.png](https://MinominoDomino.github.io/assets/img/ios/naverAPI/15.png)

##유의 사항!!!!! (중요)
예제소스에서는 간단히 이미지뷰와 타이틀만 출력해보았고 검색에 대해 정보도 첫번째 아이템만 가져왔습니다.
여러분들이 직접 확인해보면서 이것저것 해보시길바래요...

`그리고 가장중요한!!!! 강제언래핑이 많이 되어있는데 핵심만 말씀드리고 소스를 간단히 하기위해서 일부러 강제언래핑했습니다. 이렇게 하면 엄청 위험한거 다들아시죠?? 본인들 프로젝트 하실때는 꼭 guard let이나 if let으로 감싸주세요~~~~ 안그러면 크러쉬 엄청나올거에요....ㅎㅎㅎㅎ`


## 참조
[네이버 검색 서비스API 문서](https://developers.naver.com/docs/search/movie/)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/ios-sample-store)

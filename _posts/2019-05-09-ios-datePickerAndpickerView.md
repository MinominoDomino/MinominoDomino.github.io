---
layout: post
title:  "[IOS] 데이트 피커(UIDatePicker)와 피커뷰(UIPickerView)"
subtitle:   "iOS UIDatePicker & UIPickerView"
categories: devlog
tags: ios

---

데이트 피커(UIDatePicker)와 피커뷰(PickerView)를 알아보자.

`Date Picker`는 날짜 선택 도구입니다.
아이폰에서 알람을 설정하거나 웹에서 회원가입을 할때 자주보이는 컨트롤입니다.
Date Picker는 델리게이트나 데이터소스가 따로 없습니다.
Picker View는 델리게이트와 데이터소스를 지정해야합니다.
s
`class UIDatePicker : UIControl` 

`class UIPickerView : UIView` 

![1.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/1.png)

## Date Picker 모드
Date Picker는 기본적으로 4가지의 모드를 제공합니다.
 - dateAndTime Mode
 - date Mode
 - time Mode
 - countDownTimer Mode

![2.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/2.png)


## Date Style과 Time Style
Date Picker에서 가져오는 정보를 DateFormatter를 이용하여 표현할 수 있습니다.
그리고 Date부분 Style과 Time부분 Style을 지정해줄 수 있습니다.
각각 스타일은 5가지를 제공합니다.
날짜와 시간 부분의 스타일을 섞어서 사용할 수 있습니다.

| 모드 | 날짜 | 시간 |
|--------|--------|
|none|아무 스타일도 없음|아무 스타일도 없음|
|short|10/9/19| 2:31 AM|
|medium|Oct 9, 2019| 2:31:06 AM|
|long|October 9, 2019| 2:31:06 AM GMT+9 |
|full|Wednesday, October 9, 2019| 2:31:06 AM Korean Standard Time|

## 예제
Date Picker의 값을 화면에 출력하고 데이터를 커스텀 할수 있는 UIPickekView를 만들어 보겠습니다.

새로운 프로젝트를 생성해주세요.
스토리보드에서 아래 그림처럼 뷰를 배치 해줍니다.
![3.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/3.png)

ViewController로 이동합니다.
스토리보드에 뷰들의 아웃렛과 액션을 아래 그림처럼 설정해 줍니다.
그리고 UIPickerView의 `UIPickerViewDelegate`와 `UIPickerViewDataSource` 프로토콜을 채택해줍니다.
필수로 작성되어여하는 numberOfComponents(in pickerView: UIPickerView)와 pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int)를 오버라이드 해줍니다.
추가로 pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int)와 pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int)를 사용할게요. 
![4.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/4.png)

`numberOfComponents(in pickerView: UIPickerView)`는 PickerView에서 컴포넌트 갯수를 알려주면됩니다.
우리는 Date부분과 Time부분으로 나눌테니 2개로 주겠습니다.
![5.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/5.png)

`pickerView( pickerView: UIPickerView, numberOfRowsInComponent component: Int)`는 각 컴포넌트 별로 행의 갯수를 알려주면 됩니다. Date와 Time의 스타일은 각각 5개니까, 그냥 5를 줘도되지만 배열을 만들어서 배열 갯수를 주겠습니다.
![6.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/6.png)
![7.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/7.png)

`pickerView( pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int)`는 행의 타이틀을 넣어주는 함수입니다. 각 로우를 인덱스로 배열의 값을 리턴해줍시다.
![8.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/8.png)

`pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int)`는 PickerView의 델리게이트 메서드입니다. 행이 선택되었을때 호출됩니다. 여기서는 각 Style를 설정하겠습니다.
먼저 DateFormatter객체를 하나 만들어줍니다.
델리게이트 메서드에서 각각 컴포넌트에 설정된 값을 가져와서 DateFormatter객체의 스타일로 각각 지정해줍니다. (강제언래핑 피해야합니다.)
그리고 표시되는 값을 지우고 화면을 새로 그리겠습니다.
![9.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/9.png)
![10.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/10.png)

`@IBAction func datePicker(_ sender: UIDatePicker)` 에서 DatePicker를 움직였을 때 해당 설정된 값을 받아와서 레이블에 표시해주겠습니다.
![11.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/11.png)

`@IBAction func touchUpChangeModeButton(_ sender: UIButton)`에서 버튼을 누를때 마다 DatePicker의 Mode를 변경해주겠습니다. 화면을 다시 그려줘야 합니다.
![12.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/12.png)

이렇게 코딩을 하고 실행하기 전에 스토리보드로 이동해서 PickerView의 데이터소스와 델리게이트를 뷰 컨트롤러로 지정해줍니다.

그리고 실행을 해보겠습니다.
잘되는군요!!!
모드를 바꿔가면서 레이블이 어떻게 변하는지 보고 DatePicker와 PickerView를 살펴봤습니다.
![13.png](https://MinominoDomino.github.io/assets/img/ios/UIDatePickerAndUIPickerView/13.png)


## 참조
[Apple Developer document - UIDatePicker](https://developer.apple.com/documentation/uikit/uidatepicker)

[Apple Developer document - UIPickerView](https://developer.apple.com/documentation/uikit/uipickerview)


###### 해당 챕터의 예제 소스는 제 깃헙에 올라가 있어요~
###### [눌러서 이동! star(별) 꾸욱 해주세요ㅎㅎ](https://github.com/MinominoDomino/ios-sample-store)








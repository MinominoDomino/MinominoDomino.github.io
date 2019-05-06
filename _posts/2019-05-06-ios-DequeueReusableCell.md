---
layout: post
title:  "[IOS] dequeueReusableCell 함수(dequeueReusableCell)"
subtitle:   "IOS dequeueReusableCell"
categories: devlog
tags: ios

---

dequeueReusableCell 함수(dequeueReusableCell)를 알아보자.

## dequeueReusableCell()
테이블뷰나 컬렉션뷰에서 셀을 넣을때 이 함수를 썻어요.
일단 그냥 함수명으로 유추해볼게요 `dequeue` 큐에서 뭔가를 빼네요?
`ReusableCell` 재사용가능한 셀을...
아하! 이 함수는 재사용가능한 셀을 큐에서 빼내오는 함수인가보네요
함수의 원형을 보니 리턴 타입이 `UITableViewCell` 이네요!! 
![7.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/7.png)

그럼 확실히 큐에서 셀을 뺴서 준다는 말인데.... 무슨 말일까요...?
"셀은 분명 내가 게속 만들어주는 건데 큐에 셀이 왜있어?"

여기서 예를 하나 볼까요?
만약, 테이블에 데이터가 1000개라면 어떨까요? 이건 적을 수 있으니
10만개라면요? 100만개라면요?

셀이 1000개, 10만개, 100만개가 따로따로 죄다 생겨버릴거에요.
그럼 메모리 자원을 셀로만 엄청 잡아먹을수 있어요.
그래서 셀을 재사용하자 라는 개념이 생겼어요.

직접 보는게 최고! 먼저 예제를 볼게요.

테이블 뷰를 하나 만들고 다음 화면처럼 만들어보세요.
![1.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/1.png)
![2.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/2.png)

구분을 할 수 있도록 특정 행만 배경화면을 파란색으로 바꿔줘볼까요?
![3.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/3.png)
![4.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/4.png)


4번쨰 행인 D에 파란색 배경이 생겼어요.
조건에 row가 3인것으로 했으니 로우는 0부터 시작이네요.

테이블을 살살 밑으로 내려볼게요.
![5.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/5.png)
![6.png](https://MinominoDomino.github.io/assets/img/ios/dequeueReusableCell/6.png)

!!!!아니!!!!????
분명 row가 3일때만 배경색을 파란색으로 하라고 했는데 다른 행에도 파란색이 들어가 있네요??
왜이런가요?? 이게 바로 ReusableCell이기 때문입니다.

## dequeueReusableCell 원리
앞에 보이는 테이블 뷰의 셀이 A ~ L까지 한 화면에 보였죠??
그리고 밑으로 내리면서 위에서 하나씩 셀이 사라졌어요.
그리고 밑에는 새로운 셀이 하나씩 나타나죠.

이게 셀이 사라지고 새로 생기는게 아닙니다.
dequeueReusableCell()로 만들어 진 셀은 해당 셀이 화면에서 안보이게 되면 큐에 셀을 넣고 
화면에 셀이 필요 할때 큐에서 뺴가서 셀을 표시합니다.
이렇게 셀을 게속 재사용하는거죠.

그래서 재사용가능한 셀을 큐에서 빼간다는 내용입니다.

정리해서,
테이블뷰의 각 셀은 화면에서 사라지면 큐에 들어가고 새로운 셀을 표시 할때는 큐에서 셀을 가져와서 표현한다.
이렇게 수많은 셀을 게속 만들지 않고 필요한 셀을 재사용하여 메모리 자원을 아낄수 있다.

그럼, 위에서 4번째 행이 아닌 곳에서 배경이 파란색으로 바뀌던 이유까지 설명이 되면서 셀이 재사용된다는 것을 증명할 수 있죠!?

## 참고
[애플 개발 문서 - dequeueReusableCell](https://developer.apple.com/documentation/uikit/uitableview/1614878-dequeuereusablecell)



















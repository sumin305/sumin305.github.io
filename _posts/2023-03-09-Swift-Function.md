---

layout: post
title : "[Swift] 함수"
categories : Swift
tags : 
    - Swift
    - Function
---
## 함수
앨런 부트캠프를 수강하면서 swift문법에서 평소에 쓰던 함수에 대해 메모리 구조 혹은 @discardableResult과 같은 새로운 개념도 알게 되었고, 헷갈렸던 부분도 더욱 구체적으로 알게되었다   
배운 내용들을 정리하고 예제도 만들어보고 내 생각도 덧붙이면서 지식을 공유하고자 한다   
<br>
함수는 특정한 기능을 하는 코드의 모음이고, 입력과 출력이 존재할 수 있다    
함수는 코드의 반복되는 부분을 줄이고 기능에 따라 분류해놓기 위해 사용된다    
예를 들어 a와 b에 다양한 수를 넣어 a를 b만큼 출력하는 알고리즘을 작성하려고 할 때,
```swift
var a = 2
var b = 3
for i in 1...b {
    print(a)
}

a = 6
b = 4
for i in 1...b {
    print(a)
}

a = 13
b = 33
for i in 1...b {
    print(a)
}
```   
위와 같이 한 번의 알고리즘을 수행할 때 for문을 작성해서 수행하여야 한다   
함수를 이용해보자   

```swift
func repeatNum(a: Int, b: Int) -> Void {
    for i in 1...b {
        print(a)
    }
}

repeatNum(a: 2, b: 3)
repeatNum(a: 6, b: 4)
repeatNum(a: 13, b: 33)
```   
이렇게 함수를 선언하면 코드를 간결화하고 함수 이름에 어떠한 기능을 하는지 명시적으로 나타낼 수 있다   
또 앞으로 어떠한 수를 반복해서 출력할 경우 이 함수를 가져와 사용할 수 있으므로 굉장히 편리하다   
이것이 함수를 사용하는 이유이다   


### 정의와 실행
함수는 크게 정의 단계와 실행 단계로 나뉘어진다   
```swift
// 함수 정의 
func repeatNum(a: Int, b: Int) -> Void {
    for i in 1...b {
        print(a)
    }
}

// 함수 실행
repeatNum(a: 2, b: 3)
repeatNum(a: 6, b: 4)
repeatNum(a: 13, b: 33)
```  
-  함수는 어떤 동작을 수행할 때, 다룰 값들을 받아올 수도 있고, 수행한 결과를 반환해줄수도 있다   
-  이 받아온 값들은 input, 반환한 값들은 output이라고 한다
- 위에서 살펴본 이 예제 코드를 보면 (a: Int, b: Int)의 값들은 input을 나타낸 것이고 Void는 output의 자료형을 나타낸 것이다      

<br>

> 함수 선언 : func 함수이름 ( parameter ) -> return type { 함수 내용 }      
`parameter`는 함수를 실행할 때 받아온 값들을 의미한다  
코드를 한 줄씩 읽을 때 정의에서는 아무런 일도 발생하지 않는다   
<br>
> 함수 호출 : `함수이름` + `소괄호()`    
이 때 input으로 넘기는 값들을 소괄호 안에 넣어준다    
이렇게 함수로 넘겨주는 값들을 `argument`라고 한다     


#### parameter
#### inout parameter
#### argument
#### 표기법 
* * *   
### scope

* * *   
### 제어전송문

* * *   
### 메모리 구조

* * *   
### 오버로딩

* * *   
### 주의할 점

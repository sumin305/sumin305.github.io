---
layout: post
title:  "[Swift] Swift 기본 문법 - 변수 & 상수와 반복문"

categories : Swift
  
tags:
  - Swift
  - iOS
---

## Swift문법
Swift 언어 공부를 시작하며 여태까지 배운 언어들과의 차이점 위주로 정리하려 한다.   

### 변수와 상수

#### 선언
> Swift는 함수형 프로그래밍의 패러다임을 채용한 언어임 -> **불변 객체**를 굉장히 중요시 한다.   

대부분 상수나 변수 선언 시에 타입을 꼭 명시해준다.   
```swift
//변수의 선언
var integer : Int = 5

//상수의 선언
let greet : String = "Hello" 
```

리터럴 문법에도 동일하다.   


```swift
//Array선언
var integers : Array<Int> = Array<Int>()

//Dictionary선언
var anyDic : Dictionary<String, Any> = [String : Any]()

//Set선언
var integerSet : Set<Int> = Set<Int>()
```
#### 기본 데이터 타입
> Swift는 데이터 타입에 엄격한 언어이다.    
    
    

1. Bool : true와 false값만 가진다.
2. Int, UInt 
  - Int : 64비트 정수형
  - UInt : 64비트 양의 정수형
3. Float , Double
  - Float : 32비트 부동소수형
  - Double : 64비트 부동소수형

4. Character, String
  - Character : 문자 타입, 큰따옴표 사용
  - String : 문자열 타입, 큰따옴표 사용

<-- 여기서부터는 Swift에서 처음 본 자료형이다 -->

5. Any : Swift는의 모든 타입을 지칭하는 키워드   


```swift
var anything : Any = 100
anything = "String도 넣을 수 있음"
anything = 1234.67

let double = anything //컴파일 에러
명시적인 데이터 타입 변환이 필요함
```

6. AnyObject : 모든 클래스 타입을 지칭하는 프로토콜

```swift
class SomeClass{}
var someAnyObject : AnyObject = SomeClass()

//AnyObject는 클래스의 인스턴스만 수용 가능함
someAnyObject = 123.5 //컴파일 에러
```

7. nil : Swift에서 '없음'을 의미하는 키워드이다.    
  0이 아니라 정말 값이 없음을 의미한다.   
 - NULL과의 차이점
   - NULL : 포인터가 기리키는 객체가 존재하지 않음
   - nil : 특정 타입에 대한 값의 부재를 나타냄

```swift 
let str1 = "hello486"
let str2 = "1234"

print(Int(str1) == nil ? 0 : 1 ) //0 출력
print(Int(str2) == nil ? 0 : 1 ) //1 출력
```

- str1를 Int형으로 변환할 때 Int형으로 변환할 수 없는 문자열 부분이 있음으로 nil을 반환한다.    


### 반복문
- for문과 while문은 파이썬과 매우 유사하다.   

#### for문
```swift
for i in 1...4{
    print("\(i)")
}
```
<p align="left">
  <img width="25" alt="실행결과1" src="https://user-images.githubusercontent.com/110437548/210287119-d07468bd-0225-46be-a46a-b687950ddc8c.png">
</p>

- stride를 사용하여 범위 지정도 가능하다
```swift
for i in stride(from:10, to:0, by:-2){
    print("\(i)")
}
```
<p align="left">
  <img width="34" alt="실행결과2" src="https://user-images.githubusercontent.com/110437548/210287068-2c133e86-0998-495a-a7b9-84bf83dcafab.png">
</p>
   
#### while문
```swift
var i : Int = 0
while true{
    print("\(i)")
    i+=1
    //4까지만 출력
    if i==5{
        break
    }
}
```
<p align="left">
  <img width="25" alt="실행결과3" src="https://user-images.githubusercontent.com/110437548/210287096-360c5d71-20eb-458d-aa50-4dd666394cba.png">
</p>

#### repeat - while문
```swift
i=100
//repeat문 사용
repeat {
    print("\(i)")
    i+=1
}while i<110
```
<p align="left">
  <img width="29" alt="실행결과4" src="https://user-images.githubusercontent.com/110437548/210287108-b6c0235c-70a6-4b08-a1c7-969e9f8972ed.png">
</p>
   
### 마무리
 Swift 문법을 배우면서 점점 느끼는 바로는 정말 매력적인 언어라고 생각한다.    
 이렇게 생각한 이유는 다음과 같다
 > java와 python 과의 장점을 합친 빠르고 정확한 언어    

라고 생각한다.    
앞으로 이렇게 문법을 더 정리해 Swift를 정복해보겠다.

---

layout: post
title : "[Swift] 클래스와 구조체"
categories : Swift
tags : 
    - Swift
    - class
    - struct
    
---

## 클래스와 구조체

[Swift 공식문서 한국판](https://jusung.gitbook.io/the-swift-language-guide/language-guide/09-classes-and-structures) 통해 공부한 내용을 정리해야겠다   
항상 프로그래밍을 할 때 클래스로 구현할 지 구조체로 구현할 지 고민을 해서 한번 정리해야겠다고 생각했다   
객체지향프로그래밍의 기본이 되는 객체를 특성과 행동으로 분류하는 클래스와 구조체는 서로 특징이 다르고 사용하는 경우도 다르다  
어떤 경우에 클래스와 구조체를 사용해야 할 지 차이점과 같이 함께 알아보자

### 공통점
#### 1. 기능
- 값을 정의하기 위한 프로퍼티 정의
- 기능을 제공하기 위한 메소드 정의 
- subscript문법을 이용해 특정 값을 정의할 수 있는 `subscript` 정의
- 초기 상태를 설정할 수 있는 `initializer` 정의
- 기본 구현에서 기능 확장 가능
- 특정한 종류의 표준 기능을 제공하기 위한 프로토콜 순응

#### 2. 선언 문법
- 키워드를 이름 앞에 적어서 선언
- `UpperCamelCase`로 선언(ex: String, Int, Double, ..) 
-> 프로퍼티와 메소드는 `lowerCamelCase` 로 선언

```swift
struct Wheel{
    var radius = 0
    var shape = "Circle"
}

class Car{
    var name = "내 차"
    var wheel = Wheel()
    var maxSpeed = 0.0
    var isOld = false
}
```
#### 3. 인스턴스 생성
- 이름 뒤에 빈 괄호를 적으면 각각의 인스턴스 생성
```swift
var myCar = Car()
let myWheel = Wheel()
```

#### 4. 프로퍼티 접근
- 점(dot) 문법을 통해 클래스/구조체 인스턴스의 프로퍼티에 접근할 수 있다
```swift
print("\(myCar.name)는 새 차일까요? -> \(myCar.isOld)")
//출력 : 내 차는 새 차일까요? -> false
```
- 하위레벨 프로퍼티도 점 문법을 통해 접근하고 값을 할당할 수 있다 
```swift
myCar.wheel.radius = 30
print("\(myCar.name)의 바퀴 반지름은 \(myCar.wheel.radius)이다")
//출력 : 내 차의 바퀴 반지름은 30이다
```

#### 5. 초기화
- 선언과 동시에 기본값을 갖거나
```swift
struct Wheel{
    var radius = 0
    var shape = "Circle"
}
```
- 옵셔널 타입으로 선언하거나
```swift
struct Wheel{
    var radius : Int?
    var shape : String?
}
```
- 혹은 init함수 직접 작성해야 한다 
```swift
class Car{
    var name = "내 차"
    var wheel = Wheel()
    var maxSpeed : Double
    var isOld = false

    init(maxSpeed: Double){
        self.maxSpeed = 100.0
    }
}
```
<br>

* * *   

### 클래스
#### 클래스에서만 가능한 기능
- 상속 : 클래스의 여러 속성을 다른 클래스에게 물려줌
- 타입 캐스팅 : 런타임에 클래스 인스턴스 타입을 확인함
- 소멸자 : 할당된 자원을 해제 시킴
- 참조 카운트 : 클래스 인스턴스에 하나 이상의 참조가 가능하고 그 참조 개수를 카운트 할 수 있다 

#### 초기화

#### 참조 타입   


* * *  
### 구조체
#### 초기화
#### 값 타입
> 구조체에서만 Memberwise Initializers를 제공    
> 구조체가 자동으로 제공하는 생성자로, 파라미터를 통해 모든 프로퍼티의 초기화를 가능하게 한다    
> 프로퍼티의 초기화를 따로 지정하지 않을 경우, 초기화되지 않은 프로퍼티를 초기화할 수 있는 init함수를 자동으로 제공한다 !!
```swift
let myWheel = Wheel.init(radius: 34, shape: "rectangle")
let myWheel = Wheel(radius: 34, shape: "rectangle)
//위의 선언식 둘은 동일하다
```
결국 하나의 프로퍼티라도 기본 값을 갖지 않고 옵셔널 변수가 아니라면 init함수를 작성해주어야 하는데, 구조체는 init함수를 작성하지 않아도 오류가 발생하지 않는다

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
> 함수는 특정한 기능을 하는 코드의 모음이고, 입력과 출력이 존재할 수 있다      


- 함수는 코드의 반복되는 부분을 줄이고 기능에 따라 분류해놓기 위해 사용된다    
- 예를 들어 a와 b에 다양한 수를 넣어 a를 b만큼 출력하는 알고리즘을 작성하려고 할 때,   

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
- 함수는 크게 정의 단계와 실행 단계로 나뉘어진다      

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
- 함수는 어떤 동작을 수행할 때, 다룰 값들을 받아올 수도 있고, 수행한 결과를 반환해줄수도 있다   
- 이 받아온 값들은 input, 반환한 값들은 output이라고 한다
- 위에서 살펴본 이 예제 코드를 보면 (a: Int, b: Int)의 값들은 input을 나타낸 것이고 Void는 output의 자료형을 나타낸 것이다      

<br>

> 함수 정의 : func 함수이름 ( `argument label` `parameter name` : `parameter type` ) -> `return type` { 함수 내용 }      

- `parameter`는 함수를 실행할 때 받아온 값들을 의미한다  
- 코드를 한 줄씩 읽을 때 정의에서는 아무런 일도 발생하지 않는다   
  - return 값이 있을 경우 : 함수의 호출은 어떠한 값을 나타낸다 
  - return 값이 없을 경우 : return type은 생략하거나 (), 혹은 Void로 나타낼 수 있음
<br>   

> 함수 호출 : `함수이름` + `소괄호(argument label: argument)`    

- 이 때 input으로 넘기는 값들을 소괄호 안에 넣어준다    
- 이렇게 함수로 넘겨주는 값들을 `argument`라고 한다     
- return type이 있는 함수를 호출할 경우, 그 자체로 표현식이 된다
```swift
func myName() -> String{
  return "soom"
}
//함수 호출이 표현식이 됨
var name = myName()
```   


#### 1. parameter
- parameter는 위에서 말한 것처럼 함수에서 입력값으로 받아와서 함수 내부에서 사용되는 변수를 의미한다    
- parameter는 받아온 값을 복사하여 let 값으로 사용하게 되므로, 함수 내부에서 수정할 수 없다     
```swift
var a = 1
var b = 2
func addAB(_ a :Int, _ b: Int) -> Int {
  var temp = a
  a = b     //컴파일 에러 'a'는 상수임
  b = temp  //컴파일 에러 'b'는 상수임
}
addAB(a,b)
```

##### 기본 값 설정
- 함수 정의시 기본 값을 미리 설정해놓으면, 나중에 호출할 때 해당 정의된 기본 값의 argument를 생략하면 자동으로 기본 값으로 실행된다 
```swift
func repeatNum(repeatNumber a: Int, count b: Int = 5) -> Void {
    for i in 1...b {
        print(a)
    }
}

repeatNum(repeatNumber: 2, count: 3) //2 3번 출력
repeatNum(repeatNumber: 5, count: 4)//5 4번 출력
repeatNum(repeatNumber: 3)//3 5번 출력 -> 기본 값 설정된 parameter 이용
```


##### 가변 파라미터
- 개수가 정해지지 않은 여러 개의 parameter일 경우 사용한다   
- 하나의 parameter로 2개 이상의 argument 전달할 수 있다     
- 개별 함수마다 하나씩만 선언 가능하다     
```swift
func sum(nums: Int..., num: Int) -> Int {
    var temp = num
    for n in nums {
        temp += n
    }
    return temp
}

print(sum(nums: 1,2,3,4,5, num: 5)) //20출력
```   
   
- print도 Variadic Parameters의 예시이다.    
```swift
func print(_ items: Any..., separator: String = " ", terminator: String = "\n")
```   
- 출력할 값들을 Any 타입의 가변 파라미터로 입력을 받고, separator와 terminator에 기본 parameter값이 설정되어 있다      


#### 2. argument
argument는 함수 호출 시 parameter type에 맞게 데이터를 전달하는 것을 의미한다   
argument label을 작성해주면 함수가 원하는 것을 길게 작성하여 명시해줄 수 있다   
```swift
func repeatNum(repeatNumber a: Int, count b: Int) -> Void {
    for i in 1...b {
        print(a)
    }
}

repeatNum(repeatNumber: 13, count: 33)
```

- 함수를 정의할 때, argument label을 작성하지 않으면 함수 호출 시 반드시 argument label을 명시해주어야 한다     


```swift
func repeatNum(a: Int, b: Int) -> Void {
    for i in 1...b {
        print(a)
    }
}

repeatNum(2, 3) // 컴파일 에러 **Missing argument labels 'a:b:' in call**
```    


- 함수를 정의할 때, argument label을 \_(와일드 카드 패턴)로 작성하면 함수 호출 시 argument label을 생략할 수 있다      


```swift 
func repeatNum(_ a: Int, _ b: Int) -> Void {
    for i in 1...b {
        print(a)
    }
}

repeatNum(2, 3)
```   

#### 3. inout parameter
- parameter type앞에 inout 키워드를 추가해주면 parameter의 복사본이 전달되지 않고 원본 데이터의 메모리 주소값이 전달된다    
- 즉 함수 내부에서 이 값을 변경하면 외부에서 받아온 변수의 값도 변한다    
- 이러한 외부 변수들을 받아올 때 argument 변수 앞에 `&`값을 붙여준다   


```swift
var a = 1
var b = 2
func addAB(a :inout Int, b: inout Int) -> Int {
  var temp = a
  a = b
  b = temp
  return (a+b)
}
print(addAB(a: &a,b: &b))
```   

#### 표기법 
- 개발자 문서를 읽을 때 알아야하는 함수 표기법이다   
- 함수이름과 parameter name까지만 적어준다    
- `,`도 생략해준다
  - NumberPrint(n:) 
  - repeatNum(a:b:) 
  - addAB(\_:_:)
- 함수의 타입 표기 
  - () -> ()
  - () -> Void
  - (String) -> String
  - (Int, Int) -> Int   
   

* * *     
### scope
- 변수는 범위 내에서 꼭 선언되어야만 접근이 가능하다
- 상위 스코프 변수에는 접근이 가능하지만 (하위 스코프에서는 상위 스코프 변수가 전역변수처럼 작용)   


```swift
func add(a: Int, b: Int) {
    var temp = a
    func minus(a: Int, b: Int) {
        temp = 5
    }
}
```   


- 하위 스코프 변수에는 접근이 불가능하다   


```swift
func add(a: Int, b: Int) {
    temp = a // 컴파일 에러 **Cannot find 'temp' in scope**
    func minus(a: Int, b: Int) {
        var temp = 5
    }
}
```   


- 다른 스코프에서 이름 중복이 가능하다   


```swift
func add(a: Int, b: Int) {
     var temp = a
     temp += 5
    func minus(a: Int, b: Int) {
        var temp = 5
        temp += 5
    }
}
```   


* * *    

### 제어전송문
- break
  - switch문 : 해당 케이스에서 실행하는 문장이 없을 경우, 빈 칸으로 두지 않고 반드시 `break`를 명시해준다
  - 반복문 : 가장 인접한 반복문을 break을 만나는 순간 바로 빠져나온다    
- fallthrough 
  - switch문 : switch문에서 fallthrough를 만나면 조건 상관없이 무조건 다음 case도 실행한다
- continue 
  - 반복문에서 continue를 만나면 그 밑의 코드를 실행하지 않고 바로 반복문의 다음 cycle로 넘어간다
- return
  - 반복문에서 함수의 실행을 중지함 (return type 있을 경우 결과 반환)
- throw
  - 에러가 발생가능한 함수에서 throw 키워드 다음에 정의된 에러의 타입을 반환하면서 함수를 빠져나온다        
     
* * *    

### 메모리 구조
- 함수 실행 시 메모리의 stack 영역에 메모리 공간을 만들고 함수가 실행이 완료될 경우 stack frame이 사라진다!
- 재귀 함수를 사용할 경우, 종료 조건을 만날 때까지 stack frame에 메모리 쌓이고 하나씩 처리된다
- 즉, 종료 조건을 명시하지 않으면 stack over flow가 발생!   


* * *     


### 오버로딩
swift에서는 같은 이름의 함수에 parameter를 다양하게 선언하는 것이 가능하다    
함수이름, parameter의 수와 타입, argument label, return type을 모두 포함하여 함수를 식별함!
```swift


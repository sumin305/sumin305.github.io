---

layout: post
title : "[Swift] 변수와 상수"
categories : Swift
tags : 
    - Swift
    - Variables
    - Constants
---

## 변수와 상수
- 메모리에 값(데이터)을 저장한다
- 선언한 변수의 이름은 식별자이므로 한 영역에 하나의 이름만 사용한다
- 소문자로 시작, 중간에 대문자 혹은 숫자가 들어가도 된다   

### 변수
```swift
var age = 15
//age라는 변수를 선언하고 그 변수에 15라는 값을 저장한다

var age2 : Int
age2 = 15
//변수를 먼저 선언하여 메모리 공간을 생성하고 값을 저장할 수도 있다  

var a = 1, b = 2, c = 3
//변수를 여러 개 한꺼번에 선언 가능하다

var yourName = age
//새로운 변수를 선언하고 변수 값을 복사한다
```

- String Interpolation (스트링 인터폴레이션) -> 문자열 보간법
- 변수를 문자열처럼 출력할 경우 사용
- `"\(삽입할 변수)"`

```swift
var name = "Hongildong"
var age = 55

print("my name is \(name), and my age is \(age)")
//출력 "my name is Hongildong, and my age is 55"
```

### 상수

- 변하지 않는 데이터
```swift
let name2 = "철수"
name2 = "영희"
//컴파일 에러
```   

## 데이터 타입
- 데이터 타입이 존재하는 이유
  : 데이터를 메모리에 얼마 만큼의 크기와 형태로 저장할 것인지 약속하는 것 -> 메모리 효율적으로 사용
- 타입에 관한 키워드는 모두 대문자로 시작한다
### 데이터 타입의 종류
#### Int
- 정수형 데이터 타입
-`-5, -100, 0, 100` 등..
- `9223372036854775808 ~ 9223372036854775807`

#### Float
- 실수 타입
- 부동소수 타입 (flating-point)
- `-3.455, -9.5433, 0, 3.567, 8.987`
- 소수점 표현 가능
- 소수점 포함 전체 `6`자리 정보를 저장
- `4 byte = 32 bit`
- 더 작은 소수들을 담을 수 있다

#### Double
- 실수 타입
- 소수점 표현 가능
- 소수점 포함 전체 `15`자리 정보를 저장
- `8 byte = 64 bit` (Float의 두배)

#### Character
- 하나의 문자 저장
- 공백 문자를 담을 수 없다
- `"A", "b", "글", " "` 등..
#### String
- 문자열을 저장
- `"Abcd", "b", "글자열", ""` 등..

#### Bool
- true 혹은 false값 저장

#### 기타
- UInt, UInt64, Int32..

### 타입 주석(Type Annotation)
- 변수를 선언하면서 타입도 명확하게 지정해주는 방식
```swift

var num : Int = 5
//정식 문법 -> 데이터 타입을 기입해준다

var num2 : Int // 변수를 선언, 타입 선언 *메모리 공간 생성
num2 = 100 // 값을 저장 (초기화)
```

### 타입 추론(Type Inference)
- 타입을 지정하지 않아도 컴파일러가 타입을 유추하여 알아서 알맞는 타입으로 저장하는 방식

```swift
var one = 1
var two = "2"
var three = 3.0
print(type(of: one))
//Int 출력
```
### 타입 안정성(Type Safety)
- 스위프트는 데이터 타입을 엄격하게 분리하여 사용한다
> 다른 데이터 타입끼리 계산할 수 없다

```swift
let num = 9
let double = 12.4

num = 33.6
//메모리 공간의 크기가 달라 호환되지 않음 -> 컴파일 에러

print(num + double)
//컴파일 에러
```

### 타입(형) 변환(Type Conversion)
- `바꿀 타입(변환하려는 변수)`
- 데이터 타입 변환에 실패하거나 데이터가 유실될 수 있다는 점 주의
> 타입 변환에 실패했을 경우에는 nil값 return
```swift
let str = "123"
let num = Int(str)

print(num)
//Optional(123) 출력

let str1 = "123.4"
let num1 = Int(str1)
print(num1)
//nil 출력

let double = 123.45
let num2 = Int(double)
print(num2)
//123출력
//소수점 버림

```
## 타입애일리어스(Type Alias)
- 기존에 선언되어있는 타입이나 내가 만든 타입에서 새로운 별칭을 붙여 코드 가독성을 높이는 문법
- 치환과 유사
- 어떤 형태든 치환이 가능
- 코드가 짧아져 문법이 간단해짐

```swift
typealias Name = String
let name : Name = "leesumin"
//Name 타입은 String타입과 동일

typealias Something = (Int) -> String

func someFunction(completionHandler : (Int) -> String) {
    //~~
}

func someFunction2(completionHandler : Something) {
    //~~
}
```   

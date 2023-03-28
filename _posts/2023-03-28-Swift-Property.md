---
layout: post
title : "[Swift] 프로퍼티"
categories : Swift
tags : 
    - Swift
    - Property
    - Value Property
    - Reference Property
---    

## 속성과 메서드

구조체와 클래스에는 속성(프로퍼티)와 메서드가 존재한다

속성은 두 타입에서 동일! 메서드에서는 약간의 차이가 있음

<aside>
> 💡 속성 : 구조체/클래스의 변수

</aside>

- 저장 속성
    - 지연 속성
- 계산 속성
- 타입 속성
    - 타입 저장 속성
    - 타입 계산 속성
- 속성 감시자 등

<aside>
💡 메서드 : 구조체/클래스의 함수

</aside>

- 인스턴스 메서드
- 타입 메서드
- 서브스크립트
- 생성자
- 소멸자

### 1. 저장 속성(Stored Properties)

- 클래스/구조체에서 찍어낸 각 인스턴스가 가지는 고유의 데이터
- var/let으로 선언가능
- `객체 초기화를 마치는 경우, 저장 속성은 반드시 값을 가져아함` (기본값 & 생성자 & 옵셔널 타입)
- 열거형은 따로 메모리 공간이 필요한 저장 속성은 선언 불가능
- 저장 속성은 각 속성 자체가 고유의 메모리 공간을 갖는다

```swift
struct Person {

	//저장 속성!
	var name: String
	var age: Int

	func sit() {
		print("앉는다")
	}
```

### 1-1. 지연 저장 속성(Lazy Stored properties)

- 게으른 저장 속성으로 처음부터 값을 초기화하지 않고 지연시킴
- 해당 속성이 반드시 처음부터 초기화가 필요하지 않은 경우 (이미지 등 메모리 공간을 많이 차지하는 속성에서 주로 사용)
- 값에 대한 접근이 있어야 메모리 공간을 생성하여 초기화시킨다
- lazy var로만 선언 가능(lazy let 불가능)
- 초기화를 지연시키기 때문에 반드시 기본값이 필요(기본값에  함수 실행문, 계산식, 클로저 실행문 들어갈 수 있음)

- 지연 저장 속성 사용하는 이유?
    - 메모리 공간 낭비와 불필요한 성능저하를 줄인다 → 메모리 공간을 많이 차지하는 이미지와 같은 속성들 다룰
    - 다른 저장 속성을 이용하여 선언해야 할 경우

```swift
struct Person {
    var name: String
    lazy var age: Int = 20

		//컴파일 에러 
    var nameCount: Int = name.count

		//다른 저장 속성을 이용하여 선언해야 할 경우 lazy 사용
    lazy var nameCount: Int = {
				return name.count
		}()

    init(name: String) {
        self.name = name
    }
}
var p1 = Person(name: "soom")

//해당 변수에 접근하는 시점에서 초기화된다! (메모리 공간 생성)
//접근 이후에는 계속 메모리 공간에 남아있음
var age = p1.age
p1.age = 22

struct View {
	var count: Int
	//해당 속성이 메모리를 많이 차지할 경우 늦게 메모리에 올리기 위해 사용한다
	lazy var view = UIImageView()

	//다른 속성을 이용하여할 때
	lazy var b: Int = {
		return a * 10
	}()
}
```

### 2. 계산 속성(Computed Properties)

- 실질적으로 메서드 역할을 하는 속성임
- 다른 저장 속성에 의존한 결과로 나옴
- `인스턴스에 메모리 공간이 할당되지 않음` → 실질적으로 메서드!
- var로만 선언 가능 & 자료형까지 선언
- get 블록 : 해당 인스턴스의 값을 얻는다
    - 필수구현
    - get 블록만 선언시 읽기 전용 (Read-Only)
    - 읽기 전용의 프로퍼티 선언시 get블록 생략 가능
- set 블록 : 밖에서 해당 인스턴스에 접근하여 값을 세팅한다
    - 기본 파라미터 newValue가 제공됨 (파라미터 이름 설정도 가능)

```swift
class Person {
    var name: String = "사람"
    var height: Double = 160.0
    var weight: Double = 60.0
    
		//다른 속성의 저장 속상을 계산해서 나오는 방식의 메서드를 계산 속성으로 바꿀 수 있다!
    func calculateBMI() -> Double {
        let bmi = weight / (height * height) * 10000
        return bmi
    }
		
		var bmi: Double {
        get {                                               
            let result = weight / (height * height) * 10000
            return result
        }
    }
		//get 블록 생략 가능
		var bmi: Double {
		      let bmi = weight / (height * height) * 10000
		      return bmi
		  }
		
		var bmi: Double {
		        get {       
		            let bmi = weight / (height * height) * 10000
		            return bmi
		        }
		        set(bmi) {  
		            weight = bmi * height * height / 10000
		        }
		}
}
```

- 메서드 대신 계산 속성 사용시 장점
    - 관련이 있는 두가지 메서드(함수)를 한번에 구현할 수 있다
    - 외부에서 보기에 속성이름을 설정가능하므로 보다 명확해보인다
    - 계산 속성은 메서드를 개발자들이 보다 읽기 쉽고, 명확하게 쓸 수 있는 형태인 속성으로 변환해놓은 것이다.

### 2-1)  계산 프로퍼티의 메모리 동작 구조 (클래스 기준)

메모리 공간을 가지지 않는다

<aside>
> 1. 코드에서 계산 프로퍼티 실행문을 만나면 (p1.bmi)
  2. 힙에 있는 인스턴스로 가고 인스턴스가 가리키고 있는 데이터 영역의 클래스 선언문을 본다
  3. 클래스 선언문의 get블록, set블록 계산 프로퍼티의 메모리 주소를 찾아가서 그 코드를 실행한다
  4. 함수의 실행이므로 스택에 쌓인다
  5. 다루고 있는 인스턴스의 메모리 주소가 get, set 함수의 파라미터 (그 내부 프로퍼티 사용 가능)로 넘겨진다

</aside>
(항상 메서드의 실행은 스택 프레임에서 실행된다)

- 구조체의 경우 실제로는 함수 메모리 주소 직접 접근

### 3. 계산속성 vs 지연저장속성

지연저장속성은 접근 이후 메모리에 계속 남아있기 때문에 메모리가 큰 프로퍼티 다룰 때 사용한다. 

지연저장속성은 한번 접근해서 그 값을 초기화하면 그 값이 계속 유지된다

반면에, 계산속성은 접근 시마다 연산을 하기 때문에 계속 값이 변한다

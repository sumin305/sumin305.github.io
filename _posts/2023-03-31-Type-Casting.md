---
layout: post
title : "[Swift] 타입 캐스팅"
categories : Swift
tags : 
    - Swift
    - Type Casting
    - is/as
    - Down Casting
    - Up Casting 
    - 다형성
    - Any
    - AnyObject
---    

## 타입 캐스팅

### is 연산자(type check operator)

- 인스턴스의 타입을 검사함
- `인스턴스 is 타입` 형태로 사용 (이항 연산자)
- 상속은 `저장속성 확장 개념`이기 때문에 하위 클래스가 상위 클래스의 타입일 수는 있지만, 상위 클래스가 하위 클래스 타입일 수는 없다.

(메모리 관점에서 하위 클래스의 인스턴스는 상위 클래스의 속성을 가지고 있지만, 상위 클래스는 하위 클래스의 속성을 가지고 있지 않다.)

Person 인스턴스 is Undergraduate(false)

Undergraduate인스턴스 is Person (true)


```swift
class Person {
    var id = 0
    var name = "데이지"
    var email = "abc@naver.com"
}

// Person 클래스 상속
class Student: Person {
    var studentId = 1
}

// Student 클래스 상속
class Undergraduate: Student {
    var major = "정보통신공학과"
}

// is 연산자를 활용한 타입 검사

var person = Person()
var student = Student()
var undergraduate = Undergraduate()

person is Person // true
person is Student  // false
person is Undergraduate // false

student is Person // true
student is Student // true
student is Undergraduate // false

undergraduate is Person // true
undergraduate is Student // true
undergraduate is Undergraduate // true

let person1 = Person()
let student1 = Student()
let undergraduate1 = Undergraduate()
let people = [person, person1, student, student1, undergraduate, undergraduate1]
]

for one in people {
    if one is Student {
        studentNumber += 1
    }
}

// student, student1, undergraduate, undergraduate1 인스턴스가 Student 타입임
print(studentNumber)
```

### as 연산자(type cast operator)

- 클래스 계층 상의 타입 변환
- `Upcasting`
    - `인스턴스 as 타입`  형태
    - 하위 클래스 인스턴스를 상위 클래스 타입으로 인식
    - 항상 성공한다

```swift
let student: Student = Student()
student.studentId
// 업캐스팅, 컴파일러가 에러가 발생할 가능성이 없다는 것을 안다
let person = student as Person
person.name
person.email
person.studentId // 컴파일 에러 Value of type 'Person' has no member 'studentId'

// 위 코드와 같은 업캐스팅 예제
let student1: Person = Student()
```

- `Downcasting`
    - `인스턴스 as?/! 타입` 형태
    - 상위 클래스 인스턴스를 하위 클래스 타입으로 인식
    - as? 연산자일 경우, 참이면 반환값은 Optional / 실패 시 nil 반환
    - as! 연산자일 경우, 참이면 반환값은 Optional 타입의 값을 강제 언래핑한 타입 / 실패 시 런타임 오류 !!
- 타입 캐스팅은 인스턴스 사용 시 어떤 타입으로 사용할 지 메모리 구조에 대한 힌트를 변경하는 것!
    
    
    ```swift
    // 힙 메모리에 이미 Undergraduate() 데이터를 생성하여 5개의 저장 속성을 가지지만
    // 변수에 정의된 타입은 Person 이므로, 컴파일러는 Person 타입이 들어있다고 인식
    // 업캐스팅
    let person: Person = Undergraduate()
    // let person: Undergraduate = Person() (x) 참고로 하위 클래스의 변수에 상위 클래스를 담을 수는 없음
    
    person.id
    person.email
    // 컴파일 에러 Value of type 'Person' has no member 'major'
    person.major
    
    person is Undergraduate // true
    person is Person // true
    
    let ppp = person as? Undergraduate  // ppp -> Undergraduate? 타입
    
    if let newPerson = person as? Undergraduate {  // if let 바인딩과 함께 사용
    		
        newPerson.major
        print(newPerson.major)
    }
    
    // 실제로 인스턴스의 접근 범위를 늘려주는 것 뿐임
    
    let person3: Undergraduate = person as! Undergraduate
    person3.major
    
    let people = [person1, person2, student1, student2, undergraduate1, undergraduate2]
    var studentArray: [Student] = []
    for someone in people {
        if someone is Student {
            studentArray.append(someone)
        }
    }
    
    var studentArray: [Student] = []
    for someOne in people {
        if someOne is Student {
            if let one = someOne as? Student {
                studentArray.append(one)
            }
        }
    }
    // 컴파일러는 people 배열을 인스턴스의 공통 분모인 [Person] 타입으로 인식
    // is 연산자는 실제 메모리 내 데이터가 어떠한 타입이고, 어떠한 타입을 상속받았나 확인 시 사용
    // 배열 내 아이템들은 각자의 타입 데이터를 가지고 있지만, 컴파일러는 Person 타입으로 인식한다
    ```
    

    - `let person: Person = Undergraduate()` 의 person은 Person 타입으로 업캐스팅 되었지만 실제 데이터는 Undergraduate 타입이라 Undergraduate() 클래스를 가리키고 있기 때문에 study()나 party() 함수는 실행이 불가능하지만, walk() 함수를 실행할 경우, Undergraduate 클래스에서 재정의한 walk()-2 함수를 시행한다
        - (메서드 디스패치)
    
    - as 연산자의 활용
        - Bridging → 서로 호환되는 형식을 캐스팅해서 쉽게 사용하는 것
        - swift에서는 여전히 objective-C의 프레임워크를 많이 사용
        - 서로 완전히 상호 호환 가능 (업 다운 캐스팅 개념 x)
        - 강제 캐스팅할 필요가 없으므로 `as` 연산자를 활용한다
        
        ```swift
        let str: String = "HELLO"
        let nsStr = str as NSString
        ```
        

### 상속과 다형성

- 다형성이란
- 하나의 객체가 여러가지의 타입으로 표현될 수 있음
- 하나의 타입으로 여러 종류의 객체를 해석할 수 있음
- 클래스의 상속 + 프로토콜과 깊은 연관성

```swift
class Person {
    var id = 0
    var name = "이름"
    var email = "abc@gmail.com"
    
    func walk() {
        print("사람이 걷는다.")
    }
}

class Student: Person {
    var studentId = 1
   
    override func walk() {         // 재정의 메서드. walk() - 1
        print("학생이 걷는다.")
    }
    
    func study() {
        print("학생이 공부한다.")
    }
}

class Undergraduate: Student {
    var major = "전공"
    
    override func walk() {       // 재정의 메서드. walk() - 2
        print("대학생이 걷는다.")
    }
    
    override func study() {      // 재정의 메서드. study() - 1
        print("대학생이 공부한다.")
    }
    
    func party() {
        print("대학생이 파티를 한다.")
    }
}

let person1 = Person()
person1.walk() //사람이 걷는다. 출력

let student = Student()
student.walk() // 학생이 걷는다. 출력
student.study() // 학생이 공부한다. 출력

let student1: Person = Student()
student1.walk() // 학생이 걷는다. 출력  -> 다형성의 예 
student1.study() // 컴파일 에러 Value of type 'Person' has no member 'study'

let people: [Person] = [Person(), Student(), Undergraduate()]

// 반복문
for person in people {
    person.walk()
}

// 사람이 걷는다. 출력
// 학생이 걷는다. 출력
// 대학생이 걷는다. 출력
```

- 상속 관계에서 다형성은 메서드를 통해 발현된다.
    
    
- 업캐스팅된 타입 (Person) 형태이더라도 메서드 호출 시 실제 메모리에서 구현된 “재정의”된 메서드 (Walk() -1 )가 호출되어 실행된다.
- 타입의 저장 형태는 속성/메서드에 대한 접근 가능 범위를 나타내는 것이고, (Person)
- 다형성은 인스턴스에서 메모리의 실제 구현 내용에 대한 것임 (Student)
- 메서드는 재정의 가능하고, 메서드 테이블을 통해 동작한다 !

### **가상 메소드 테이블이란?**

가상 메소드 테이블은 동적 디스패치를 지원하기 위해 프로그래밍 언어에서 사용되는 메커니즘이다. 클래스가 가상 함수을 정의할 때마다, 대부분의 컴파일러들은 클래스에 숨겨진 멤버 변수를 추가하는데, 이것은 함수들에 대한 포인터들의 배열들을 가리킨다


### Any/ AnyObject 타입

1. Any
    - 기본 타입(Int, String, Bool 등..) 과 커스텀 타입(클래스, 구조체, 열거형, 함수, 클로저) 까지 포함하여 어떠한 타입의 인스턴스도 표현할 있는 타입
    - 옵셔널 타입도 포함
    - 단점 : 저장된 타입의 메모리 구조를 알 수 없기 때문에 항상 타입 캐스팅해서 사용해야 된다.
    - 장점 : 모든 타입을 다 담을 수 있는 배열 생성 가능!
    
    ```swift
    var some: Any = "Swift"
    // some.count -> 컴파일 에러 Value of type 'Any' has no member 'count'
    
    // String 타입으로 타입 캐스팅 후 문자열 메서드 사용 가능
    if let str = some as? String {
        print(str.count)
    }
    
    print(type(of: some)) // String 출력
    some = 10
    some = 3.2
    print(type(of: some)) // Double 출력
    
    let array: [Any] = [5, "안녕", 3.5, Person(), Superman(), {(name: String) in return name}]
    
    //array[1].count
    (array[1] as! String).count
    
    for (index, item) in array.enumerated() {
    		// is/ as 패턴으로 switch문에서 case 별로 분기처리 가능!
        switch item {
        case is Int:                                  // item is Int
            print("Index - \(index): 정수입니다.")
        case let num as Double: // 이미 Double 타입으로 변환 가능한 경우를 다루므로 as  // let num = item as Double
            print("Index - \(index): 소수 \(num)입니다.")
        case is String:                               // item is String
            print("Index - \(index): 문자열입니다.")
        case let person as Person:                    // let person = item as Person
            print("Index - \(index): 사람입니다.")
            print("이름은 \(person.name)입니다.")
            print("나이는 \(person.age)입니다.")
        case is (String) -> String:                   // item is (String) -> String
            print("Index - \(index): 클로저 타입입니다.")
        default:
            print("Index - \(index): 그 이외의 타입입니다.")
        }
    }
    // 출력 결과
    
    // Index - 1: 문자열입니다.
    // Index - 2: 소수 3.5입니다.
    // Index - 3: 사람입니다.
    // 이름은 이름입니다.
    // 나이는 10입니다.
    // Index - 4: 그 이외의 타입입니다.
    // Index - 5: 클로저 타입입니다.
    ```
    
    - 옵셔널 값의 Any 반환
        - Any는 모든 타입, 옵셔널 타입까지 포함하므로 옵셔널 값을 사용할 때 Any 타입으로 변환해주면 컴파일러의 경고를 없앨 수 있다.
        - 옵셔널값은 임시적인 값이므로 옵셔널을 벗겨줘야 하는데 Any로 변환하는 것은 그 자체를 사용하겠다는 의미이다.
        
        ```swift
        let optionalNumber: Int? = 3
        print(optionalNumber)          // 경고
        print(optionalNumber as Any)   // 경고 없음
        ```
        
    
    
2. AnyObject
    - 어떤 클래스 타입의 인스턴스도 표현할 수 있는 타입
    - 구조체 인스턴스 불가능 클래스만 !
    
    ```swift
    let objArray: [AnyObject] = [Person(), Superman(), NSString()]
    
    //objArray[0].name
    (objArray[0] as! Person).name
    ```

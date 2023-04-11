---
layout: post
title : "[Swift] self vs Self 비교"
categories : Swift
tags : 
    - Swift
    - self
    - Self
    - 인스턴스
    - 타입
---     

## self와 Self 비교

생성자를 다루면서 자주 사용했었던 self와 간간히 나오는 Self의 차이점을 비교해보겠다   
### self 
> self는 인스턴스를 가리킨다!!   


- self는 자기 자신의 인스턴스를 가리킨다고 한다
- 다양한 예시를 보며 self가 쓰이는 경우를 알아보자     

1. 생성자 내부에서 사용      
클래스 or 구조체의 생성자에서 해당 저장 프로퍼티를 초기화할 경우 `self.저장 프로퍼티` 와 같이 self 키워드로 자기 자신의 인스턴스의 저장 프로퍼티에 접근한다        
   
  ```swift
  class Person {
      var name: String

      init(name: String) {
          self.name = name
      }
  }

  var p = Person(name:"sumin")
  ```       
   
2. 클래스 내부에서 저장 프로퍼티에 접근하는 경우 사용      
  ```swift
  class Person {
      var name: String
      init(name: String) {
          self.name = name
      }
      func sayMyName() {
          print(self.name)
      }
  }

  var p = Person(name:"sumin")
  p.sayMyName()
  // sumin 출력
  ```        
  다음과 같이 p 인스턴스는 self 키워드를 통해 자기 자신의 인스턴스의 name 저장 프로퍼티를 초기화하고,
  함수 내부에서 자기 자신의 인스턴스에 접근하여 해당 프로퍼티를 출력할 수 있다!    
  물론 구조체에서는 값 타입으로 함수에서 자기 자신의 프로퍼티에 접근할 수 없으므로
  self 키워드 없이 `mutating fun` 키워드를 사용하면 된다   

3. 새로운 값으로 프로퍼티 초기화 하는 경우      
- 값 타입에서는 인스턴스 값 자체를 치환할 수 있다
- 인스턴스를 새로 생성하여 치환하는 것도 가능하다
- 클래스는 사용 불가능 (클래스는 참조 타입이므로 self 타입 변경 불가능하다)   


  ```swift
  struct Calculator {
      var number: Int = 0

      mutating func plusNumber(_ num: Int) {
          number = number + num
      }

      // 구조체의 메서드 내에서 self 키워드로 인스턴스를 새로 생성하여 치환
      mutating func reset() {
          self = Calculator()  
      }
  }

  var c = Calculator(number: 4)
  c.reset()
  print(c.number) // 0 출력
  ```   

4. 타입 멤버에서 사용하는 경우, 인스턴스가 아닌 타입 자체를 가르킨다   
  ```swift
  struct Dog {
      static let species = "강아지"

      static func doPrintSpecies() {
          print("종은 \(self.species)입니다.")
      }
  }

  Dog.doPrintSpecies() // 종은 강아지입니다. 출력
  ```      
self 키워드가 타입 메서드내에서 해당 인스턴스가 아닌 해당 타입을 가르킨다   

5. 타입 인스턴스를 가르키는 경우, 타입 자체의 뒤에 붙여서 사용한다      
- 외부에서 타입을 가르키는 경우
- 타입 인스턴스는 데이터 영역에 저장된다   

  ```swift
  class SomeClass {
      static let name = "클래스"
  }


  let myClass: SomeClass.Type = SomeClass.self
  myClass.name

  SomeClass.name //  "클래스"
  SomeClass.self.name //  "클래스"

  Int.max // 9223372036854775807
  Int.self.max // 9223372036854775807 
  ```        

### Self
> Self는 타입을 가르킨다!   

1. 특정 타입 내부에서 타입을 선언하는 위치에서 사용한다    
  ```swift
  extension Int {
      // 타입 저장 속성
      static let zero: Self = 0   

      // 인스턴스 계산속성
      var zero: Self {  
          return 0
      }
       static func toZero() -> Self {
          return Self.zero     
      }


      // 인스턴스 메서드
      func toZero() -> Self {
          return self.zero    
      }
  }
  ```   
    
    
2. 프로토콜에서의 Self 사용   
- 범용적으로 사용가능한 프로토콜에서 사용
- 프로토콜을 채택하는 해당 타입을 가르킨다!

![image](https://user-images.githubusercontent.com/110437548/231165334-bcfdae77-cb1e-4e7a-a495-18ba9aadf314.png)    


Some 프로토콜을 채택하는 Mouse에서는 name의 타입이 Mouse로 잘 들어가 있음을 알 수 있음!   


이제 self 와 Self의 차이를 명확히 알고 사용할 수 있을 것 같다!




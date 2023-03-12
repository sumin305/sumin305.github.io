---

layout: post
title : "[Design Pattern] 델리게이트 패턴이란?"
categories : Design Pattern
tags : 
    - Swift
    - Delegate
    - Delegate Pattern
    - Design Pattern
---
# Delegate Pattern 
<aside>
💡 특정 다른 클래스에 있는 어떠한 행동을 하고 싶을 때, 상속을 사용하는 대신 delegate라는 위임자를 선언하여 다른 클래스의 행동을 재사용하도록 하는 것. 이때, 위임할 클래스에 위임한 내용을 수신받을 클래스의 객체를 참조하도록 해줘야됨.

</aside>

***"Delegate는 어떤 객체가 해야 하는 일을 부분적으로 확장해서 대신 처리를 한다."***

- `delegate` 는 위임자라고 해석할 수 있다.
- 위임자를 갖고 있는 객체가 다른 객체에게 자신이 해야하는 일을 위임하는 형태의 `디자인 패턴`
- 클래스 상속과 동일하게 `코드를 재사용`할 수 있도록 하는 `객체 지향 디자인 패턴`이다
- swift 에서의 Delegate Pattern 구현 :
    - 어떠한 클래스에서 특정 protocol의 type을 가지는 `delegate` 위임자 변수를 가져서 다른 클래스의 객체에게 자신의 할일을 위임할  수 있다  ~~→ 왜 optional type일까?~~
- 이러한 특정 protocol을 준수하는 class들을 `delegate class`라고 한다
    - 이러한 `delegate class`들은 `delegate` 위임자 변수를 가지고 있는 class에서 이벤트를 전송하면, 이벤트를 감지하여 대리자 메서드를 호출한다 (protocol에 기입되어 있는 메서드들)
    - 즉 위임자를 갖고 있는 객체가 해야 하는 일중에 일부분을 커스텀하여 대신 처리를 해준다

## 예시

```swift
protocol Area {
    func area(width: Double, height: Double)
}

class Rectangle: Area {
		//Window 인스턴스에 대한 참조자를 갖고 있음.
    init(receiver: Window) {
        receiver.delegate = self
    }
    func area(width: Double, height: Double) {
        print(width * height)
    }
}

class Window {
    var delegate: Area?
}

var w = Window()

let r = Rectangle(receiver: w)

//Window 클래스가 Rectangle 인스턴스에게 area() 연산을 넘겨준다
w.delegate?.area(width: 40, height: 30)
```

- 두 객체가 하나의 요청을 처리하는데, 수신 객체가 연산의 처리를 위임자에게 보낸다. (서브 클래스가 부모 클래스에게 요청 전달과 유사)
- (연산 처리의 결과를 받는) 수신 객체는 대리자에게 자신을 매개변수로 전달해서 위임된 연산이 수신자를 참조하게 한다
- Window class를 Rectangle class의 서브클래스로 만드는 대신, Window class는 Rectangle class를 자신의 인스턴스 변수로 만들고, Rectangle class에 정의된 행동이 필요할 때만 Rectangle class에 위임함으로써 Rectangle class의 행동을 재사용할 수 있다

<aside>
💡 즉, 상속에 의해 Window인스턴스를 Rectangle 인스턴스로 간주하는 것이 아닌, Window 인스턴스가 Rectangle 인스턴스를 포함하도록 하고, 
Window 인스턴스가 자신이 받은 요청을 Rectangle 인스턴스로 전달하는 것!

</aside>

```swift

protocol Work {
    func Coding(language: String)
    func Reading(num: Int)
}

class Boss {
    var delegate: Work?
}

class Employee1: Work {
    
    init(supervisor: Boss) {
        supervisor.delegate = self
    }
    
    func Coding(language: String) {
        print("\(language)언어로 코딩하는중..")
    }
    
    func Reading(num: Int) {
        print("\(num)줄째 코드 리딩중..")
    }
}

class Employee2: Work {
    
    init(supervisor: Boss) {
        supervisor.delegate = self
    }
    
    func Coding(language: String) {
        print("\(language)언어로 코딩하는중..")
    }
    
    func Reading(num: Int) {
        print("\(num)줄째 코드 리딩중..")
    }
}
var boss = Boss()

//boss의 delegate를 sunny로
let sunny = Employee1(supervisor: boss)
boss.delegate?.Coding(language: "python")

//boss의 delegate를 marco로
let marco = Employee2(supervisor: boss)
boss.delegate?.Reading(num: 4)
```

## Cocoa API에서 사용하는 Delegate Pattern

cocoaAPI에서도 UI를 구현할 때 Delegate Pattern을 자주 사용한다고 한다   

```swift
protocol NSWindowDelegate {
	func window(NSWindow, willPositionSheet: NSWindow, using: NSRect) -> NSRect
}

//위임자 클래스
//이벤트 감지 -> 대리자 메서드 호출한다
//func window~ -> 앱이 이벤트에 응답하는 방식으로 사용자 지정함
class MyDelegate: NSObject, NSWindowDelegate {
    func window(_ window: NSWindow, willUseFullScreenContentSize proposedSize: NSSize) -> NSSize {
        return proposedSize
    }
}

//NSWindow class에서 `var delegate: NSWindowDelegate?`라는 delegate 변수 선언
let myWindow = NSWindow(
    contentRect: NSRect(x: 0, y: 0, width: 5120, height: 2880),
    styleMask: .fullScreen,
    backing: .buffered,
    defer: false
)

//myWindow의 위임자를 지정
myWindow.delegate = MyDelegate()

//사용자가 창 크기를 조정하여 이벤트 발생!
//let eventResponse = myWindow.delegate?.window(myWindow, willUseFullScreenContentSize: mySize)
//위 코드와 같이  이벤트 응답할 필요가 없으면 위임자 생성할 필요 없다.
// 위임자에게로 메세지를 보내기전에 옵셔널 체이닝을 이용하여 해당 window의 위임자가 존재하는지 확인한다!
if let fullScreenSize = myWindow.delegate?.window(myWindow, willUseFullScreenContentSize: mySize) {
    print(NSStringFromSize(fullScreenSize))
}
```

- 참고

[Using Delegates to Customize Object Behavior | Apple Developer Documentation](https://developer.apple.com/documentation/swift/using-delegates-to-customize-object-behavior)

## 클래스 상속

- 서브클래싱
- 다른 부모 클래스에서 상속받아 한 클래스의 구현을 정의하는 것
- 메소드, 프로퍼티와 다른 특징들을 다른 클래스로 부터 상속받을 수 있다
- 저장 프로퍼티, 계산 프로퍼티와 상관없이 상속받은 프로퍼티에 `프로퍼티 옵저버`를 설정해 값 설정에 반응할 수 있다
- `화이트박스` 재사용

## 객체 합성

- 클래스 상속에 대한 대안
- 다른 객체를 여러 개 붙여서 새로운 기능 혹은 객체를 구성하는 것 → 객체 또는 데이터 유형을 더 복잡한 유형을 결합한다
- 객체를 합성하려면, 합성에 들어가는 객체들의 인터페이스를 명확하게 정의해 두어야함
- `블랙박스` 재사용

## Delegate Pattern를 사용하는 이유

- 클래스 상속과 객체 합성의 적절한 조합
- 객체의 내부를 공개하지 않고 protocol을 통해서만 재사용된다 (블랙박스 재사용)
- 런타임에 행동의 복합을 가능하게 하고, 복합하는 방식도 변경해준다
- 클래스의 캡슐화에 유리!
- 위임하고 있는 클래스의 변화가 발생하더라도 (함수의 추가 등) 상속과 다르게 문제가 발생하지 않는다 !

## 단점

- 클래스에 상호작용이 다 정의되어 있지 않고, 런타임 시 delegate를 선언한 객체에 따라 그 결과가 달라진다
    - →  런타임에 비효율적
    - → 정적인 디자인 구조보다 이해가 어려움
- 복잡함 < 단순화의 효과일 경우, 이러한 `Delegate Pattern` 을 사용하는 것이 유리하다

## 참고

[Using Delegates to Customize Object Behavior | Apple Developer Documentation](https://developer.apple.com/documentation/swift/using-delegates-to-customize-object-behavior)

[[디자인 패턴] 위임 패턴(Delegate Pattern)](https://june0122.github.io/2021/08/21/design-pattern-delegate/)

[[iOS] Delegate 패턴이란 무엇일까?](https://velog.io/@zooneon/Delegate-패턴이란-무엇일까)

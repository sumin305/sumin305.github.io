## 1. 클래스와 구조체

- 같은 데이터들을 가지고 있는  데이터 묶음을 정의해두는 틀
- 동일한 종류의 값들 (이름, 나이, 성별 등)을 가지고 있는 객체들을 생성할 때, 공통으로 사용되는 부분을 미리 정의해놓는것
- 즉, `객체를 생성하기 위한 설계도`라고 할 수 있다
- 사용자 정의 타입 : String, Int와 같은 타입을 사용자가 정의한다 → Optional 처리 가능
- 프로퍼티, `메서드`로 구성
- 데이터 모델링 시 주로 사용
- .(점문법)으로 내부의 멤버( 프로퍼티, 메소드)에 접근

```swift
//데이터 묶음을 정의 (사용자가 직접 type 생성)
class Vehicle {

    //프로퍼티(변수)
    var wheelCount: Int = 4
    var name: String = "car"

    //메서드(함수)
    func engineSound() {
				//메서드에서 해당 class/struct내의 프로퍼티에 접근할 수 있음
        print("\(name)이 달린다 부아아앙~")
				// 메서드 정의문 내에는 메서드(함수) 실행문 존재할 수 있다!
				turnRight()
    }
		func turnRight() {
				print("오른쪽으로 회전")
		}

		//클래스 내부에는 직접 메서드(함수) 실행문이 올 수 없다.
		turnRight()  //컴파일 에러
}

//인스턴스 생성
var v1 = Vehicle()
var v2 = Vehicle()

//점문법으로 인스턴스의 멤버에 접근
v2.name = "autobicycle"
v2.wheelCount = 2
v1.engineSound()

```

```swift

//앱 개발 시 데이터를 다룰 때 거의 필수적으로 사용 & 네트워크 통신 시에 (json형태와 유사)도 사용 
struct AccountData: Codable{
    
    var category: AccountCategory = .none
    var title: String = ""
    var account: String = "0"
    var date: Date = Date()
    var is_expenditure: Bool = true
    
    init(category: AccountCategory, title: String, account: String, is_expenditure: Bool) {
        self.category = category
        self.title = title
        self.account = account
        self.is_expenditure = is_expenditure
    }
}
```

---

## 2. 메모리 구조

### 클래스

- `참조 타입` : 변수나 상수에 값을 할당을 하거나 함수에 인자로 전달할 때 그 값이 복사되지 않고 참조 (메모리 참고 하고 있다)
- 인스턴스 데이터를 `힙`에 저장
- 힙을 가르키는 변수(메모리 주소 값)는 스택에 저장 → 복사 시 힙을 가르키는 메모리 주소 값을 전달
- ARC시스템 (참조 카운트) 을 통해 메모리 관리
- 소멸자가 있음
    

```swift
//class의 인스턴스 복사 예제
class Person {
    var name: String
    var nickName: String
    
    init(name: String, nickName: String) {
        self.name = name
        self.nickName = nickName
    }
}

var p1 = Person(name: "수민", nickName: "daisy")

//s1에 Person클래스의 인스턴스 s의 값 복사 (힙에 있는 s를 가르키는 메모리의 주소가 전달됨)
var p2 = p1

//s1과 s가 힙의 같은 인스턴스를 가르키고 있으므로 s1의 멤버를 수정하면 s의 멤버도 변한다
p2.nickName = "sunny"

//"sunny" 출력
print(p1.nickName)
```

### 구조체

- `값 타입` : 함수에서 상수나 변수에 전달될 때 그 값이 **복사되서 전달**
- 인스턴스 데이터를 `스택`에 저장
- 복사 시 다른 메모리 공간을 생성하여 복사본을 넣는다
- 데이터를 스택에 저장하므로, 스택 프레임 종료 시, 메모리에서 자동으로 제거된다   


```swift
//struct의 인스턴스 복사 예제
struct Person {
    var name: String
    var nickName: String
}

var p1 = Person(name: "sumin", nickName: "daisy")

//서로 다른 메모리 공간에 값만 복사가 되었다 
var p2 = p1

//복사된 값의 프로퍼티를 수정하면 메모리 주소를 다르므로 그 인스턴스의 값만 변한다
p2.nickName = "marco"

//"daisy" 출력
print(p1.nickName)
```

## 3. let과 var

- class
    - let으로  인스턴스를 선언할 경우 힙에 있는 `메모리 주소`가 변할 수 없는 값이 된다
    - 메모리 주소 복사 불가능
- struct
    - let으로 인스턴스를 선언할 경우 그 스택에 있는 `인스턴스의 프로퍼티`들이 let으로 선언된 것처럼 취급된다

```swift
class PersonClass {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

struct AnimalStruct {
    var name: String
    var age : Int
}

//***************<class>***************//
let person1 = PersonClass(name: "clamp", age: 22)
var person2 = PersonClass(name: "marco", age: 18)

//person1 = person2 -> error! person1은 let으로 선언되었기 때문에 메모리 주소를 변경할 수 없음

//person1의 메모리 주소가 복사
person2 = person1
//"clamp" 출력
print(person2.name)

//person1과 person2는 현재 메모리 주소를 공유하고 있음
person1.name = "daisy"
//"daisy" 출력
print(person2.name)

//***************<struct>***************//
let animal1 = AnimalStruct(name: "panda", age: 5)
var animal2 = AnimalStruct(name: "rabbit", age: 3)

//animal1 = animal2 -> error! animal1의 속성들은 let으로 취급되므로 변경 불가능

//animal2의 메모리 공간내에 있는 프로퍼티의 값이 바뀐다
animal2 = animal1
//"panda" 출력
print(animal2.name)
```

## 4. 초기화


> 💡 모든 저장 프로퍼티를 초기화 해야된다!
> 생성자 종료 시점에서는 모든 저장 프로퍼티의 초기값이 저장되어 있어야 함!!



- 생성자 (init)
    - 인스턴스를 만들 때 사용하는 특별한 메서드이다 → 저장 프로퍼티 초기화가 목적
    - `func` 키워드 생략
    - Optional Type의 프로퍼티인 경우, nil값으로 자동으로 초기화된다
    - self : 클래스나 구조체 내에서  자기 자신의 인스턴스를 가르킨다
    - 오버로딩 지원
- struct
    - 구조체에서는 초기화시 프로퍼티를 선언할 수 있는 생성자 자동으로 생성하여 제공

```swift
class PersonClass {
    var name: String
    var age: Int
		var weight: Double?
    
		//init 함수를 작성해주거나, 프로퍼티 선언과 동시에 초기화를 해주거나, optional값으로 지정해야된다
    init(name: String, age: Int) {
				//클래스나 구조체 내에서 자기 자신의 인스턴스를 가르킴
        self.name = name
        self.age = age
    }
}

struct AnimalStruct {
    var name: String
    var age : Int
}

let person1 = PersonClass(name: "clamp", age: 22)
print(person1.weight) //nil출력 (Optional type이므로 자동 초기화)

let animal1 = AnimalStruct(name: "panda", age: 5)
```

- 식별 연산자
    - 두 개의 참조가 같은 인스턴스를 가르키고 있는지 비교
        - ===
        - !==



## 5. 언제 사용할까?

- 구조체
    - 단순 캡슐화 구현할 경우
    - 인스턴스들을 참조하는 것보다 복사하는 것이 유리할 경우
    - 프로퍼티나 메소드 상속이 필요 없을 경우
    - 애플에서는 구조체 사용을 권장
    - stack에 데이터를 저장하기 때문에 메모리 사용이 가볍고 용이함
    
- 클래스
    - 상속의 구조가 필요할 경우
    - 인스턴스들을 참조하는 것이 유리할 경우

## 6. 클래스와 구조체의 뷰 구현

- UIkit
    - UIView는 제약과 배경, 정렬방법등과 같은 매우 많은 프로퍼티와 메서드를 가지고 있는 클래스의 서브클래스
    - UIkit에서의 모든 View는 이러한 UIView를 또 상속받은 서브클래스이므로
    - 뷰 생성 시 그 안에 너무 많은 프로퍼티들이 담김
    - 참조 타입 -> 클래스의 뷰를 생성함으로 어떤 곳에서 참조를 한다면 내부 값을 쉽게 바꿀 수 있음
    - 그러므로, 항상 모든 뷰의 상태를 체크하고 변화를 감시해야함

- swiftUI
    - 구조체로 view를 생성
    - 상속되는 값 없이 생성하려는 구조체만큼의 크기를 가짐
    - 배경 및 색상과 같은 프로퍼티 -> ViewModifier 사용 (속성 부여)
    - 즉, 뷰 자체가 UIKit에 비해 매우 가벼움
    - 값 타입 -> 해당 뷰를 정의한 경우 그 정의한 곳에서만 사용되므로, 그 부분만 신경쓰면 된다
    - 메모리 관리에서도 매우 효율적.

```swift
//swiftUI내에서 기본으로 제공되는 모든 view component들은 구조체 형식으로 정의되고 사용된다
struct MainScrollView: View{
    
		@StateObject var dataManager: AccountDataManager = AccountDataManager.shared
    @State var acCategory: AccountCategory = .none

    var body: some View{
        VStack{
            List{
                ForEach(Array(dataManager.getList(Category: acCategory).enumerated()), id:\.offset) {idx, data in
                    AccountRow(accountData: data)
                }.onDelete(perform: deleteItem)
                
            }.padding()
            GetCategorySelectionArea(selectedCategory: $acCategory)
        }
    }
    
    func deleteItem(at offsets: IndexSet) -> () {
        dataManager.acDataList.remove(atOffsets: offsets)
        UserDefaults.standard.set(try? PropertyListEncoder().encode(dataManager.acDataList), forKey: AccountDataManager.ACCOUNT_DATA_LIST_KEY)
    }
}
```

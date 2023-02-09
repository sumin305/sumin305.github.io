---
layout: post
title : "[Project] Account Book"
categories : Project
tags : 
    - Swift
    - SwiftUI
    - Toy Project
    - View & View Model
    - State
    - Binding
    - UserDefaults
---
## Introduce
간단한 앱을 구현하고 다양한 기능들을 추가하며 앱 생성 시 어려웠던 점들이나 알게된 점들을 기록해보고자 글을 쓰기로 하였다


### Account Book
이번 프로젝트는 가계부 구현하기이다    
사용자에게 다양한 정보를 입력받아서 가계부 list에 추가한다   
각 가계부 정보들을, 금액, 메모, 카테고리등 다양한 종류의 데이터를 갖는다   
- 메인화면 상단에는 정보들을 입력받는 버튼과 전체 삭제버튼
- 중앙에는 가계부 list
- 하단에는 Category별로 list를 필터링하여 나타낼 수 있는 picker와   
- 총 지출, 수입, 총 자산 등 현재 상황을 나타내주는 Text칸이 있다   
- 이 칸은 list에 정보가 업데이트될 때마다 변한다   
<img width="257" alt="image" src="https://user-images.githubusercontent.com/110437548/216274396-382fb337-0ef3-49ae-8cbf-6268fe485d1e.png">   

* * *   
## View
### sheet
상단에는 데이터를 추가하는 뷰를 띄우는 버튼과 데이터를 모두 초기화하는 버튼이 있다  
데이터를 추가하는 뷰를 띄울 때는 sheet를 이용하였다   
다음은 sheet의 정의이다 
```swift
func sheet<Content>(
    isPresented: Binding<Bool>,
    onDismiss: (() -> Void)? = nil,
    content: @escaping () -> Content
) -> some View where Content : View
```
> sheet는 임시적인 작업 뷰를 띄우고 싶을 때 사용한다    
![Simulator Screen Recording - iPhone 14 Pro - 2023-02-02 at 17 51 43](https://user-images.githubusercontent.com/110437548/216276667-69fbadfe-271c-4571-9dfd-effc797906a3.gif)     

- isPresented: Binding<Bool> 매개변수로 sheet을 띄우고, 해제할 수 있다   
보통 body밖에 `@State`로 SwiftUI가 참조하도록 bool함수를 선언하여 isPresented에 따라서 swiftUI가 sheet을 나타내거나 없애도록 한다       
<br>    
  
  
- sheet 지우는 경우
  - 돌아가기 버튼을 누른 경우 : Dismiss()
  - 데이터를 추가하는 경우 : isPresented
  이 두가지이다    
  Dismiss()는 ```@Environment(\.dismiss) private var dismiss```을 선언하여 사용하는데 이것은 dismiss의 정의를 살펴보면  `extension EnvironmentValues`에 정의되어 있다   
- 프로젝트내에 활용한 sheet content->  `InputAccountModal` View를 나타내도록 했다
 ```swift
.sheet(isPresented: self.$isShowModal) {
    InputAccountModal(isPresented:  self.$isShowModal)
}
 ```

### On Delete
> list View에서 ForEach를 통해 행 그룹을 열거해 목록을 작성할 때, 각각의 항목을 삭제하고 싶은 경우에 사용한다   
  
다음은 On Delete의 정의이다 
```swift
func onDelete(perform action: Optional<(IndexSet) -> Void>) -> some DynamicViewContent
```
이렇게 onDelete로 perform의 인자로 클로저나 함수를 넣어주는데, 이 클로저나 함수는 삭제하는 해당 항목의 `IndexSet`을 받아올 수 있다   
아래는 내가 작성한 코드이다    
  
```swift
  func deleteItem(at offsets: IndexSet) -> () {
        dataManager.acDataList.remove(atOffsets: offsets)
    }
 List{
    ForEach(Array(dataManager.getList(Category: acCategory).enumerated()), id:\.offset) {idx, data in
           AccountRow(accountData: data)
     }.onDelete(perform: deleteItem)
}.listStyle(PlainListStyle())
```
dataManager등은 아직 소개하지 않았지만 코드를 통해 삭제하려는 IndexSet을 받아와서 리스트의 해당 항목을 삭제할 수 있음을 알 수 있다   
이렇게 View에서는 sheet View와 List Foreach의 OnDelete 함수를 사용해보고 그 정의를 조사해보았다   
  
* * *    
## View Model
사용자가 데이터를 추가 혹은 삭제한 후 앱을 껐다 키면 그 변화된 데이터 상태가 유지되어 있어야 한다
나는 서버까지는 필요없는 작은 미니 프로젝트이고 앱이 활동할 때만 앱 내에 데이터를 저장하면 되므로 `User Default`를 사용하여 이러한 데이터들을 저장하였다 
  
### User Default
> `사용자의 기본 데이터베이스에 대한 인터페이스`로, 앱을 실행할 때마다 키-값 쌍을 영구적으로 저장할 수 있다   
- Default : 일반적으로 앱을 시작할 때 앱의 기본 상태 또는 기본적으로 작동하는 방법을 결정하는 데 사용되는 Parameter들을 가리킨다     
- 앱은 사용자의 기본 데이터베이스에 있는 `Parameter Set`에 값을 할당하여 이러한 환경설정을 저장한다 

```swift
class AccountDataManager: ObservableObject {
   static let ACCOUNT_DATA_LIST_KEY: String = "ACCOUNT_DATA_LIST_KEY"
  
  init() {
        if let data = UserDefaults.standard.value(forKey: AccountDataManager.ACCOUNT_DATA_LIST_KEY) as? Data{
            let accountList = try?
            PropertyListDecoder().decode([AccountData].self, from: data)
            if let list = accountList {
                acDataList = list
            }
        }
    }
   func add(AccountData acData: AccountData?) -> Bool {
        if let data = acData {
            acDataList.insert(data,at:0)
            
            UserDefaults.standard.set(try?
                                      PropertyListEncoder().encode(acDataList),
                                      forKey: AccountDataManager.ACCOUNT_DATA_LIST_KEY)
            return UserDefaults.standard.synchronize()
        }
        return false
    }
 }
```    
- `PropertyListDecoder().decode()`: 기본 속성 목록 형식을 사용하여 속성 목록을 디코딩하여 지정된 유형의 값을 반환한다   
하단에는 프로젝트에 User Default를 적용한 코드이다     
- key를 먼저 선언한 후, UserDefault에서 해당 key에 mapping되는 데이터를 가져와 원하는 데이터 형태로 decode한다   

- add함수는 사용자가 데이터 즉, 가계부에 특정 항목을 추가할 경우 호출되는 함수이다    
- 받아온 추가할 데이터가 nil이 아닐 경우, 추가하는데 그 때 UserDefault에도 업데이트를 해줘야한다    
- `PropertyListEncoder().encode(acDataList)` 아까는 decode()함수를 사용했었는데, 이번에는 입력받은 데이터를 User Default에 저장할 수 있는 데이터 형태로 encode하여 업데이트한다    
- `UserDefaults.standard.synchronize()` : default 데이터 베이스로의 비동기적인 보류적인 업데이트를 기다리고 그 데이터가 디스크에 성공적으로 저장되면 true, 그렇지 않으면 false를 반환한다   
- 해당 어플에서는 데이터를 추가할 경우 `UserDefaults.standard.synchronize()`에서 true를 반환할 경우 !을 붙여 그 반대값인 false를 sheet를 나타내는 여부를 결정하는 `isPresented`에 넣어주어 데이터 베이스에 저장이 완료되면 데이터 추가 화면인 sheet를 없애도록 하였다   
이렇게 설정을 해줌으로써 해당 데이터가 완전히 default 데이터 베이스 저장이 되었을 때 다음 동작을 수행할 수 있도록 해 좀 더 안전하게 코드를 짤 수 있다  

## Takeaway 
서버를 따로 설정하지 않아도 어플 내에 간단한 정보들이 저장되는 것은 앞으로 어플을 생성할 때 굉장히 필요한 기능인 것 같아서 더욱 연습해봐야겠다고 느꼈다   
초기에 코트를 짰을 때 데이터를 삭제할 경우에 UserDefault 업데이트를 해주지 않아서 어플을 껐다키면 삭제한 데이터가 다시 나타내는 버그 때문에 꽤나 오랜시간 코드를 붙잡고 고생을 했다   
하지만 그런 시간 덕분에 어느정도 UserDefault에 익숙해진 것 같아서 다행이다    
이렇게 코드를 쳐보고 분석하는 시간은 참 재밌는 것 같다   
앞으로도 자주 실습을 같이 하면서 문법 공부를 하는 시간을 가져야겠다    

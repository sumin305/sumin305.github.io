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
sheet는 임시적인 작업 뷰를 띄우고 싶을 때 사용한다    
![Simulator Screen Recording - iPhone 14 Pro - 2023-02-02 at 17 51 43](https://user-images.githubusercontent.com/110437548/216276667-69fbadfe-271c-4571-9dfd-effc797906a3.gif)   
- isPresented: Binding<Bool> 매개변수로 sheet을 띄우고, 해제할 수 있다   
보통 body밖에 `@State`로 SwiftUI가 참조하도록 bool함수를 선언하여 isPresented에 따라서 swiftUI가 sheet을 나타내거나 없애도록 한다   
- sheet 지우는 경우
  - 돌아가기 버튼을 누른 경우 : Dismiss()
  - 데이터를 추가하는 경우 : isPresented
  이 두가지이다    
  Dismiss()는 ```@Environment(\.dismiss) private var dismiss```을 선언하여 사용하는데 이것은 dismiss의 정의를 살펴보면  `extension EnvironmentValues`에 정의되어 있다   
- 프로젝트내에 활용한 sheet
sheet content->  `InputAccountModal` View를 나타내도록 했다
 ```swift
.sheet(isPresented: self.$isShowModal) {
    InputAccountModal(isPresented:  self.$isShowModal)
}
 ```

### On Delete
- list View에서 ForEach를 통해 행 그룹을 열거해 목록을 작성할 때, 각각의 항목을 삭제하고 싶은 경우에 사용한다   
  다음은 On Delete의 정의이다 
```swift
func onDelete(perform action: Optional<(IndexSet) -> Void>) -> some DynamicViewContent
```
이렇게 onDelete로 perform의 인자로 클로저나 함수를 넣어주는데, 이 클로저나 함수는 삭제하는 해당 항목의 IndexSet을 받아올 수 있다   
아래는 내가 작성한 코드이다    
  
```swift
  func deleteItem(at offsets: IndexSet) -> () {
        dataManager.acDataList.remove(atOffsets: offsets)
        UserDefaults.standard.set(try? PropertyListEncoder().encode(dataManager.acDataList), forKey: AccountDataManager.ACCOUNT_DATA_LIST_KEY)
    }
 List{
    ForEach(Array(dataManager.getList(Category: acCategory).enumerated()), id:\.offset) {idx, data in
           AccountRow(accountData: data)
     }.onDelete(perform: deleteItem)
}.listStyle(PlainListStyle())
```
이렇게 View에서는 sheet View와 List Foreach의 OnDelete 함수를 사용해보고 그 정의를 조사해보았다   
  
* * *    
## View Model
사용자가 데이터를 추가 혹은 삭제한 후 앱을 껐다 키면 그 변화된 데이터 상태가 유지되어 있어야 한다
나는 서버까지는 필요없는 작은 미니 프로젝트이고 앱이 활동할 때만 앱 내에 데이터를 저장하면 되므로 `User Default`를 사용하여 이러한 데이터들을 저장하였다 
  
### User Default
- `사용자의 기본 데이터베이스에 대한 인터페이스`로, 앱을 실행할 때마다 키-값 쌍을 영구적으로 저장할 수 있다
- 앱은 사용자의 기본 데이터베이스에 있는 `Parameter Set`에 값을 할당하여 이러한 환경설정을 저장한다 
- Default : 일반적으로 앱을 시작할 때 앱의 기본 상태 또는 기본적으로 작동하는 방법을 결정하는 데 사용되는 Parameter들을 가리킨다     
- `PropertyListDecoder().decode()`: 기본 속성 목록 형식을 사용하여 속성 목록을 디코딩하여 지정된 유형의 값을 반환한다   
하단에는 프로젝트에 User Default를 적용한 코드이다     
 key를 먼저 선언한 후, UserDefault에서 해당 key에 mapping되는 데이터를 가져와 원하는 데이터 형태로 decode한다   
  
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
 }
``` 

  
### AccountDataManage
### User Default
    

* * *     

## Add    


* * *
## Takeaway

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
### TopArea()
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
isPresented: Binding<Bool> 매개변수로 sheet을 띄우고, 해제할 수 있다   
보통 body밖에 `@State`로 SwiftUI가 참조하도록 bool함수를 선언하여 isPresented에 따라서 swiftUI가 sheet을 나타내거나 없애도록 한다   
InputAccountModal으로 시트의 View를 따로 정의해주었다      
  
![Simulator Screen Recording - iPhone 14 Pro - 2023-02-02 at 17 51 43](https://user-images.githubusercontent.com/110437548/216276667-69fbadfe-271c-4571-9dfd-effc797906a3.gif)   

<center>TopArea()의 body</center>     
  
```swift
@State private var isShowModal = false
  
var body : some View {
  Button {
    self.isShowModal = true
  } label: {
    Text("💵💵💵")
        .font(.system(size: 33.3))
        .frame(maxWidth: .infinity)
   }
    .sheet(isPresented: self.$isShowModal) {
     InputAccountModal(isPresented:  self.$isShowModal)
     }
 }
```   
  
<center>InputAccountModal(isPresent: Binding<Bool>)</center>   
   
```swift
struct InputAccountModal: View{
    @Binding var isPresented: Bool
    @Environment(\.dismiss) private var dismiss
    @State private var money: String = ""
    @State private var memo: String = ""
    @State private var selectedCategory: AccountCategory = .none
    @State private var is_bool : Bool = true
 ```
  
 InputAccountModal은 데이터 입력을 sheet이기 때문에 `@State`로 입력받을 변수들을 미리 선언해주었다   

    var TopButton: some View {
        VStack{
            Button {
                dismiss()
            } label: {
                Text("돌아가기")
            }
        }.padding()
    }
    
    var InputArea: some View{
        VStack{
            HStack{
                Button {
                    is_bool = true
                } label: {
                    Text("지출")
                        .font(.title)
                        .foregroundColor(.red)
                        .padding()
                        .background(is_bool ? .green : .white)
                        .cornerRadius(20)
                }
                Button {
                    is_bool = false
                } label: {
                    Text("수입")
                        .font(.title)
                        .foregroundColor(.red)
                        .padding()
                        .background(is_bool ? .white : .green)
                        .cornerRadius(20)
                }
            }.padding(.trailing)
            HStack{
                Text("얼마나 쓰셨나요?")
                    .font(.title)
                Spacer()
                Button {
                    let result = addAcountData()
                    isPresented = result
                } label: {
                    Image(systemName: "arrow.up")
                        .imageScale(.large)
                        .frame(width: 40, height: 40)
                        .foregroundColor(.white)
                        .background(.gray)
                        .clipShape(Circle())
                }
            }
            TextField("금액 입력", text: $money)
                .keyboardType(.decimalPad)
                .font(.title)
            Text("")
            TextField("메모 입력", text: $memo)
                .font(.title)
            Text("")
            Picker("지출 종류를 골라주세요", selection: $selectedCategory){
                ForEach(AccountCategory.allCases,id: \.self) { category in
                    Text(category.DisplayImoji)
                        .tag(category)
                }
            }.pickerStyle(.segmented)
            Text("")
            HStack{
                Text("오늘은~~")
                Spacer()
            }
            Text(selectedCategory.Display)
                .font(.title)
        }.padding()
    }
}
```   
  
### MainScrollView()
### TotalView()
* * *    
## View Model
### AccountBookModel
### AccountDataManager
    

* * *     

## Add    


* * *
## Takeaway

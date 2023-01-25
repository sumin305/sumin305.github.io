---
layout: post
title : "[Project] Caffeine Holic"
categories : Project
tags : 
    - Swift
    - SwiftUI
    - Project
    - View & View Model
    - Clone Coding
---
## Introduce
CNU SW academy에서 실습한 미니 프로젝트를 소개하고 복습하는 과정을 포스팅하려고 한다  
실습 프로젝트를 수행하면서 Swift 문법에 대해서도 자세히 알게 되었고 앱 개발 기술도 습득할 수 있었다     
다시 앱을 혼자서 구현해보면서 배운 점들을 상기시키고 추가할 점들이나 문제점 개선을 해보며 기록해려고 한다   

### Caffeine Holic
처음으로 진행한 미니 프로젝트의 주제는 'Caffeine Holic' 이었다   
사용자에게 상태 변화를 입력받고, 이 상태 변화를 기록하며 화면에 재밌게 나타내는 간단한 어플이다   
상단에는 Top Button이, 가운데에는 list와 Image View, 하단에는 상태 변화를 입력받는 Button들이 배치되어 있다   
<img width="299" alt="image" src="https://user-images.githubusercontent.com/110437548/213984080-945a7dab-3bdb-418f-b097-e8c80c28890b.png">   

### function
하단 버튼을 통해 스트레스 받기, 커피 먹기, 휴식 취하기등 상태 변화를 입력받을 수 있다   
이렇게 상태 변화를 입력 받았을 때의 변화는 다음과 같다
- list에 입력한 상태가 추가된다
- 현재 상태와 입력받은 상태가 같을 경우) 배경색이 그대로, 센터에 있는 이미지의 크기가 조금 커진다
- 현재 상태와 입력받은 상태가 다를 경우) 배경색이 변함, 센터에 있는 이미지가 변한다   
또한 상단의 Top Button을 누를 경우 초기화면으로 돌아간다   

* * *   

## View
```swift
    var body: some View {
        VStack {
            ZStack{
                DailyView
                Images
                Buttons
            }
        }
        .padding()
    }
```
다음과 같이 ContentView의 body안에 ZStack으로 View들을 겹쳐서 쌓았다  
ZStack도 Stack 개념 (First-In-Last-Out) 이기 때문에 화면 가장 깊은 곳에 배치할 DailyView를 가장 먼저 배치했다   
이렇게 하지 않고 Buttons을 먼저 배치할 경우, DailyView나 Images로 인해 버튼이 눌리지 않을 수도 있다 

<img width="233" alt="image" src="https://user-images.githubusercontent.com/110437548/213989348-cc95514c-b6c2-4941-a125-c3ebfb0f7653.png">


```swift
struct ContentView: View {
    @State private var dailyList : [String] = ["Not yet", "Hello~!"]
   
    var DailyView : some View {
        HStack(alignment: .top){
            VStack(alignment: .leading,spacing : 20){
                List(dailyList, id: \.self) { item in
                    Text(item)
                }
                Spacer()
                Text(" ")
            }
            .frame(width : 200)
            .opacity(30)
            Spacer()
        }
    }
    var Images : some View {
        Image()
            .resizable()
            .frame(width: 200, height: 200)
    }
    var Buttons : some View {
        VStack {
            HStack(spacing: 20){
                Text("")
                Spacer()
                Button("NEW"){
                }
            }
            Spacer()
            HStack{
                Button("Get Some Rest"){
                }.padding()
                Button("Get Some Coffee"){
                }.padding()
                Button("Get Some Stress"){
                }.padding()
            }
        }
    }
```
* * *    

## View Model
데이터와 뷰와의 코드를 분리하기 위해 Caffeine Model을 생성하였다   
다른 파일로 데이터를 구분하는 것은 헷갈리고 접근 제어를 신경써줘야 하기 때문에 더욱 복잡해졌다   
하지만 깔끔하고 정리되어 있는 코드를 짜기 위해선 필수적이라는 생각이 든다   

```swift
class CaffeineModel {

    enum CaffeineState : String {
        case Intro
        case Wakening
        case Stressful
        case Rest
    }
    
    let originList = ["Welcome", "to Cafeine Holic"]
    var dailyList : [String] = []
    var currentState : CaffeineState = CaffeineState.Intro
    var imageFrame : CGSize = CGSize(width : 200.0, height : 200.0)
```
생성한 Caffeine Model 파일에 클래스를 생성하고 enum 열거형 형식으로 model의 상태들을 나타내준다
리스트를 초기화할 경우를 위한 상수 문자열 originList, 상태가 변할 경우의 리스트 dailyList, 현재 상태를 나타내주는 인스턴스인 currentState, 또 기본 이미지 크기 값인 imageFrame 로 변수들을 선언해주었다   

```swift
func changeState (newState : CaffeineState) -> [String] {
        addDailyList(newState : newState)
        if newState == .Intro{
            dailyList = originList
            imageFrame.width = 200
            imageFrame.height = 200
        }
        else if newState != self.currentState { //상태가 변할 경우
            imageFrame.width = 200
            imageFrame.height = 200
            self.currentState = newState
        }
        else{ // 상태가 변하지 않을 경우
            if imageFrame.width < 350{
                imageFrame.width += 10
                imageFrame.height += 10
            }
        }
            return dailyList
    }
``` 
위의 함수는 버튼이 눌린 newState에 따른 처리를 나타내주었다   
- 일단 버튼이 눌릴 경우, `addDailyList` 함수를 통해 배경에 있는 List에 newState를 추가해주었다   
- 만약 newState가 .Intro일 경우, 초기화면으로 돌아가기 위해 리스트를 초기 리스트로 바꾸어주고, 이미지의 크기를 원본 크기로 바꾸었다   
- 만약 newState != currentState일 경우, 이미지를 해당 상태로 바꾸어주고, 이미지의 크기를 원본 크기로 바꾸었다   
- 상태가 변하지 않을 경우, 최대 크기를 지정한 후 최대 크기가 될 때까지 이미지의 크기를 증가하였다   
최대 크기를 지정하지 않을 경우 이미지가 매우 커지면 다른 view들을 밀어내 전체 화면이 망가지기 때문에 최대 크기를 설정해주었다   

```swift
    private func addDailyList (newState : CaffeineState) {
        dailyList.insert(newState.rawValue, at: 0)
    }
    func changeImage() -> String {
        return "Profile." + currentState.rawValue
    }
```
- addDailyList는 newState를 리스트에 추가해주는 함수이고,    
- changeImage()는 상태가 변할 경우, 이미지를 변화시켜 주는 함수이다   
changeImage() 함수는 CaffineModel의 상태가 변할 때마다 Images View에 신호를 줘서 이미지를 변경하도록 한다   
이 두 함수는 아래 코드와 같이 상태별로 case를 나눠서 일일히 코드를 작성해주지 않고    
`currentState/newState.rawValue`를 활용하여 짧게 작성해주었다

```swift
switch(currentState) {
  case .Intro : return "Profile.Intro";
  case .Wakening : return "Profile.Wakening";
  case .Stressful : return "Profile.Stressful";
  case .Rest : return "Profile.Rest";
```

```swift
func doReset() -> [String]{
  changeState(newState: .Intro)
}
func doRest() -> [String]{
  changeState(newState: .Rest)
}
func doWakening() -> [String]{
  changeState(newState: .Wakening)
}
func doStress() -> [String]{
  changeState(newState: .Stressful)
}
```
View의 버튼으로 부터 이러한 함수들을 호출하여 ViewModel에서 카페인 모델의 상태변화를 처리한다   

> View Model 생성 후 변경된 View Code
```swift
struct ContentView: View {
    @State var caffeine : CaffeineModel = CaffeineModel()
    @State private var dailyList : [String] = ["Welcome", "to Caffeine Holic"]
    
   
    func resetState() {
        dailyList = caffeine.doReset()
    }
    func getRest() {
        dailyList = caffeine.doRest()
    }
    
    func incCoffee() {
        dailyList = caffeine.doWakening()
    }
    func incStress() {
       dailyList = caffeine.doStress()
    }
  
     var DailyView : some View {
        HStack(alignment: .top){
            VStack(alignment: .leading, spacing : 20){
                List(dailyList, id: \.self) { item in
                    Text(item)
                }
                Spacer()
                Text(" ")
            }
            .frame(width : 200)
            .opacity(0.3)
            Spacer()
        }
        .padding()
    }
    var Images : some View {
            Image(caffeine.changeImage())
                .resizable()
                .frame(width: caffeine.imageFrame.width, height: caffeine.imageFrame.height)
        }
    var Buttons : some View {
        VStack {
            HStack(spacing: 20){
                Text("")
                Spacer()
                Button("NEW"){
                    resetState()
                }
            }
            .padding()
            Spacer()
            HStack{
                Button("Get Some Rest"){
                    getRest()
                }.padding()
                Button("Get Some Coffee"){
                    incCoffee()
                }.padding()
                Button("Get Some Stress"){
                    incStress()
                }.padding()
            }
        }
        .padding()
    }
```
* * *
## Add
### 색상 변경하기
나에게 주어진 첫번째 추가 요소는 상태 변화에 따른 배경 색상 변경하기였다  
View의 ZStack에 some View 하나를 더 추가하여 배경 색상을 나타내주었다    
```swift
var body: some View {
            ZStack{
                ColorView
                DailyView
                Images
                Buttons
            }
}
var ColorView : some View {
        VStack{
            
        }.frame(maxWidth : .infinity, maxHeight : .infinity)
        .background(bgColor)
        .opacity(0.5)
    }
```
이후 버튼이 눌려 상태가 변화할 때마다 색상을 바꾸어주었다   
- 초기 화면일 경우 white
- 휴식을 취할 경우 blue
- 커피를 마실 경우 brown
- 스트레스를 받을 경우 pink
```swift
@State var bgColor : Color = .white
   
    func resetState() {
        dailyList = caffeine.doReset()
        bgColor = .white
    }
    func getRest() {
        dailyList = caffeine.doRest()
        bgColor = .blue
    }
    
    func incCoffee() {
        dailyList = caffeine.doWakening()
        bgColor = .brown
    }
    func incStress() {
        dailyList = caffeine.doStress()
        bgColor = .pink
    }
```

### 리스트 변경
dailyList는 같은 상태가 입력되더라도 계속 리스트 가장 앞 부분에 추가한다   
이 점을 수정하여 같은 상태가 계속 입력될 경우, 추가하지 않고 원래 상태 표기 옆에 +1, +2와 같이 숫자를 더해간다

* * *
## Problem

* * *
## Takeaway
코드를 다시 짜보고 직접 설명을 작성해보니 실습 시간에 빠르게 지나갔던 내용들에 왜? 라는 질문을 던져볼 수 있는 시간이었다   
완성본만 보고서 다시 코드를 작성해보는 것은 삽질하는 시간이 매우 많지만 서치해보면서 알아가는 것도 많고 문제해결 능력도 기를 수 있었다   
앞으로도 이렇게 작고 큰 프로젝트들을 리뷰해보면서 생각과 코드를 정리하는 시간을 가져야겠다고 생각했다   

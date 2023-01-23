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



## Add
## Problem

---
layout: post
title:  "[알고리즘/Swift] 과제 진행하기"
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
  - stack
  - Last-in-First-Out
---

## 문제
과제를 받은 루는 다음과 같은 순서대로 과제를 하려고 계획을 세웠습니다.  

과제는 시작하기로 한 시각이 되면 시작합니다.  
새로운 과제를 시작할 시각이 되었을 때, 기존에 진행 중이던 과제가 있다면 진행 중이던 과제를 멈추고 새로운 과제를 시작합니다.  
진행중이던 과제를 끝냈을 때, 잠시 멈춘 과제가 있다면, 멈춰둔 과제를 이어서 진행합니다.  
만약, 과제를 끝낸 시각에 새로 시작해야 되는 과제와 잠시 멈춰둔 과제가 모두 있다면, 새로 시작해야 하는 과제부터 진행합니다.  
멈춰둔 과제가 여러 개일 경우, 가장 최근에 멈춘 과제부터 시작합니다.  
과제 계획을 담은 이차원 문자열 배열 plans가 매개변수로 주어질 때, 과제를 끝낸 순서대로 이름을 배열에 담아 return 하는 solution 함수를 완성해주세요.  

### 제한 사항
3 ≤ plans의 길이 ≤ 1,000  
plans의 원소는 \[name, start, playtime]의 구조로 이루어져 있습니다.  
name : 과제의 이름을 의미합니다.  
2 ≤ name의 길이 ≤ 10  
name은 알파벳 소문자로만 이루어져 있습니다.  
name이 중복되는 원소는 없습니다.  
start : 과제의 시작 시각을 나타냅니다.  
"hh:mm"의 형태로 "00:00" ~ "23:59" 사이의 시간값만 들어가 있습니다.  
모든 과제의 시작 시각은 달라서 겹칠 일이 없습니다.  
과제는 "00:00" ... "23:59" 순으로 시작하면 됩니다. 즉, 시와 분의 값이 작을수록 더 빨리 시작한 과제입니다.  
playtime : 과제를 마치는데 걸리는 시간을 의미하며, 단위는 분입니다.  
1 ≤ playtime ≤ 100  
playtime은 0으로 시작하지 않습니다.  
배열은 시간순으로 정렬되어 있지 않을 수 있습니다.   
진행중이던 과제가 끝나는 시각과 새로운 과제를 시작해야하는 시각이 같은 경우 진행중이던 과제는 끝난 것으로 판단합니다.  

### 입출력   


|plans|result|     
|---|---|    
|\[\["korean", "11:40", "30"], \["english", "12:10", "20"], \["math", "12:30", "40"]]	\["korean", "english", "math"]|\["korean", "english", "math"]|      
|\[\["science", "12:40", "50"], \["music", "12:20", "40"], \["history", "14:00", "30"], \["computer", "12:30", "100"]]	\["science", "history", "computer", "music"]|\["science", "history", "computer", "music"]|     
|\[\["aaa", "12:00", "20"], \["bbb", "12:10", "30"], \["ccc", "12:40", "10"]]	\["bbb", "ccc", "aaa"]|\["bbb", "ccc", "aaa"]|     


* * *
## 풀이
시각을 포맷을 다루는 문제가 처음이라 꽤 많은 시간이 걸렸지만 차근 차근 해결하니 풀 수 있었다.   
과제 수행을 완료하지 못한 과제들을 `waitStack`에 넣어준 후,    
남은 과제들을 시간이 빌 때마다 `waitstack`에 있는 과제들을 `popLast`하여 수행해주었다.    
그래도, pop한 과제를 수행을 다 못할 경우에는 남은 `남은 과제 수행 - 틈새 시간`을 다시 `waitStack`에 `pushLast` 해주었다.    

## 나만의 알고리즘
시각 포맷 변환 함수들을 먼저 만들고,  
입력 받은 과제 리스트들을 과제 시작하는 시각을 오름차순으로 정렬한다.    
그 후, 정렬된 과제 리스트들을 idx 1부터 돌면서   
i번째 과제 시작하는 시각과 (i-1번째 과제 시작하는 시간 + i-1번째 과제하는 시간)의 차를 구해서     
```swift
var waitTime = dateDif(sortedPlans[i][1], dateAdd(sortedPlans[i-1][1],Int(sortedPlans[i-1][2])!))
```   
그 차가 0 이상이면 i-1번째 과제를 완료한 것으로, 0 미만이면 과제를 전부 수행하지 못한 것으로 간주했다.    
- 과제를 완료한 경우에는
  - `result` 배열에 과제명을 넣어주고,
  - `waitTime > 0` 이고 `waitStack 원소가 있을 경우`, 그 과제를 수행해주고 과제 수행을 완료할 경우 `waitTime`에서 과제 수행하는 데 쓴 시간을 빼고 업데이트 해준다. 또, 수행 완료한 과제를 `result` 배열에 넣어준다.    
  - `waitTime` 동안에 과제 수행을 완료하지 못할 경우 `waitTime`을 다 썼으므로 0으로 초기화 해주고, `waitStack`에 과제의 남은 시간을 다시 `pushLast`해준다.   
- 과제를 완료하지 못한 경우에는, 
  - `waitStack`에 과제명과, `waitTime`을 튜플로 함께 넣어준다.
- 모든 처리를 마친 후, i가 마지막 과제일 경우, 무조건 완료가 되므로 `result` 배열에 넣어준다.
- 이 후, `waitStack`에 남아있는 모든 과제들을 `popLast`한 순서대로 완료해준다. (`result` 배열에 넣어준다)



시간 계산을 하는 함수

```swift
// 두 시각을 String으로 입력받아 두 시각의 차를 분으로 Int 값 반환
func dateDif(_ bighhmm:String, _ hhmm:String) -> Int {
    // a[0] = hh, a[1] = mm
    let a = hhmm.split(separator: ":").map{Int(String($0))!}
    // b[0] = hh, b[1] = mm
    let b = bighhmm.split(separator: ":").map{Int(String($0))!}
    // 분끼리의 차
    let min = b[1] - a[1]
    if a[0] == b[0] {
        return min
    } 
    // 시각끼리의 차
    let hour = b[0] - a[0]
    // 시각끼리의 차와 분끼리의 차를 minute 형태로 변환하여 반환
    return hour*60 + min
}

// 입력 받은 시간에 입력 받은 분을 더했을 때의 시간 String 값으로 반환
func dateAdd(_ hhmm:String, _ add:Int) -> String {
    let date = hhmm.split(separator: ":").map{Int(String($0))!}
    // 분끼리 더했을 때 60분을 넘을 때
    if date[1] + add < 60 {
        return String(date[0]) + ":" + String(date[1] + add)
    }
    let hour = (date[1] + add) / 60 + date[0]
    let min = (date[1] + add) % 60
    // minute이 0인 경우, mm 형식인 "00"으로 바꿔줘야함.
    if min < 10 {
        return String(hour) + ":" + "0" + String(min) 
    }
    return String(hour) + ":" + String(min)
}
```  

### 코드
```swift
import Foundation
// 두 시간을 String으로 입력받아 시간 차의 분을 Int 값을 반환
func dateDif(_ bighhmm:String, _ hhmm:String) -> Int {
    let a = hhmm.split(separator: ":").map{Int(String($0))!}
    let b = bighhmm.split(separator: ":").map{Int(String($0))!}
    let min = b[1] - a[1]
    if a[0] == b[0] {
        return min
    } 
    let hour = b[0] - a[0]
    return hour*60 + min
}
// 입력 받은 시간에 입력 받은 분을 더했을 때의 시간 String 값으로 반환
func dateAdd(_ hhmm:String, _ add:Int) -> String {
    let date = hhmm.split(separator: ":").map{Int(String($0))!}
    if date[1] + add < 60 {
        return String(date[0]) + ":" + String(date[1] + add)
    }
    let hour = (date[1] + add) / 60 + date[0]
    let min = (date[1] + add) % 60
    if min < 10 {
        return String(hour) + ":" + "0" + String(min) 
    }
    return String(hour) + ":" + String(min)
}

func solution(_ plans:[[String]]) -> [String] {
    print(dateAdd("12:56", 23))
    print(dateDif("14:28", "11:56"))
    var waitStack: [(String, Int)] = []
    var result: [String] = []
    // 과제 시작하는 시점으로 오름차순 정렬
    let sortedPlans = plans.sorted{ ($0[1].split(separator: ":")[0], $0[1].split(separator: ":")[1]) < ($1[1].split(separator: ":")[0] , $1[1].split(separator: ":")[1])}
   
    for i in 1..<plans.count {
        var waitTime = dateDif(sortedPlans[i][1], dateAdd(sortedPlans[i-1][1],Int(sortedPlans[i-1][2])!))
        // 과제를 수행한 경우
        if waitTime >= 0 {
            result.append(sortedPlans[i-1][0])
            while waitTime > 0 && !waitStack.isEmpty {
                let assign = waitStack.removeLast()
                // 임시 과제를 다 못 끝내는 경우 다시 스택에 넣는다
                if waitTime < assign.1 {
                    waitStack.append((assign.0, assign.1 - waitTime))
                    waitTime = 0
                } 
                // 임시 과제를 다 끝내는 경우, waitTime 갱신해주고 다시 while문
                else {
                    waitTime = waitTime - assign.1
                    result.append(assign.0)
                }
            }
        }
        // 과제를 수행하지 못한 경우
        else {
            waitStack.append((sortedPlans[i-1][0], abs(waitTime)))
        }
        // 마지막 과제에 도달할 경우, 무조건 수행한다
         if i == plans.count - 1 {
           result.append(sortedPlans[i][0])
         }
    }
    // waitStack에 남아있는 과제들을 전부 popLast해 result에 넣는다
    while !waitStack.isEmpty {
        let assign = waitStack.removeLast().0
        result.append(assign)
    }
    return result
}
```

---
layout: post
title:  "[알고리즘/Swift] 0126 빌딩 "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
  - dp
---

## 문제
윷대전시에는 빌딩 N 개가 한 줄로 세워져 있는 곳이 있다.   
한 줄로 선 빌딩의 왼편이나 오른편에 서서 빌딩을 관찰하면, 어떤 빌딩은 앞선 빌딩에 가려져 보이지 않을 수 있다.   
엄밀히 정의하면, i 번째 빌딩을 보기 위해서는 관측자와 i 번째 빌딩 사이에 있는 모든 빌딩이 i 번째 빌딩보다 높이가 낮아야 한다.   

![image](https://user-images.githubusercontent.com/110437548/214753920-653530e2-eb56-4b12-8200-8a8a9cc5a1a0.png)
위 그림은 빌딩의 높이가 [1, 4, 2, 5, 3, 7, 1] 인 예시이다.   
![image](https://user-images.githubusercontent.com/110437548/214753960-45816ba7-82fc-4fad-83ef-988df91077b1.png)
![image](https://user-images.githubusercontent.com/110437548/214753967-894258ce-1e9b-4eab-8e10-3b2f4e033482.png)
위 빌딩을 왼편에서 관찰하면 4 개의 빌딩을, 오른편에서 관찰하면 2 개의 빌딩을 관찰할 수 있다.

N 개의 빌딩의 높이가 주어질 때, 왼편에서 관찰했을 때 보이는 빌딩의 개수와, 오른편에서 관찰했을 때 보이는 빌딩의 개수는 얼마일까?
### 입력   
첫째 줄에 빌딩의 개수 N (1 ≤ N ≤ 10) 이 주어진다.

둘째 줄에 정수 A[1], A[2], ... , A[N] (1 ≤ A[i] ≤ 10) 이 주어진다. A[i] 는 i 번째 빌딩의 높이이다.

### 출력 
첫째 줄에 왼편에서 관찰했을 때 보이는 빌딩의 개수와, 오른편에서 관찰했을 때 보이는 빌딩의 개수를 출력한다.





```swift
// 입력 : 
7
1 4 2 5 3 7 1

// 출력 : 
4
```
* * *
## 풀이
나는 해당 문제를 DP 알고리즘으로 해결하였다    
- Dynamic Programming은 이전 항들을 이용해 현재 항을 계산하므로 하나의 항목을 계산할 때 단 한번만 보는 특징이 있다  
- 먼저 왼쪽과 오른쪽에서 살펴볼 dp 배열 두 개를 생성하고
- target값을 지정해준다   
- for문을 돌면서 dp에 이전 항 값을 넣어주고 만약 현재 항이 target값보다 크다면 target값을 업데이트 한 후, dp값을 +1 해준다   
- 이렇게 하면 dp_L의 가장 마지막 항이 우리 구하려는 볼 수 있는 빌딩의 개수가 된다 
- 오른쪽에서 볼 경우는 for in stride를 통해 반대로 구해준다
- stride의 to인자는 파이썬과 같이 그 포함되지 않는 범위 이므로 유의해야 한다   
( -> from:5, to:0, by:-1일 경우 5,4,3,2,1 이 해당된다 )
* * *

## 코드
  
```swift
import Foundation

let N = Int(readLine()!)!
let building = readLine()!.split(separator : " ").map{Int(String($0))!}
var dp_L = Array(repeating:1, count:N)
var dp_R = Array(repeating:1, count:N)
var target = building[0]
if N==1 {
    print(1,1)
}else{
    for i in 1...N-1{
        dp_L[i] = dp_L[i-1]
        if building[i] > target{
            target = building[i]
            dp_L[i] += 1
        }
    }
    target = building[N-1]
    for i in stride(from: N-2, to:-1, by: -1){
        dp_R[i] = dp_R[i+1]
        if building[i] > target{
            target = building[i]
            dp_R[i] +=  1
        }
    }
    print(dp_L[N-1], dp_R[0])
}

```

---
layout: post
title:  "[알고리즘/Swift] 0127 제설 "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
---

## 문제
준식이는 밤새 집 앞 마당에 쌓인 눈을 치우려고 한다. 집 앞 마당은 N × N 크기의 행렬로 생각할 수 있고, 모든 칸에는 눈이 쌓여있는 상태다.

준식이는 바쁘기 때문에 (r1, c1), (r2, c2) 를 꼭짓점으로 하는 직사각형 부분만 눈을 치우려고 한다. 눈을 치우고 난 후에 마당의 상태는 어떻게 되었을까?
  

### 입력   
첫째 줄에 정수 N (1 ≤ N ≤ 10) 이 주어진다.

둘째 줄에 두 꼭짓점의 좌표 성분 r1, c1, r2, c2 (1 ≤ r1, c1, r2, c2 ≤ N)이 주어진다. (r, c) 는 행렬에서 r번째 행, c 번째 열을 의미한다.

주어진 두 꼭짓점은 직사각형에서 이웃하지 않는 두 꼭짓점이다. 즉, 직사각형의 왼쪽 위, 오른쪽 아래 좌표가 주어질 수도 있지만, 오른쪽 위, 왼쪽 아래 좌표가 주어질 수도 있다.


둘째 줄에 정수 A[1], A[2], ... , A[N] (1 ≤ A[i] ≤ 10) 이 주어진다. A[i] 는 i 번째 빌딩의 높이이다.

### 출력 
N × N 크기의 행렬을 출력한다. 눈이 쌓여 있는 칸은 * 을그렇지 않은 칸은 . 으로 출력한다.


```swift
// 입력 : 
5
2 3 5 4
// 출력 : 
*****
**..*
**..*
**..*
**..*
```
* * *
## 풀이
단순 구현 문제롤 해결할 수 있었다   
우선 NxN의 배열을 눈("\*")으로 가득 채워서 생성한다   
그 후 r1 c1 r2 c2를 입력받아 열과 행 튜플을 (작은 값, 큰 값)으로 저장했다   
항상 r1 c1이 작은 수가 아니고 오른쪽 위, 왼쪽 아래가 좌표가 주어질 수도 있기 때문에   
for문을 돌릴 때 인덱스 밖으로 나가지 않게 하기 위해서이다 
그 다음 이중 for문으로 제설한 마당만 배열에 "."을 채워주었다    
기본적인 문제였지만 다음에 복잡한 구현 문제를 풀 경우 도움이 될 수 있을 것 같았다   

## 코드
  
```swift
import Foundation

let N = Int(readLine()!)!
var field = Array(repeating : Array(repeating : "*", count : N), count : N)
let target = readLine()!.split(separator: " ").map{Int(String($0))!}
let (r1, r2) = (min(target[0],target[2])-1, max(target[0], target[2])-1)
let (c1, c2) = (min(target[1],target[3])-1, max(target[1], target[3])-1)
for i in r1...r2{
    for j in c1...c2{
        field[i][j] = "."
    }
}
for line in field{
    for snow in line{
        print(snow, terminator : "")
    }
    print()
}

```

---
layout: post
title:  "[알고리즘/Swift] 0125 세뱃돈 "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
---

## 문제
준식이와 친구들이 모여 이번 설에 받은 세뱃돈을 자랑하기 시작했다.   
준식이를 포함한 친구들은 총 N 명이고, i 번째 사람이 받은 세뱃돈은 A[i] (1 ≤ A[i] ≤ 100) 만 원이다.   
i (1 ≤ i ≤ N) 번째 사람을 기준으로 자기 자신보다 세뱃돈을 덜 받은 사람의 수와, 더 받은 사람의 수를 구해보자.   

### 입력   
첫째 줄에 준식이를 포함한 사람의 수 N (3 ≤ N ≤ 100) 이 주어진다.

둘째 줄에 준식이를 포함한 친구들이 받은 세뱃돈이 몇 만 원인지 주어진다. i 번째 사람이 받은 세뱃돈은 A[i] (1 ≤ A[i] ≤ 100) 만 원이다. 모든 사람이 받은 세뱃돈은 서로 다르다.

결과는 0 과 1 로 이루어진 길이가 4인 문자열이고, 0 이면 뒷 면을 1 이면 앞 면을 의미한다.
### 출력 
N 개의 줄에 답을 출력한다. i 번째 줄에는 i번째 사람이 자기보다 세뱃돈을 덜 받은 사람의 수와, 더 받은 사람의 수를 출력한다.  



```swift
// 입력 :
5
3 1 2 5 4

// 출력 : 
2 2
0 4
1 3
4 0
3 1
```
* * *
## 풀이
 
### sort, sorted
- sort : 원본값에 영향을 주고 리턴값이 없음 (let으로 선언 시 sort함수 사용 불가능)
- sorted : 원본값에 영향을 주지 않고, 새로운 리턴값을 반환함

입력받은 사람들의 원소에 한 명씩 접근하면서 그 사람이 정렬된 배열의 어느 위치에 해당하는 지 변수 i에 저장한다     
- 만약 i==0 이라면) 가장 세뱃돈을 조금 받은 사람이므로 **0**과 **배열의 개수 - 1**을 출력한다
- 나머지는 i가 자신의 세뱃돈 양 순위의 index를 나타내므로 자신보다 세뱃돈을 조금 받은 사람들의 수를 나타낸다 따라서, **i**와 **count-1-i**를 출력한다   

* * *

## 코드
  
```swift
import Foundation

let N = Int(readLine()!)!
let friends = readLine()!.split(separator : " ").map{Int(String($0))!}
let sortedFriends = friends.sorted()
for friend in friends {
    let i = Int(sortedFriends.firstIndex(of : friend)!)
    let count = sortedFriends.count
    if i==0 {
        print(0,count-1)
        continue
    }
    print(i,count-1-i)
}
```

---
layout: post
title:  "[알고리즘/Swift] 연속된 부분 수열의 합"
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
  - 투 포인터
  - 배열의 합
---

## 문제
비내림차순으로 정렬된 수열이 주어질 때, 다음 조건을 만족하는 부분 수열을 찾으려고 합니다.  

기존 수열에서 임의의 두 인덱스의 원소와 그 사이의 원소를 모두 포함하는 부분 수열이어야 합니다.  
부분 수열의 합은 k입니다.  
합이 k인 부분 수열이 여러 개인 경우 길이가 짧은 수열을 찾습니다.  
길이가 짧은 수열이 여러 개인 경우 앞쪽(시작 인덱스가 작은)에 나오는 수열을 찾습니다.  
수열을 나타내는 정수 배열 sequence와 부분 수열의 합을 나타내는 정수 k가 매개변수로 주어질 때, 위 조건을 만족하는 부분 수열의 시작 인덱스와 마지막 인덱스를 배열에 담아 return 하는 solution 함수를 완성해주세요. 이때 수열의 인덱스는 0부터 시작합니다.  

### 제한 사항
5 ≤ sequence의 길이 ≤ 1,000,000  
1 ≤ sequence의 원소 ≤ 1,000  
sequence는 비내림차순으로 정렬되어 있습니다.  
5 ≤ k ≤ 1,000,000,000  
k는 항상 sequence의 부분 수열로 만들 수 있는 값입니다.  

### 입출력   

|sequence|k|result|
|---|---|---|
\|[1, 2, 3, 4, 5]|7|[2, 3]|
\|[1, 1, 1, 2, 3, 4, 5]|5|[6, 6]|
\|[2, 2, 2, 2, 2]|6|[0, 2]|   
* * *
## 풀이
부분 수열의 최댓값 문제는 많이 풀어봤지만 다양한 조건에 해당하는 값을 찾는 문제는 처음이라 많이 해맸다.   
부분 수열의 조건을 정리하면 다음과 같다.  
- 합이 k
- 길이가 가장 짧은 수열
- 길이가 같다면 가장 앞의 부분 수열
- 수열 idx는 0부터
- 수열은 모두 오름차순으로 정렬되어 있음

일반적인 투 포인터 문제이다.   
모든 부분 수열들을 다 살펴줘야 하지만, 오름차순으로 정렬되어 있으므로 두 포인터를 사용하여  
sum에 뒤의 추가적인 원소를 더하고 앞의 원소를 빼면 좀 더 효율적인 코드를 짤 수 있을 거라고 판단하였다.   
알고리즘은 sum 에 배열의 첫 원소를 넣어주고, start, end 두 포인터를 0으로 초기화한다.  
길이가 가장 짧고, 길이가 같다면 가장 앞의 부분 수열을 구해야 하므로, min 변수의 값보다 배열의 길이가 작을 때만 업데이트 해주도록 하였다.  
while문을 돌면서  
- start가 배열을 다 돌 때까지 while을 돌면서
- sum이 k보다 작을 경우,
  - end가 배열의 마지막이면, while문을 중단하고, 
  - 마지막이 아닐 경우, end값을 뒤로 한 칸 이동시키고, sum값에 더해준다.
- sum이 k보다 클 경우, 
  - sum에서 start값을 빼주고, sum을 뒤로 한 칸 이동시킨다.
- sum == k 일 경우, min값과 비교하여 최소 길이의 정답 부분 수열을 업데이트 해준다.
                  이 후, end를 뒤로 한 칸 이동시키고, sum에 더해주는데, end가 마지막 원소일 경우, 연산을 해주지 않는다.  
                  

### 코드
```swift
import Foundation

func solution(_ sequence:[Int], _ k:Int) -> [Int] {
    var min = sequence.count+1
    var (start, end) = (0, 0)
    var result: [Int] = []
    var sum = sequence[0]
    while start < sequence.count {
        if sum < k {
            if end == sequence.count - 1 {
                break
            }
            end += 1
            sum += sequence[end]
        } else if sum > k {
            sum -= sequence[start]
            start += 1
        } else {
            if end - start + 1 < min {
                min = end - start + 1
                result = [start, end]
            }
            if end == sequence.count - 1 {
                break
            }
            end += 1
            sum += sequence[end]
        }
    }
   return result
}
```

---
layout: post
title:  "[알고리즘/Swift] Codility - TapeEquilibrium "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Dictionary
  - 구현
  - Codility
---

## 문제
- [Codility] <https://app.codility.com/programmers/lessons/3-time_complexity/tape_equilibrium/>   
A non-empty array A consisting of N integers is given. Array A represents numbers on a tape.

Any integer P, such that 0 < P < N, splits this tape into two non-empty parts: A[0], A[1], ..., A[P − 1] and A[P], A[P + 1], ..., A[N − 1].

The difference between the two parts is the value of: |(A[0] + A[1] + ... + A[P − 1]) − (A[P] + A[P + 1] + ... + A[N − 1])|

In other words, it is the absolute difference between the sum of the first part and the sum of the second part.

For example, consider array A such that:

  A[0] = 3
  A[1] = 1
  A[2] = 2
  A[3] = 4
  A[4] = 3
  
We can split this tape in four places:

P = 1, difference = |3 − 10| = 7
P = 2, difference = |4 − 9| = 5
P = 3, difference = |6 − 7| = 1
P = 4, difference = |10 − 3| = 7
Write a function:

public func solution(_ A : inout [Int]) -> Int

that, given a non-empty array A of N integers, returns the minimal difference that can be achieved.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [2..100,000];
each element of array A is an integer within the range [−1,000..1,000].

### 입력   

For example, given:

  A[0] = 3
  A[1] = 1
  A[2] = 2
  A[3] = 4
  A[4] = 3
  
### 출력 
the function should return 1, as explained above.


* * *
## 풀이
이 문제는 time complexity 문제라서 시간 복잡도가 어떻게 하면 작게 나오게 할 수 있을지 고민하면서 문제를 풀었다    
배열을 두 개의 배열로 나누어서 첫 번째 배열과 두 번째 배열의 차의 절대값을 비교하는데,   
배열의 몇 번째 인덱스에서 두 개의 배열로 나누어야 두 배열의 차의 절대값이 가장 작아지나 찾는 문제였다   
   
   
처음에는 배열을 i를 1부터 늘려가면서 배열의 i index를 기준으로 앞과 뒤 배열의 차를 비교하는 알고리즘을 작성하였다  
```var target = abs(sum(arr1) - sum(arr2))```    
하지만 이 알고리즘은 시간 복잡도가 O(n^2)로 문제에서 요구했던 시간보다 길게 나왔다🥲    
   
그래서 정답 코드에 있는 코드와 같이 먼저 두 개의 수에 초기값 (max1 : 배열의 첫번째 값, max2 : 나머지 배열의 총 합)을 넣어주고   
for문을 돌면서 i에 해당하는 원소를 max2에서 빼서 max1에 더해주면서 최솟값과 비교를 하였다   
그 결과 시간 복잡도가 O(n)으로 줄어들었다   

### 알게된 점
for문 안에서 배열의 메소드들을 사용할 때에는 시간 복잡도가 대폭 증가하므로 주의하여 사용하여야 한다
밑 코드에서 구현한 함수도 시간 복잡도가 O(n) 이므로 for문 밖에서 사용하도록 유의해서 사용하였다!   
```
public func sum (_ array : Array<Int>) -> Int {
    return array.reduce(0,+)
}
```
* * *   
## 코드
  
```swift
import Foundation
import Glibc

public func solution(_ A : inout [Int]) -> Int {

    var max1 = A[0]
    var max2 = sum(A) - max1
    var min = abs(max1 - max2)
    for i in 1..<A.count-1{
        max1 = max1 + A[i]
        max2 = max2 - A[i]
        var target = abs(max1-max2)
        if min >= target{
            min = target
        }
    }
    return min
}
public func sum (_ array : Array<Int>) -> Int {
    return array.reduce(0,+)
}
```

---
layout: post
title:  "[알고리즘/Swift] 0131 리포그램 "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
---

## 문제
리포그램(lipogram)은 팬그램(pangram)과 반대되는 개념으로, 알파벳의 일부 글자를 사용하지 않고 만든 문장이다. 주어진 문자열이 리포그램인지 확인해보자.

### 입력   
첫째 줄에 알파벳 소문자로 이루어진 문자열 S (1 ≤ |S| ≤ 100) 가 주어진다. S 는 공백 또는 알파벳 대소문자로 이루어진 문자열이다.


### 출력 
주어진 문자열 S 가 리포그램이라면 YES 를 출력하고 두 번째 줄에 사용하지 않은 알파벳을 출력한다. 사용하지 않은 알파벳은 소문자로 출력하며, 알파벳 순서대로 출력한다.

리포그램이 아니라면 NO 를 출력한다.



```swift
입력 : 
Bubble sort Quick sort Merge sort prefix sum Binary search Fibonacci Dijkstra
출력 : 
YES
vwz
```
* * *
## 풀이
- 입력 문자열을 Character 배열로 받는다
- 이 때 입력 문자열에 소문자, 대문자, 여백등이 혼합되어 있으므로 `lowercased()`를 통해 소문자화 하였고 `filter{$0 != " "&& $0 != "\n"}`을 통해 여백을 없애주었다  
- 모든 소문자 알파벳의 아스키코드 값이 들어있는 배열과 ([Int])   
- Character 배열의 원소들의 아스키코드를 중복되지 않게 넣을 Set을 생성해준다 (Set<Int>)   
  
```swift
import Foundation
var S : [Character] = Array(readLine()!.lowercased().filter{$0 != " " && $0 != "\n"})
var alpAscii : [Int] = Array(97...122)
var alpSet = Set<Int>()
```
  
- 이 후 for문을 돌면서 Character배열의 원소들을 Set에 넣어준다
```swift
for s in S{
    let ascii = Int(UnicodeScalar(String(s))!.value)
    alpSet.insert(ascii)
}
```
  
- for문을 다 돌았을 때 Set의 count가 26개 (소문자 알파벳의 개수)인 경우) 리포그램이 아닌 팬그램이므로 "NO"를 출력한다
- 그렇지 않은 경우) 모든 소문자 알파벳의 아스키코드 값이 들어있는 배열을 원소중에 Set에 포함되어 있지 않은 원소의 UnicodeScalar값을 출력한다   
  
```swift
if alpSet.count == 26{
    print("NO")
}else{
    print("YES")
    for i in alpAscii{
        if !(alpSet.contains(i)){
            print(UnicodeScalar(i)!, terminator:"")
        }
    }
}
```



 
### ASCII code -> String
```swift
var ascii = UnicodeScalar(65)!
//print(A)
 ```

   
  
## 코드
  
```swift
import Foundation
var S : [Character] = Array(readLine()!.lowercased().filter{$0 != " " && $0 != "\n"})
var alpAscii : [Int] = Array(97...122)
var alpSet = Set<Int>()
for s in S{
    let ascii = Int(UnicodeScalar(String(s))!.value)
    alpSet.insert(ascii)
}
if alpSet.count == 26{
    print("NO")
}else{
    print("YES")
    for i in alpAscii{
        if !(alpSet.contains(i)){
            print(UnicodeScalar(i)!, terminator:"")
        }
    }
}

```

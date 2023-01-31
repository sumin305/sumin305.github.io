---
layout: post
title:  "[알고리즘/Swift] 0130 팬그램 "
categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - Array
  - 구현
---

## 문제
팬그램(pangram)은 알파벳의 모든 글자를 사용해 만든 문장이다. 주어진 문자열이 팬그램인지 확인해보자.

### 입력   
첫째 줄에 알파벳 소문자로 이루어진 문자열 S (1 ≤ |S| ≤ 100) 가 주어진다. S 는 공백 또는 알파벳 대소문자로 이루어진 문자열이다.

### 출력 
주어진 문자열 S 가 팬그램이라면 YES 를 그렇지 않다면 NO 를 출력한다.




```swift
// 입력 : The Quick Brown Fox Jumps Over The Lazy Dog
// 출력 : YES
```
* * *
## 풀이
문자열을 입력받아 대문자화한 후, 여백들을 제외한 후 Array<Character> 형으로 만들어준다     
입력받은 문자를 아스키코드로 변환하여 아스키코드가 65~80(알파벳 대문자의 아스키코드 범위) 범위안에 있는 Character들만 alpSet에 넣어준다     
alpSet의 개수가 대문자와 알파벳의 개수(26개)와 동일한지 확인하여 팬그림인지 확인한다     
모든 알파벳이 다 사용되었나 확인할 경우에는 같은 알파벳이 중복되는 경우는 count 하면 안되므로 Set에 넣어주었다      
 
### Character -> ASCII code 
```swift
1. let ascii = s.asciiValue! 
//Character -> Ascii code 전환 (Optional 값 반환하기 때문에 강제 언래핑 해주었다)

2.let ascii = Int(UnicodeScalar(String(s))!.value) 
//UnicodeScalar는 String형만 변환해주기 때문에 Character를 String으로 형 변환 해주었다
 ```

   
  
## 코드
  
```swift
var S : [Character] = Array(readLine()!.uppercased().filter{$0 != " " && $0 != "\n"})
var alpSet = Set<Character>()
for s in S{
    //let ascii = s.asciiValue!
    let ascii = Int(UnicodeScalar(String(s))!.value)
    if ascii >= 65 &&  ascii <= 90{
        alpSet.insert(s)
        }}
if alpSet.count == 26{
    print("YES")
}else{
    print("NO")
}

```

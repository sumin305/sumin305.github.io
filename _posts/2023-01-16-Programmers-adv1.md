---
layout: post
title:  "[알고리즘/Swift] 0116 준식이의 모험1"

categories : Algorithm
  
tags:
  - 프로그래머스
  - Problem Solving
  - 배열
  - 문자열
  - 구현
---

## 문제
준식이의 모엄 은 준식이가 2차원 N × M 격자 세상에서 떠나는 모험을 주제로 한 게임이다. 당신은 이 게임의 개발자가 되었다.
준식이는 2차원 N × M 격자 세상의 어딘가에 위치해 있다. 플레이어는 L , R , U , D 입력을 통해 준식이를 왼쪽, 오른쪽, 위, 아래 칸으로 이동시킬 수 있다. 하지만 만약 이동하려는 칸이 격자 바깥이거나, 벽인 경우 준식이는 플레이어의 입력을 무시하고 제자리에서 이동하지 않는다.
2차원 N × M 격자 세상과 플레이어의 입력이 주어질 때, 플레이어의 모든 입력이 끝난 후의 준식이의 위치는 어디일까?
### 입력
첫째 줄에 테스트 케이스의 개수 T가 주어진다.   
각 테스트 케이스의 첫째 줄에 2차원 격자 세상의 크기 N, M (1 ≤ N, M ≤ 8) 이 주어진다. N 은 격자의 세로 길이이고, M 은 가로 길이이다.   
둘째 줄부터 N 개의 줄에 2차원 N × M 격자 세상 S 가 주어진다.   
S 에서 @ 는 단 한 번 등장하며, 준식이를 의미한다.   
나머지 위치의 문자는 . 이거나 # 이다. . 은 빈 칸을 의미한다. 준식이는 빈 칸으로 자유롭게 이동할 수 있다. # 은 벽을 의미한다. 준식이는 벽이 있는 칸으로 이동할 수 없다.   
셋째 줄에 플레이어의 입력의 길이 K (1 ≤ K ≤ 100) 이 주어진다.   
넷째 줄에 플레이어의 입력 T 가 주어진다. T 는 길이가 K 인 문자열이며 L , R , U , D 만으로 구성되어 있다. 플레이어의 입력은 입력된 순서대로 주어지며, 준식이를 격자 바깥이나 벽으로 이동하게 하는 입력이 있을 수 있다.   

### 출력   
각 테스트 케이스마다 플레이어의 모든 입력이 끝난 후의 준식이의 위치를 의미하는 두 정수 R, C (1 ≤ R, C ≤ 8) 를 출력한다. R 은 격자에서 행 번호를 의미하고 C 는 열 번호를 의미한다.   
```
 123
1...
2...
3.@.
```

예를 들어 위 그림에서 준식이는 (3, 2) 에 위치하고 있다.
* * *
## 풀이
MxN 크기의 격자 세상을 빈칸으로 둘러 싸기 위해 (N+2)x(M+2) 배열을 구현하였다
```swift
var array : [[Character]] = Array(repeating: Array(repeating: " ", count: M+2), count: N+2)
```
그 다음 준식이의 위치를 파악한 후, 입력받는 문자열의 문자("L","R","D","U")에 하나씩 접근하여   
이동할 수 있는 경우(격자 바깥이나 벽으로 이동하지 않는 경우 = "."으로 이동하는 경우)에만 해당 방향으로 이동해주었다.

### 문제해결
- 준식이가 초기에 있던 자리로 돌아오는 경우를 처리해주지 않아서 오류 발생 -> 준식이의 초기 자리에 "."를 넣어줌으로써 이동할 수 있는 공간으로 바꾸어줌.
```
array[jc][jr] = "."
```   

### 사용한 문법
- 2차원 배열에서 특정 배열의 값 바꾸기
```
 array[i][1...M] = ArraySlice(S)
```
** Character 배열을 slicing 한 값(ArraySlice<Character>)에 접근하기 위해서 Array<Character> 앞에 ArraySlice를 붙여 형 변환을 해주었다 **
  
- switch-case 문
```swift
 switch (T[i]) {
            case "L" :
                if array[jc][jr-1] == "."{
                    jr -= 1
                }
            case "R" :
                if array[jc][jr+1] == "."{
                    jr += 1
                }
            case "U" : 
                if array[jc-1][jr] == "."{
                    jc -= 1
                }
            case "D" : 
                if array[jc+1][jr] == "."{
                    jc += 1
                }
            default :
                break
        }
```
** T[i] 값에 따른 처리를 다르게 해주었다   
  swift 에서는 switch-case 문에 각 case 마다 break 함수가 내장되어 있어서 저절로 종료된다   
  default 값을 꼭 붙여주어야 한다**
  
  

* * *

## 코드
  
```swift
import Foundation
for _ in 1...Int(readLine()!)! {
    let n = readLine()!.split(separator:" ").map{Int(String($0))!}
    let (N, M) = (n[0], n[1]) 
    //배열을 빈칸으로 둘러싸기 위해 (N+2)x(M+2) 배열을 구현
    var array : [[Character]] = Array(repeating: Array(repeating: " ", count: M+2), count: N+2)
    var junsik = (0,0)
  
    // 배열에 해당 값들을 넣어준다
    for i in 1...N{
        let S = Array(readLine()!)
        array[i][1...M] = ArraySlice(S)
        if S.contains("@"){
            //준식이("@")의 위치 좌표 
            junsik = (i,  S.index(of:"@")!+1) 
        }       
    }
  
    var (jc,jr) = (junsik)
    array[jc][jr] = "."
    let K = Int(readLine()!)!
    let T = Array(readLine()!)
    for i in 0...K-1{
        switch (T[i]) {
            case "L" :
                if array[jc][jr-1] == "."{
                    jr -= 1
                }
            case "R" :
                if array[jc][jr+1] == "."{
                    jr += 1
                }
            case "U" : 
                if array[jc-1][jr] == "."{
                    jc -= 1
                }
            case "D" : 
                if array[jc+1][jr] == "."{
                    jc += 1
                }
            default :
                break
        }
    }
    print(jc,jr)
}
```

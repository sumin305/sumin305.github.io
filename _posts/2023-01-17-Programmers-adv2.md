---
layout: post
title:  "[알고리즘/Swift] 0117 준식이의 모험2"

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
게임을 좀 더 흥미진진하게 만들기 위해서 격자 세상에 가시 함정(^)이 추가되었다. 준식이는 가시 함정이 위치한 칸으로 이동할 수 있지만 함정을 밟아서 데미지를 1만큼 입게 된다. 함정은 밟더라도 사라지지 않는다. 만약 함정 위에서 입력받은 방향으로 움직일 수 없어 제자리에 멈추는 경우, 해당 함정을 또 밟게 되고 데미지를 1만큼 또 입게 된다.
예를 들어 다음과 같은 격자 세상에서 플레이어가 RDUUR 을 입력했을 때 게임이 어떻게 진행되는지 보자.
![image](https://user-images.githubusercontent.com/110437548/212865117-a1841def-778d-458d-aeac-caa5236bc863.png)
위 그림에서 준식이는 1, 3, 4번째에 가시를 밟게 되고, 총 3의 데미지를 입게 된다.
2차원 N × M 격자 세상과 플레이어의 입력이 주어질 때, 플레이어의 모든 입력이 끝난 후의 준식이의 위치는 어디일까? 또, 가시 함정을 밟아서 입은 데미지는 얼마일까?
이 문제에서 준식이의 체력은 무한하기 때문에 아무리 데미지를 입어도 쓰러지지 않고 꿋꿋이 이동한다.   

### 입력
첫째 줄에 테스트 케이스의 개수 T가 주어진다.   
각 테스트 케이스의 첫째 줄에 2차원 격자 세상의 크기 N, M (1 ≤ N, M ≤ 8) 이 주어진다. N 은 격자의 세로 길이이고, M 은 가로 길이이다.   
둘째 줄부터 N 개의 줄에 2차원 N × M 격자 세상 S 가 주어진다.   
S 에서 @ 는 단 한 번 등장하며, 준식이를 의미한다.   
S 에서 ^ 는 여러 번 등장할 수 있으며, 가시 함정을 의미한다.   
나머지 위치의 문자는 . 이거나 # 이다. . 은 빈 칸을 의미한다. 준식이는 빈 칸으로 자유롭게 이동할 수 있다. # 은 벽을 의미한다. 준식이는 벽이 있는 칸으로 이동할 수 없다.   
셋째 줄에 플레이어의 입력의 길이 K (1 ≤ K ≤ 100) 이 주어진다.   
넷째 줄에 플레이어의 입력 T 가 주어진다. T 는 길이가 K 인 문자열이며 L , R , U , D 만으로 구성되어 있다. 플레이어의 입력은 입력된 순서대로 주어지며, 준식이를 격자 바깥이나 벽으로 이동하게 하는 입력이 있을 수 있다.   

### 출력   
각 테스트 케이스마다 플레이어의 모든 입력이 끝난 후의 준식이의 위치를 의미하는 두 정수 R, C (1 ≤ R, C ≤ 8) 를 출력한다. R 은 격자에서 행 번호를 의미하고 C 는 열 번호를 의미한다. 또한 준식이가 가시 함정을 밟아서 입은 데미지를 출력한다.   
예를 들어 위 그림에서 준식이는 (3, 2) 에 위치하고 있다.   

<img width="121" alt="image" src="https://user-images.githubusercontent.com/110437548/212865578-f48a83c3-2d6b-41f7-b641-7000c0927be1.png">   

* * *
## 풀이
이전에 풀었던 준식이의 모험1 풀이에 약간의 변수만 변경 및 추가해주면 해결되는 문제였다   
[준식이의 모험1 문제]<https://sumin305.github.io/2023/01/16/Programmers-adv1>
- 변수 정리해주기
`(jc,jr) = (i,input.index(of : "@")!+1)`

- 데미지 변수 추가 
```swift
var damage = 0
if array[jc][jr] == "^"
        {
            damage += 1
        }
```
### tuple
```swift
let n = readLine()!.split(separator : " ").map{Int(String($0))!}
let (N,M) = (n[0],n[1])
```    
swift로 알고리즘 풀이를 하다보면 위에서와 같이 ( )로 변수를 감싼 형태를 자주 사용하게 되는데 
정확히 정의를 모르고 사용하고 있어서 인터넷에 찾아보았다   

> tuple? 매우 간단한 struct이다
- 여러 가지 타입을 한꺼번에 묶어서 사용할 수 있다 (또 다른 튜플이나, 함수까지도 가능)
``` 
var tuple = (1, "hello",true)
var anotherTuple = (3, tuple, sayhi())
```
- 튜플의 있는 값에 접근하려면 ? 
  - 튜플이름.인덱스
  - 혹은 이름 지정해주기
```swift
var tuple = (1, "hello",true)
print(tuple.1) //1 출력
var namedTuple (age : 2, name: "lee") 
print(namedTuple.age) //2 출력
```
- tuple은 임시로 값들을 그룹 지을 때만 사용한다
- tuple은 for문을 돌릴 수 없다   

* * *

## 코드
  
```swift
import Foundation

for _ in 1...Int(readLine()!)! {
    let n = readLine()!.split(separator : " ").map{Int(String($0))!}
    let (N,M) = (n[0],n[1])
    var array : [[Character]] = Array(repeating : Array(repeating : " ", count : M+2), count : (N+2))
    var (jc,jr) = (0,0)
    var damage = 0
    for i in 1...N {
        let input = Array(readLine()!)
        array[i][1...M] = ArraySlice(input)
        if input.contains("@"){
            (jc,jr) = (i,input.index(of : "@")!+1)
        }
    }
    array[jc][jr] = "."
    let K = Int(readLine()!)!
    let T = Array(readLine()!)

    for i in 0...K-1{
        switch (T[i]) {
            case "L" : 
                if array[jc][jr-1] != " " && array[jc][jr-1] != "#"{
                    jr -= 1
                }
            case "R" : 
                if array[jc][jr+1] != " " && array[jc][jr+1] != "#"{
                    jr += 1
                }
            case "U" : 
                if array[jc-1][jr] != " " && array[jc-1][jr] != "#"{
                    jc -= 1
                }
            case "D" : 
                if array[jc+1][jr] != " " && array[jc+1][jr] != "#"{
                    jc += 1
                }
            default: 
                break
        }
        if array[jc][jr] == "^"
        {
            damage += 1
        }
    }
    print(jc,jr,damage)
}
```

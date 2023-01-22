---

layout: post
title : "[Swift] Time Complexity"
categories : Swift
tags : 
    - Swift
---

## Time Complexity
알고리즘 문제를 풀 경우나 앱을 개발할 때에도 시간 복잡도 고려는 매우 중요하다   
코드의 위치만 바꾸어 주더라도 코드의 시간 복잡도를 대폭 줄일 수도 있다   
또한 같은 결과를 출력하는 함수로 대체해주는 것도 시간복잡도를 줄이는 좋은 방법이다   
한번 정리해놓으면 잊지 않을 것 같아 포스팅해보기로 하였다

### Array   

| method | time complexity|
|:--:|:--:|
|append(_ newElement: Element)| O(1) |
|append(contensOf:)|O(M) (M은 삽입하려는 Elements의 개수)|
|insert(_ newElement: Element)|O(N) (가장 마지막에 삽입할 경우 O(1))|
|count|O(1)|
|randomElement()|O(1)|
|reversed|O(N)|
|last|O(1)|
|isEmpty|O(1)|
|popLast(), removeLast()|O(1)|
|remove(at:)|O(N)|
|removeFirst()|O(N)|
|removeAll(keepingCapacity:)|O(N)|
|contains(_ :), contains(where:)|O(N)|
|allSatisfy(_ :)|O(N)|
|first(where:),firstIndex(where:)|O(N)|
|last(where:),lastIndex(where:)|O(N)|
|firstIndex(of:), lastIndex(of:)|O(N)|
|min(), max()|O(N)|
|enumerate()|O(N)|
|sort(), sorted()|O(NlogN)|
|reverse(), reversed()|O(N)|
|shuffle(), shuffled()|O(N)|   

   
### Set

| method | time complexity|
|:--:|:--:|
|count|O(1)|
|contains(_ :)|O(1)|
|contains(where:)|O(N)|
|removeFirst()|O(1)|
|firstIndex(of:)|O(1)|   


### Dictionary

| method | time complexity |  
|:--:|:--:|
|count|O(1)
|contains(where:)|O(N)|
|index(forKey:)|O(1)|
|remove(at:), removeValue(forKey:), removeAll(keepingCapacity:)|O(N)|
|popFirst()|O(1)|
|reversed()|O(N)|   

## 알게된 점
Array에서 removeLast()는 시간복잡도가 O(1)이지만, removeFirst()는 O(N)이다   
배열은 순서와 index가 있는 컬렉션 타입이므로, removeLast()를 한 후 처리해줄 필요가 없지만     
removeFirst()를 한 후에는 추가로 index를 1씩 줄여주는 작업이 필요하다   
이에 반해 Set은 순서가 따로 없으므로 removeFirst()의 시간복잡도가 O(1)이다  
Array와 Dinctionary는 시간복잡도가 유사한 함수들이 많다고 느꼈다   

## 참고
[set](github.com/apple/swift/blob/main/stdlib/public/core/Set.swift)   

[array](github.com/apple/swift/blob/main/stdlib/public/core/Array.swift)   

[dictionary](github.com/apple/swift/blob/main/stdlib/public/core/Dictionary.swift)





# Optional

> Optional Type
> Expression : ?

- 기본 구조

```swift
var myString: String? // Optional
var myString2: String = String() // Non-Optional 반드시 이니셜라이저 해야함. 값으로서 존재는 함
```

**nil** : 아무것도 없고 어떤 상태인지 모르는 전혀 존재하지 않는 상태

- 네트워크로 데이터를 받을 때 **Optional** 사용
- 값이 있는 상태를 가정해서 사용할 수 없음
- 값이 무조건 있다는것을 가정하는 방식인 Force Unwrapping : !
  - 그러나 Force Unwrapping은 지양하는 것이 좋다.
  
안전하게 옵셔널을 제거하기 위해서 Optional Binding을 사용한다.

### Optional Binding
옵셔널 바인딩은 nil인지, 아니면 값이 있는지, 경우에 따라 결과를 달리 하고 싶을 수 있을 때 사용해 옵셔널을 검사한다.

```swift
if let hasScore = myScore {
  if hasScore < 50 {
      // 값이 있을때만 적용됨
   }
} else {

}
```

### Optional Chaining
옵셔널 체이닝은 하위 property에 optional 값이 있는지 연속적으로 확인하면서, 중간에 하나라도 nil이 발견된다면 nil이 반환되는 형식이다.

```swift
class People {
    var score: Score?
}

class Score {
    var testName: String?
}

var people: People? = People()
if let hasName =  people?.score?.testName {
    if hasName == "eojin" {
    
    }
}
```

### Nil Coalescing Operator
값을 변경하지 않은 채, nil인경우 예외적으로 특정 값으로 표현된다.
```swift
var myScore: Int?
if myScore ?? 0 < 50 {
    print("number")
}
```

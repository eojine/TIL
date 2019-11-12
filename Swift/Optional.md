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
- 값이 무조건 있다는것을 가정하는 방식 [!] : 강제 언래핑

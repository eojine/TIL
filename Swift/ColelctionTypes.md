# Colelction Types

> Array, Set, Dictionary

</br>

## Array

```swift
var someStrings = [String]()

someStrings.append("a")
someStrings.append("b")
someStrings.append("c")
someStrings.count // 3

var removeIndex = 2
if someStrings.count - 1 >= removeIndex {
    someStrings.remove(at: removeIndex)
}
```

</br>

## Set

>  순서가 없고 나열되어있는 구조, 중복을 허용하지 않음.

```swift
// Hashable = String, Int, Double, Bool

var strings1 = Set<String>()
var strings2 = Set<String>()

strings1.insert("a")
strings1.insert("b")
strings1.insert("c")

strings2.insert("c")
strings2.insert("d")
strings2.insert("f")

strings1.union(strings2) // 합집합
strings1.intersection(strings2) // 교집합
strings1.symmetricDifference(strings2) // 교집합 제외 나머지
strings1.subtracting(strings2) // 차집합
```

</br>

## Dictionary 

>  순서가 없음 [Key : Value]

```swift
var sports = [String : Any]()

sports["soccer"] = "korea"
sports["baseball"] = 3
sports["football"] = true
sports["baseball"] = "doosan"

if let hasSoccer = sports["baseball"] {
    //print(hasSoccer) // 결과 : doosan
}
```


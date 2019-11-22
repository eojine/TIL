# ARC

> 애플의 자동 메모리 관리 기능</br>
> 강한 참조 계수가 0이 되면 메모리를 자동으로 해제시켜줌. </br>
> 그런데, 강한 참조 사이클이 일어날 때를 주의해야함.

</br>

```swift
class Person {
    var name = "eojine"
  
    deinit { // 소멸자
        print("Person Deinit")
    }
}

var person1: Person?
var person2: Person?
var person3: Person?

person1 = Person()
person2 = person1
person3 = person1
```

이때 Person Instance의 참조 계수는 3이 됨

어떤 Instance를 참조할 때 retain메소드가 호출된다고 했는데, 이는 ARC구조를 통해 컴파일러가 자동으로 계산해서 호출됨.

nil을 넣는것은 소유권을 포기하는것을 의미함. 강한참조가 release됨.

```swift
person1 = nil
person2 = nil
```

Person Instance의 참조 계수는 1이 됨

```swift
person3 = nil
```

마지막 사용자가 소유권을 포기하여 챰조 계수가 0이 된다. 그리고 소멸자가 호출됨.

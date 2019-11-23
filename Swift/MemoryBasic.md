# Memory Basic

## ARC

> 애플의 자동 메모리 관리 기능</br>
>
> 강한 참조 계수가 0이 되면 메모리를 자동으로 해제시켜줌. </br>
>
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



## Strong Reference Cycle

> 강한 참조 사이클이 발생하는 원인과 해결 방법에 대해 공부함.

</br>

```swift
class Person {
    var name = "eojine"
    var car: Car?
    deinit { // 소멸자
        print("Person Deinit")
    }
}

class Car {
    var model: String?
    weak var lessee: Person? // Person인스턴스를 참조하지만 소유하지 않음
    
    init(model: String) {
        self.model = model
    }
    
    deinit {
        print("Car Deinit")
    }
}
```

Person도 Car의 속성을 가지고 있고, Car도 Person의 속성을 가지고 있음

```swift
var person: Person? = Person()
var rentedCar: Car? = Car(model: "Porsche")
```

이때, Person Instance 참조계수 : 1, Car Instance 참조계수 : 1

```swift
person?.car = rentedCar
```

person의 car속성이 rentedCar변수가 저장된 인스턴스의 참조 계수가 1 증가하여 Car Instance 참조계수가 2가 됨.

```swift
rentedCar?.lessee = person
```

rentedCar의 lessee속성이 person변수가 저장된 인스턴스의 참조 계수를 증가시킴.

```swift
person = nil // 참조 카운트가 1 감소하여 Person Instance의 계수는 1
rentedCar = nil // 참조 카운트가 1 감소하여 Car Instance의 계수는 1
```

모두 `nil`을 넣었지만, 아직 참조 계수는 1이다.

정상적으로 메모리 해제할 수 없음

이를 **강한 참조 사이클**이라고 한다.



### 결론

**ARC**는 메모리 관리를 대신 처리하지만, **참조 사이클**까지 자동으로 처리하지 못함.
강한 참조 사이클은 **약한 참조와 비소유 참조**를 사용하여 문제를 해결할 수 있음

- weak, unowned 참조 : 참조 카운트를 증가, 감소시키지 않고 접근만 가능함.
- weak 참조는 참조하고 있는 인스턴스가 해제되면 자동으로 nil로 초기화됨.
- unowned 참조는 옵셔널이 아닌 경우 사용.

</br>

1. 두 인스턴스가 서로를 참조 할 경우 강한참조사이클 문제가 발생한다.
2. 인스턴스가 정상적으로 해제되지 않는다.
3. weak, unowned 참조를 사용해서 해제한다.
   - 클로저에서도 인스턴스를 정상적으로 해제시키기 위해 weak, unowned 참조를 사용한다.

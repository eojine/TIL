# Functions

> 함수 - 기능

### Parameter

> 매개변수 - 함수와 메서드 입력 변수(Variable) 명

```swift
func sum(num1: Int, num2: Int) -> Int {
    return num1 + num2
}
```

</br>

### Argument

> 전달인자 -  함수와 메서드의 입력 값(Value)

```swift
var sumResult = sum(num1: 10, num2: 23)
```

</br>

#### 파라미터 이름 없이 값을 받을 수 있는 구조

```swift
func sum2(_ num1: Int, _ num2: Int) -> Int {
    return num1 + num2
}

sum2(1, 2)
```

</br>

#### 여러개 값을 받을 수 있는 구조

```swift
func sumAll(numbers: Int...) -> Int {
    // for in
    var total = 0
    for number in numbers {
        total += number
    }
	return total
}

sumAll(numbers: 4, 5, 6, 10, 100)
```

</br>

#### 리턴 스타일을 여러개 만들 수 있는 구조

```swift
func sum3(_ num1: Int, _ num2: Int) -> (Int, Int, result: Int) {
    return (num1, num2, num1 + num2)
}

var mySum = sum3(20, 40)

print(mySum.0)
print(mySum.1)
print(mySum.result)
```

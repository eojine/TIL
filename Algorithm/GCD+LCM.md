## 최대공약수
```swift
func GCD(_ min: Int, _ max: Int) -> Int {
    let remain = min % max
    if remain == 0 {
        return max
    } else {
        return GCD(max, remain)
    }
}
```

## 최소공배수
```swift
func LCM(_ a: Int, _ b:Int) -> Int {
    return a * b / GCD(a,b)
}
```

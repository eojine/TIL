# KeyChain

### UserDefaults

`UserDefaults`에도 데이터를 쉽게 저장할 수 있지만, 단순히 .info 파일에 키-값 쌍을 텍스트 형태로 저장하기 때문에 OS를 탈옥하면 내용물을 볼 수 있다는 문제가 있다.

보안이 필요한 데이터에는 적합하지 않다.

### KeyChain

이를 방지하기 위해 암호나 API Token과 같은 민감한 정보는 `KeyChain`에 저장하는 것이 좋다.

## KeyChain이란~~?

> Apple이 제공하는 보안 프레임워크

Keychain은 디바이스 안에 암호화된 데이터 저장 공간을 의미한다. 사용자는 암호화된 공간에 데이터를 안전하게 보관할 수 있다.

### 무엇을 저장할까?

- 로그인 및 암호 (해시)
- 결제 데이터
- 알고리즘 암호화를위한 키

등등... 단순한 구조의 비밀을 유지하고 싶은 것들을 저장

### KeyChain 특징

- 사용자가 직접 제거하지 않는 이상, 앱을 제거하고 설치해도 데이터는 남아있다.
- 디바이스를 Lock하면 KeyChain도 잠기고, 디바이스를 UnLock하면 KeyChain역시 풀린다.
    - `KeyChain`이 잠긴 상태에서는 Item들에 접근할수도, 복호화 할 수도 없다.
    - `KeyChain`이 풀린 상태에서도 해당 Item을 생성하고 저장한 어플리케이션에서만 접근이 가능하다.
- 같은 개발자가 개발한 여러 앱에서 키체인 정보를 공유할 수 있다.

## KeyChain Service API

KeyChain service API로 민감한 데이터를 암호화 - 복호화 하며 재사용 하는 행위를 보다 쉽고 안전하게 사용할 수 있게한다.

<img src="https://user-images.githubusercontent.com/26567846/101286161-5888aa00-382c-11eb-8513-f26620d697b6.png" width="50%">


### KeyChain Items

키체인에 저장된 Data 단위. 키체인은 하나 이상의 Keychain item을 가질 수 있다.

<img src="https://user-images.githubusercontent.com/26567846/101286165-5d4d5e00-382c-11eb-8c74-d0d34ea89774.png" width="50%">

### KeyChain Item Class

저장할 Data의 종류.

- **kSecClassInternetPassword**
- **kSecClassCertificate**
- **kSecClassGenericPassword**
- **kSecClassIdentity**
- **kSecClassKey**

대표적으로 인터넷용 아이디/패스워드르 저장할 때 사용하는 kSecClassInernetPassword,

인증서를 저장할 때 사용하는 kSecClassCertificate, 

일반 비밀번호를 저장할 때 사용하는 kSecClassGenericPassword등이 있다.

### Attributes

keychain item을 설명하는 특성. item class에 따라 설정할 수 있는 attributes가 달라진다.

애플 문서를 보면 아래와 같이 각 아이템 클래스별로 다양한 어트리뷰트가 존재한다.

<img src="https://user-images.githubusercontent.com/26567846/101286089-f7f96d00-382b-11eb-9956-ad3b3b2a20fb.png" width="50%">

---

# 구현해보기

> Token구조체 배열을 KeyChain으로 저장하자!

일반 비밀번호를 저장할 때 사용하는 **kSecClassGenericPassword** 라는 **Keychain Item Class** 를 사용하여 TOTP토큰배열을 저장하는 과정에 대해 살펴보도록 하겠다.

### KeyChain Query 작성하기

```swift
----------------------------------------------------------------
kSecClass: 키체인 아이템 클래스 타입,
kSecAttrService: 서비스 아이디, // 앱 번들 아이디
kSecAttrAccount: 저장할 아이템의 계정 이름,
kSecAttrGeneric: 저장할 아이템의 데이터
----------------------------------------------------------------

private let account = "TokenService"
private let service = Bundle.main.bundleIdentifier

let query: [CFString: Any] = [kSecClass: kSecClassGenericPassword,
                              kSecAttrService: service,
                              kSecAttrAccount: account,
                              kSecAttrGeneric: data]
```

### API 메소드

**Create** 

- `SecItemAdd(_:_:)`
- 키체인에 하나 이상의 항목을 추가할 때 사용

```swift
func createTokens(_ token: [Token]) -> Bool {
    guard let data = try? JSONEncoder().encode(token),
          let service = self.service else { return false }
     
    let query: [CFString: Any] = [kSecClass: kSecClassGenericPassword,
                                  kSecAttrService: service,
                                  kSecAttrAccount: account,
                                  kSecAttrGeneric: data]

    return SecItemAdd(query as CFDictionary, nil) == errSecSuccess
}
```

**Read** 

- `SecItemCopyMatching(_:_:)`
- 검색 쿼리와 일치하는 키체인 항목을 하나 이상 반환하는 기능. 또한, 특정 키 체인 항목의 속성을 복사할 수 있다.

```swift
func readTokens() -> [Token]? {
    guard let service = self.service else { return nil }
    let query: [CFString: Any] = [kSecClass: kSecClassGenericPassword,
                                  kSecAttrService: service,
                                  kSecAttrAccount: account,
                                  kSecMatchLimit: kSecMatchLimitOne,
                                  kSecReturnAttributes: true,
                                  kSecReturnData: true]
    
    var item: CFTypeRef?  // 아직 잘 모르겠다!
    if SecItemCopyMatching(query as CFDictionary, &item) != errSecSuccess { return nil }
    
    guard let existingItem = item as? [String: Any],
          let data = existingItem[kSecAttrGeneric as String] as? Data,
          let tokens = try? JSONDecoder().decode([TOTPToken].self, from: data) else { return nil }
    
    return tokens
}
```

**Update**

- `SecItemUpdate(_:_:)`
- 검색 쿼리와 일치하는 항목을 수정할 수 있는 기능

```swift
func updateTokens(_ token: [Token]) -> Bool {
    guard let query = self.query,
          let data = try? JSONEncoder().encode(token) else { return false }
    
    let attributes: [CFString: Any] = [kSecAttrAccount: account,
                                       kSecAttrGeneric: data]
    
    return SecItemUpdate(query as CFDictionary, attributes as CFDictionary) == errSecSuccess
}
```

**Delete**

- `SecItemDelete(_:)`
- 검색 쿼리와 일치하는 항목을 제거하는 기능

```swift
func deleteTokens() -> Bool {
    guard let query = self.query else { return false }
    return SecItemDelete(query as CFDictionary) == errSecSuccess
}
```

### Keychain Wrappers

기본적으로 제공되는 `Keychain Services API`도 존재하지만 이를 보다 편하고 안전하게 이용할 수 있게끔 해주는 라이브러리들이 존재한다.

1. [SwiftKeychainWrapper](https://cocoapods.org/pods/SwiftKeychainWrapper%20or%20https://github.com/jrendel/SwiftKeychainWrapper)
2. [SAMKeychain](https://cocoapods.org/pods/SAMKeychain) - (Obj - C)
3. [LockSmith](https://github.com/matthewpalmer/Locksmith)

[](https://medium.com/@justfaceit/ios-%EC%95%B1-%EB%B6%80%ED%92%88-%EB%A7%8C%EB%93%A4%EA%B8%B0-1-preferencestorage-%EC%84%A4%EC%A0%95-%EC%A0%80%EC%9E%A5%EC%9D%84-%EC%9C%84%ED%95%9C-%EA%B3%B5%ED%86%B5-%ED%81%B4%EB%9E%98%EC%8A%A4-2eb2c27af941)


 <br>

[참고1](https://kor45cw.tistory.com/285)
[참고2](https://beankhan.tistory.com/m/109)
[참고3](https://kka7.tistory.com/m/93)
[참고4](https://baked-corn.tistory.com/69)
[참고5](https://daeun28.github.io/ios%EC%82%AC%EC%9A%A9%EB%B2%95/post21/)

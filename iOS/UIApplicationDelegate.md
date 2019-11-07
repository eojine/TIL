# UIApplicationDelegate
AppDelegate.swift에는 아래와 같이 앱의 상태에 따라 실행되는 delegate 함수들이 정의되어 있기때문에 함수안에 코드를 작성 함으로써 앱의 특정 상태에서 동작하는 로직을 구현 할 수 있다.

- **application(_:didFinishLaunching:)**
  -  앱이 처음 시작될 때 실행
- **applicationWillResignActive:** 
  - 앱이 active 에서 inactive로 이동될 때 실행
  - 앱이 Active 상태를 벗어날 것을 알려준다. (ex. 홈 버튼을 누르면 호출됨)
  - 앱이 Background 상태로 전이된다는 보장은 없다. 이 상황에 잠깐 머물다가 다시 바로 Active 상태로 바뀔 수도 있다.
- **applicationDidEnterBackground:** 
  - 앱이 background 상태일 때 실행
- **applicationWillEnterForeground:** 
  - 앱이 background에서 foreground로 이동 될때 실행 (아직 foreground에서 실행중이진 않음)
- **applicationDidBecomeActive:** 
  - 앱이 active상태가 되어 실행 중일 때
- **applicationWillTerminate:**
  - 앱이 종료될 때 실행

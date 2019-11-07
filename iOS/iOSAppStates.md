# iOS App States

* **Not Running**
  * 앱이 실행되기 전 상태, 또는 실행이 되었지만 시스템에 의해 종료된 상태
* **Inactive**
  * Foreground 로 진입은 했으나 이벤트는 아직 받지 못한 상태. iOS 앱은 다른 두 상태 사이에서 상태변환을 할 때 중간  단계로  짧은 시간동안 InActive 에 머물게 된다.
* **Active**
  * 앱이 화면에 보이고 이벤트도 받으며 실행되는 정상 상태. 일반적인 앱의 실행상태이다.
* **Background**
  * 대부분의 앱은 홈버튼을 눌렀을 때 Suspended  상태가 되기 전 잠시 Background 상태에 머문다. 이 상태에서 앱은 일정 시간 일부 코드를 수행할 수 있지만 사용자의 이벤트를 받을 수는 없다. Background에서 추가적인 코드를 수행할 필요가 있는 경우 Background 상태로 남아 일정 시간동안 코드를 수행할 수 있다. 앱이 실행되자마자 Background 로 진입하는 앱은 Inactive 상태를 거치지 않고 바로 Background  상태로 진입한다.
* **Suspended**
  * 앱 실행이 중단된 상태. Suspended 상태에서는 앱은 메모리에 존재하지만 아무런 코드를 수행하지 않는다. System은 앱을 아무런 통보 없이 Background상태에서 자동으로 Suspended 상태로 바꿀 수 있다. 메모리 부족현상이 발생하면 System은 별다른 통보 없이 Suspended 상태의 앱을 종료할 수 있다.

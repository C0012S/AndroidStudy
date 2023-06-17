## Service

Service는 백그라운드 작업을 위한 애플리케이션 **구성 요소**

ex) 

음악을 재생하거나, 파일 입출력을 수행하거나, 네트워크 트랜잭션을 차리할 수 있다.

전화 앱을 켜놓지 않은 상태에서도 전화를 받을 수 있는 것

## 서비스 세 가지 유형

### 포그라운드

포그라운드 서비스는 사용자에게 잘 보이는 몇몇 작업을 수행합니다. 예를 들어 오디오 앱이라면 오디오 트랙을 재생할 때 포그라운드 서비스를 사용합니다. 포그라운드 서비스는 [알림](https://developer.android.com/guide/topics/ui/notifiers/notifications?hl=ko)을 표시해야 합니다. 포그라운드 서비스는 사용자가 앱과 상호작용하지 않을 때도 계속 실행됩니다.

### 백그라운드

백그라운드 서비스는 사용자에게 직접 보이지 않는 작업을 수행합니다. 예컨대 어느 앱이 저장소를 압축하는 데 서비스를 사용했다면 이것은 대개 백그라운드 서비스입니다.

### 바인드

애플리케이션 구성 요소가 `[bindService()](https://developer.android.com/reference/android/content/Context?hl=ko#bindService(android.content.Intent,%20android.content.ServiceConnection,%20int))`를 호출하여 해당 서비스에 바인딩되면 서비스가 *바인딩*됩니다. 바인딩된 서비스는 클라이언트-서버 인터페이스를 제공하여 구성 요소가 서비스와 상호작용하게 하며, 결과를 받을 수도 있고 심지어 이와 같은 작업을 여러 프로세스에 걸쳐 프로세스 간 통신(IPC)으로 수행할 수도 있습니다. 바인딩된 서비스는 또 다른 애플리케이션 구성 요소가 이에 바인딩되어 있는 경우에만 실행됩니다. 여러 개의 구성 요소가 서비스에 한꺼번에 바인딩될 수 있지만, 이 모든 것에서 바인딩이 해제되면 해당 서비스는 소멸됩니다.

## 수명주기

Service를 실행시킬 수 있는 방법은 2가지로, `startService()` 또는 `bindService()`를 호출하는 방법이 있습니다. 호출되는 방법에 따라 `시작된 서비스`, 그리고 `바인딩된 서비스` 로 구분해서 볼 수 있다.

- 단, 모든 서비스는 클라이언트와 바인딩될 수 있다. 즉, `startService()`로 실행된 서비스라도 `bindService()`가 호출되어 다른 클라이언트에 바인딩 될 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14355a7d-55af-4ba3-b200-673dd53944b3/Untitled.png)

- 서비스의 **전체 수명**은 `[onCreate()](https://developer.android.com/reference/android/app/Service?hl=ko#onCreate())`가 호출된 시점부터 `[onDestroy()](https://developer.android.com/reference/android/app/Service?hl=ko#onDestroy())` 반환 시점까지입니다. 액티비티와 마찬가지로 서비스는 자신의 초기 설정을 `[onCreate()](https://developer.android.com/reference/android/app/Service?hl=ko#onCreate())`에서 수행하며, 남은 리소스를 모두 `[onDestroy()](https://developer.android.com/reference/android/app/Service?hl=ko#onDestroy())`에서 릴리스합니다. 예를 들어 음악 재생 서비스는 스레드를 생성하고, 이 스레드의 `[onCreate()](https://developer.android.com/reference/android/app/Service?hl=ko#onCreate())`에서 음악이 재생됩니다. 그런 다음, `[onDestroy()](https://developer.android.com/reference/android/app/Service?hl=ko#onDestroy())`에서 스레드를 중단할 수 있습니다.
- 서비스의 **활성 수명**은 `[onStartCommand()](https://developer.android.com/reference/android/app/Service?hl=ko#onStartCommand(android.content.Intent,%20int,%20int))` 또는 `[onBind()](https://developer.android.com/reference/android/app/Service?hl=ko#onBind(android.content.Intent))`에 대한 호출에서부터 시작됩니다. 각 메서드는 `[Intent](https://developer.android.com/reference/android/content/Intent?hl=ko)`를 받아서 `[startService()](https://developer.android.com/reference/android/content/Context?hl=ko#startService(android.content.Intent))` 또는 `[bindService()](https://developer.android.com/reference/android/content/Context?hl=ko#bindService(android.content.Intent,%20android.content.ServiceConnection,%20int))`에 전달합니다.

### **onCreate()**

- 서비스가 생성될 때 최초로 호출됨
- 초기 설정을 수행

### **onStartCommand()**

- `startService()` 로 Service가 시작될 때 호출됨.
    - `startService()`를 통해 전달한 Intent를 `onStartCommand()` 에서 서비스가 수신하게 된다.
- 서비스는 작업이 완료되면 `stopSelf()`를 호출하여 스스로 중단하거나 다른 컴포넌트가 `stopService()` 를 호출하여 중단시킬 수 있다. (콜백 없음)
    - `onStartCommand()`호출을 한 번이라도 받은 서비스는 lifecycle을 직접 관리 및 중단해야 한다. (이후 `onBindCommand()`가 호출 되었더라도)
        - 시스템이 서비스를 중단하거나 소멸시키지 않음

### **onBind()**

- `bindService()` 로 클라이언트가 Service에 binding될 때 호출됨.

### **onUnbind()**

- **모든** 클라이언트들이 `unbindService()`를 호출해서 binding이 해지되었을 때 호출 됨.

### **onRebind()**

- `unbindService()`가 호출된 이후에, 클라이언트가 `bindService()`를 호출하여 Service에 binding될 때 호출됨.

### **5. onDestory()**

- Service가 해지될 때 호출 됨.
- 리소스 해지를 수행

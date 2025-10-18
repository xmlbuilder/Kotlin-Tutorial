# sealed class

## 같은 파일에서 상속 가능
sealed class NetworkClientExceptionV3를 선언하면 해당 클래스는 같은 파일 내에서만 상속이 가능합니다.  
즉, 다음과 같은 구조는 가능합니다:

```kotlin
// 같은 파일 내에서 선언
sealed class NetworkClientExceptionV3(message: String) : Exception(message)

class ConnectExceptionV3(val address: String, message: String) :
    NetworkClientExceptionV3(message)

class SendExceptionV3(val sendData: String, message: String) :
    NetworkClientExceptionV3(message)
```

## 다른 파일에서 상속 불가능

하지만 다음과 같은 구조는 불가능합니다:    
```kotlin
// 파일 A.kt
sealed class NetworkClientExceptionV3(message: String) : Exception(message)

// 파일 B.kt
class ConnectExceptionV3(...) : NetworkClientExceptionV3(...) // ❌ 오류 발생
```

- 🔒 sealed class는 계층을 제한하기 위한 키워드이기 때문에, 상속받는 모든 하위 클래스는 반드시 같은 Kotlin 파일 안에 있어야 합니다.
- 이 덕분에 when 표현식에서 컴파일러가 모든 하위 타입을 알고 있어 타입 분기 처리가 안전하고 명확해집니다.

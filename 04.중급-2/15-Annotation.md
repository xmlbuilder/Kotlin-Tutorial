# Annotation
Java 애너테이션 예제를 Kotlin 스타일로 변환한 전체 내용입니다.  
Kotlin에서는 Java 애너테이션과의 호환성이 뛰어나며, 자체적으로 애너테이션을 정의하고 사용할 수 있습니다.

## 🧠 사용자 애너테이션이란?
- Kotlin에서도 Java처럼 애너테이션`(annotation)` 을 정의하고 사용할 수 있습니다.
- @Target, @Retention, @Repeatable, @Inherited 등의 메타 애너테이션도 지원됩니다.
- Kotlin 애너테이션은 클래스, 함수, 프로퍼티, 생성자 등 다양한 요소에 적용할 수 있습니다.

## 📘 1. 커스텀 애너테이션 + 배열 애너테이션
### 🔹 Kotlin 코드
```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Author(val value: String)
```
```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Authors(val value: Array<Author>)
```
```kotlin
@Authors(
    value = [
        Author("jhjeong"),
        Author("hyangseon")
    ]
)
class AnnotationSample1
```
```kotlin
fun main() {
    val authors = AnnotationSample1::class
        .annotations
        .filterIsInstance<Authors>()
        .flatMap { it.value.toList() }

    for (author in authors) {
        println(author.value)
    }
}
```


## 📘 2. @Inherited 애너테이션의 상속 여부

Kotlin에서는 @Inherited가 기본적으로 적용되지 않으며, Java와의 상호 운용 시만 의미가 있습니다.

### 🔹 Kotlin 코드
```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
@MustBeDocumented
annotation class InheritedAnnotationType
```
```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class UnInheritedAnnotationType
```
```
```kotlin
@UnInheritedAnnotationType
open class A
```
```kotlin
@InheritedAnnotationType
open class B : A()

class C : B()
```

### 🔹 확인 코드
```kotlin
fun main() {
    println("A: " + A::class.annotations)
    println("B: " + B::class.annotations)
    println("C: " + C::class.annotations) // InheritedAnnotationType는 상속되지 않음
}
```


## 📘 3. 메서드에 애너테이션 + 디폴트 값
### 🔹 Kotlin 코드
```kotlin
@Target(AnnotationTarget.FUNCTION)
@Retention(AnnotationRetention.RUNTIME)
annotation class MyAnnotationRunTime(
    val key: String = "foo",
    val value: String = "bar"
)
```
```kotlin
class MyService {
    @MyAnnotationRunTime
    fun testDefaults() {}

    @MyAnnotationRunTime(key = "sample", value = "models")
    fun testValues() {}
}
```
```kotlin
fun main() {
    val methods = MyService::class.members
    for (method in methods) {
        val ann = method.annotations.filterIsInstance<MyAnnotationRunTime>().firstOrNull()
        if (ann != null) {
            println("${method.name}: key=${ann.key}, value=${ann.value}")
        }
    }
}
```


## 💡 실전 예제: 권한 체크 애너테이션
### 🔹 Kotlin 코드
```kotlin
@Target(AnnotationTarget.FUNCTION)
@Retention(AnnotationRetention.RUNTIME)
annotation class RequiresRole(val value: String)
```
```kotlin
class UserService {
    @RequiresRole("ADMIN")
    fun deleteUser(userId: String) {
        println("Deleting user: $userId")
    }
}
```
```kotlin
object SecurityInterceptor {
    fun checkAccess(obj: Any, methodName: String, currentRole: String) {
        val method = obj::class.members.firstOrNull { it.name == methodName }
        val role = method?.annotations?.filterIsInstance<RequiresRole>()?.firstOrNull()
        if (role != null && role.value != currentRole) {
            throw SecurityException("Access denied: role $currentRole insufficient")
        }
        method?.call(obj, "user123")
    }
}
```
```kotlin
fun main() {
    try {
        SecurityInterceptor.checkAccess(UserService(), "deleteUser", "USER") // 예외 발생
    } catch (e: Exception) {
        e.printStackTrace()
    }
}
```


📌 Kotlin 애너테이션 요약
| 항목                  | 설명 또는 키워드                                      | 예시 코드 또는 표현                         |
|-----------------------|--------------------------------------------------------|---------------------------------------------|
| 애너테이션 선언       | 사용자 정의 애너테이션 클래스                         | `annotation class Author(val value: String)` |
| 메타 애너테이션       | 적용 대상 및 유지 범위 지정                          | `@Target(AnnotationTarget.FUNCTION)`         |
| 기본값 지정           | 애너테이션 속성에 디폴트 값 설정                     | `val key: String = "foo"`                    |
| 리플렉션 접근         | 클래스의 멤버 및 애너테이션 정보 조회                | `::class.members`, `annotations.filterIsInstance<>()` |
| 실전 활용 예시        | 접근 제어, 보안, 로깅 등                              | `@RequiresRole("ADMIN")`                     |

---





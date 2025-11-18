# Spread Operator

아래는 기존 Java 중심 설명을 Kotlin 문법과 관점으로 바꾼 문서입니다.  
Kotlin의 `*` spread operator와 vararg 사용 방식에 맞춰 재작성했습니다.

## Spread Operator
Kotlin에서는 JavaScript처럼 명시적인 spread operator `*` 를 지원하며,
vararg와 함께 배열을 개별 인자로 전달할 수 있습니다.

## 🧠 Kotlin에서의 "스프레드" 개념
Kotlin은 `vararg`와 `*` spread operator를 통해 배열을 함수 인자로 펼치는 기능을 명시적으로 제공합니다.

## ✅ vararg 문법
```kotlin
fun printMany(vararg elements: String) {
    for (e in elements) {
        println(e)
    }
}
```

- vararg elements: String은 내부적으로 Array<String>처럼 동작
- 호출 예:
```kotlin
printMany("apple", "banana", "kiwi")
val fruits = arrayOf("apple", "banana", "kiwi")
printMany(*fruits) // 배열을 펼쳐서 전달
```

### 🔍 Kotlin에서 배열을 "펼쳐서" 전달하는 방식
Kotlin에서는 `*` spread operator를 사용하여 배열을 `vararg` 에 전달할 수 있습니다:
```kotlin
val fruits = arrayOf("apple", "banana", "kiwi")
printMany(*fruits) // 배열을 펼쳐서 전달
```
- 이 방식은 JavaScript의 ...fruits와 유사한 효과를 냅니다
- 단, 함수가 `vararg` 로 선언되어 있어야 `*` 를 사용할 수 있습니다

## ✅ Kotlin은 직접적인 spread operator를 지원함
Kotlin에서는 다음과 같은 문법이 가능합니다:
```kotlin
val args = arrayOf("one", "two", "three")
doSomething(*args) // ✅ Kotlin에서는 지원됨

fun doSomething(vararg args: String) {
    for (arg in args) {
        println(arg)
    }
}
```
- JavaScript의 ...args와 동일한 역할
- Java에서는 불가능했던 문법이 Kotlin에서는 자연스럽게 사용 가능

## 📌 Kotlin에서의 spread operator 요약
| 기능                     | Kotlin에서의 구현 방식                      |
|--------------------------|---------------------------------------------|
| 배열을 인자로 펼치기     | `vararg` + `*` 사용 (`fun f(vararg x: T)`)   |
| 배열을 그대로 전달       | `f(*array)` → 내부적으로 `Array<T>`         |
| 직접적인 spread 문법     | ✅ 있음 (`*args` 문법으로 배열 펼침 가능)     |


## 📌 Kotlin 핵심 요약
| 설명 또는 조건             | 코드 예시                                 | 결과 또는 지원 여부                  |
|----------------------------|-------------------------------------------|--------------------------------------|
| vararg 함수 선언           | fun doSomething(vararg args: String)      | ✅ 배열을 펼쳐서 받을 수 있음         |
| 배열을 펼쳐서 전달         | doSomething(*array)                       | ✅ `*` spread operator로 전달 가능     |
| 배열을 그대로 전달         | doSomething(array)                        | ❌ 컴파일 오류 (vararg에는 펼쳐야 함) |

---




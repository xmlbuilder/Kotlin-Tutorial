# Wrapper class

## 🧬 Kotlin에서의 기본형과 래퍼 클래스
Kotlin은 자바와 달리 기본형과 래퍼 클래스를 자동으로 구분하고, nullable 여부에 따라 내부적으로 int 또는 Integer를 선택함.
| 항목               | `Int` (비nullable)         | `Int?` (nullable) / Java `Integer` |
|--------------------|-----------------------------|-------------------------------------|
| 객체 여부          | JVM에서 기본형 `int`        | JVM에서 `Integer` 객체 사용         |
| null 표현 가능 여부| 불가능                      | 가능 (`null`)                       |
| 컬렉션 사용        | `List<Int>` 가능            | `List<Int?>` 가능                   |
| 메서드 제공        | 확장 함수로 제공 가능       | 동일                                |
| 비교 방식          | `==`는 값 비교              | `==`는 값 비교 (`equals()` 호출됨) |
| 박싱/언박싱 처리   | 자동 처리됨                 | 자동 처리됨                         |

## 🔍 Kotlin 예제 비교
### 기본형 사용
```kotlin
val value: Int = 10
val result = compareTo(value, 5)

fun compareTo(value: Int, target: Int): Int {
    return when {
        value < target -> -1
        value > target -> 1
        else -> 0
    }
}
```

### 래퍼 클래스처럼 사용 (nullable)
```kotlin
data class MyInteger(val value: Int) {
    fun compareTo(target: Int): Int {
        return when {
            value < target -> -1
            value > target -> 1
            else -> 0
        }
    }

    override fun toString(): String = value.toString()
}

val myInteger = MyInteger(10)
val result = myInteger.compareTo(5)
```

---

### 📦 박싱 & 언박싱 in Kotlin
Kotlin은 nullable 타입을 사용하면 자동으로 박싱됨:
```kotlin
val a: Int = 10         // JVM에서 int
val b: Int? = 10        // JVM에서 Integer (박싱됨)
val c: Int = b!!        // 언박싱
```

- Int? → 박싱됨 (Integer)
- !! 또는 연산 시 → 언박싱됨 (int)

### 🧰 주요 메서드 대응
| Java 메서드        | Kotlin 대응 방식                          |
|--------------------|-------------------------------------------|
| `valueOf("10")`    | `"10".toInt()`, `"10".toIntOrNull()`      |
| `parseInt("10")`   | `"10".toInt()`                            |
| `compareTo()`      | `a.compareTo(b)`, `a < b`                 |
| `sum(a, b)`        | `a + b`                                   |
| `min(a, b)`        | `minOf(a, b)`                             |
| `max(a, b)`        | `maxOf(a, b)`                             |


### ⚡ 성능 비교: Kotlin에서 기본형 vs 박싱
| 항목               | `Int` (비nullable)         | `Int?` (nullable) / Java `Integer` |
|--------------------|-----------------------------|-------------------------------------|
| JVM 타입           | `int`                       | `Integer`                           |
| 객체 여부          | ❌ 기본형                    | ✅ 객체로 처리됨                     |
| null 표현 가능 여부| ❌ 불가능                    | ✅ 가능 (`null`)                     |
| 컬렉션 사용        | ✅ `List<Int>` 가능          | ✅ `List<Int?>` 가능                 |
| 박싱 발생 여부     | ❌ 없음                      | ✅ 있음 (`Int?` → 박싱됨)            |
| 언박싱 발생 여부   | ❌ 없음                      | ✅ 있음 (`Int?` → `!!` 또는 연산 시) |
| 연산 성능          | ✅ 빠름                      | ❌ 느림 (박싱/언박싱 비용 발생)      |
| 메모리 사용량      | 낮음                         | 높음 (객체 메타 포함)               |


### 🧠 유지보수 vs 최적화 in Kotlin
| 관점             | 유지보수 중심 (`Int?`, `toIntOrNull()`)     | 성능 최적화 중심 (`Int`)                  |
|------------------|----------------------------------------------|-------------------------------------------|
| 코드 안정성       | ✅ null-safe 처리 가능                        | ❌ null 처리 불가                          |
| 예외 발생 가능성  | ✅ 예외 없이 안전한 변환 (`toIntOrNull()`)   | ❌ 잘못된 입력 시 예외 발생 가능           |
| 가독성            | ✅ 명확한 의도 표현 (`Int?`로 값 없음 표현) | ✅ 간결한 코드                             |
| 성능              | ❌ 박싱/언박싱 비용 발생 가능                | ✅ 빠른 연산 및 메모리 효율                |
| 컬렉션 처리       | ✅ `List<Int?>`로 유연한 구조 가능           | ✅ `List<Int>`로 성능 최적화 가능          |
| 적용 상황         | 일반적인 앱, 사용자 입력 처리 등             | 대규모 반복 연산, 실시간 처리 등           |

### 💡 핵심 요약
- 유지보수 중심: null-safe, 예외 없는 처리, 유연한 구조
- 성능 중심: 빠른 연산, 낮은 메모리 사용, 반복 처리에 유리
- Kotlin은 상황에 따라 Int와 Int?를 적절히 선택하는 것이 중요함


## 📥 전체 요약: Kotlin 관점
| 항목               | Kotlin: `Int` / Java `int`         | Kotlin: `Int?` / Java `Integer`           |
|--------------------|-------------------------------------|-------------------------------------------|
| 타입 구분           | 기본형                              | 래퍼 클래스 / nullable 객체               |
| JVM 타입           | `int`                               | `Integer`                                 |
| null 처리           | ❌ 불가능                           | ✅ 가능 (`null`)                           |
| 컬렉션 사용         | `List<Int>` 가능                    | `List<Int?>` 가능                          |
| 비교 방식           | `==` 값 비교                        | `==` 값 비교 (`equals()` 자동 호출됨)     |
| 언박싱 처리         | ❌ 없음                             | ✅ `Int?!!` 또는 연산 시 자동 언박싱       |
| 주요 메서드         | `toInt()`, `compareTo()`, `minOf()`, `maxOf()` | 동일                             |
| 성능               | ✅ 빠름                             | ❌ 느림 (박싱/언박싱 비용 발생)            |


### 🔍 조금 더 자세히 설명하자면:
- Kotlin은 기본적으로 Int 하나만 제공해. 개발자는 int와 Integer를 구분할 필요 없이 Int 또는 Int?만 사용하면 됨.
- 하지만 Kotlin은 JVM 위에서 동작하기 때문에, 실제 바이트코드로 컴파일될 때는 다음과 같이 처리됨:
#### 🔍 Kotlin 타입 → JVM 타입 매핑
| Kotlin 타입 | JVM에서 변환되는 타입 | 설명                          |
|-------------|----------------------|-------------------------------|
| `Int`       | `int`                | 기본형, 박싱 없이 빠름         |
| `Int?`      | `Integer`            | nullable이므로 객체로 처리됨   |

- 즉, nullable 여부에 따라 내부적으로 자바의 int인지 Integer인지 결정돼.

### ✅ 예시로 보면 더 명확해:
```kotlin
val a: Int = 10      // JVM에서는 int
val b: Int? = 10     // JVM에서는 Integer (박싱됨)
val c: Int = b!!     // 언박싱 발생
```

### 💡 핵심 포인트
- Kotlin은 Int 하나로 개발자 경험을 단순화함
- 성능이 중요한 경우엔 Int를, null 처리가 필요한 경우엔 Int?를 사용
- 내부적으로는 자바의 int와 Integer를 자동으로 매핑해서 처리


### 🧬 Kotlin의 실행 환경별 타입 처리 방식
| 실행 환경       | `Int` 처리 방식         | `Int?` 처리 방식              |
|----------------|--------------------------|-------------------------------|
| JVM            | `Int` → `int`            | `Int?` → `Integer`            |
| Kotlin/Native  | `Int` → 플랫폼별 정수 타입 | `Int?` → nullable 구조체       |
| Kotlin/JS      | `Int` → `number`         | `Int?` → `number` 또는 `null` |

## 🔍 핵심 요약
- Kotlin은 언어적으로 Int 하나만 제공하지만, 컴파일 타겟에 따라 내부적으로 다른 방식으로 변환.
- JVM에서는 자바의 int와 Integer로 자동 매핑되고, Native나 JS에서는 해당 플랫폼에 맞는 타입으로 변환됨.
- 개발자는 Int와 Int?만 신경 쓰면 되고, 어떤 환경에서 실행되든 Kotlin 컴파일러가 알아서 처리해줌.

---

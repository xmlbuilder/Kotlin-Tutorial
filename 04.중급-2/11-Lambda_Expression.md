# Lambda Expression
아래는 Java 기반 람다 표현식 예제를 Kotlin 스타일로 완전히 변환한 버전입니다.  
클래스 구조, 정렬, 반복, 스레드 생성까지 모두 코틀린답게 간결하고 직관적으로 바꿨습니다.

## 📦 Kotlin Lambda Expression Sample
### ✅ Product 클래스
```kotlin
data class Product(
    var name: String,
    var isFood: Boolean,
    var madeBy: String
)
```
- data class로 간결하게 정의
- getter/setter 자동 생성

### 🚀 Main 함수
```kotlin
fun main() {
    val products = arrayListOf(
        Product("Sample1", true, "Korea"),
        Product("Sample22", true, "Korea"),
        Product("Sample345", true, "Korea")
    )

    // 🔢 1. 전통 방식 (익명 클래스 → Kotlin에서는 object 표현)
    products.sortWith(object : Comparator<Product> {
        override fun compare(o1: Product, o2: Product): Int {
            return o2.name.length - o1.name.length
        }
    })
    products.forEach { println(it.name) }

    // ⚡ 2. 람다 표현식 기반 정렬
    products.sortBy { it.name.length }
    products.forEach { println(it.name) }

    // 🔁 3. 람다를 이용한 컬렉션 순회
    products.forEach { product ->
        println(product.name)
    }

    // 🧵 4. 스레드 생성 비교
    val th = Thread {
        println("NormalThread")
    }
    th.start()

    val th1 = Thread {
        println("NormalStarted")
    }
    th1.start()
}
```


## 💡 요약: 람다 표현식 vs 전통 방식

| 기능         | 전통 방식                     | Kotlin 람다 표현식 방식         |
|--------------|-------------------------------|----------------------------------|
| 정렬         | Comparator                    | sortBy { ... }, sortWith { ... } |
| 컬렉션 순회  | for-each                      | forEach { item -> ... }          |
| 스레드 생성  | new Runnable() { ... }        | Thread { ... }                   |


## 🧩 참고
- Kotlin은 람다 표현식이 기본 문법으로 내장되어 있어 훨씬 간결함
- data class, forEach, sortBy, Thread {} 등으로 코드량 대폭 감소
- Java 8의 람다 표현식보다 더 직관적이고 함수형 스타일에 가까움

---

## 🔍 it의 정체
- 람다 표현식에서 매개변수가 하나일 경우, Kotlin은 it이라는 이름으로 자동 참조
- 명시적으로 이름을 지정하지 않아도 됨
- 예를 들어:
```kotlin
products.forEach { println(it.name) }
```
- 여기서 it은 Product 타입의 각 요소를 의미
- forEach는 products 리스트를 순회하며 각 요소를 it으로 넘김

## ✅ 명시적으로 이름 지정도 가능
```kotlin
products.forEach { product ->
    println(product.name)
}
```
- 이렇게 하면 it 대신 product라는 이름으로 직접 참조 가능
- 복잡한 람다에서는 명시적으로 이름을 지정하는 게 가독성에 좋을 때도 있음

## 📌 요약: Kotlin 람다 표현식에서 it 사용

| 표현 방식                          | 코드 예시                          | it 사용 여부 |
|-----------------------------------|------------------------------------|--------------|
| 단일 인자, it 자동 제공           | { println(it.name) }               | ✅ 사용됨     |
| 명시적 인자 이름 지정             | { product -> println(product.name) } | ❌ 사용 안 함 |

---




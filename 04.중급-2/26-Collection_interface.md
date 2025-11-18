# Collection Interface
Kotlin에도 Java처럼 Collection이라는 인터페이스가 존재합니다.  
하지만 대부분은 `List`, `Set`, `Map` 같은 구체적인 컬렉션 타입을 직접 사용하기 때문에  
`Collection` 자체를 명시적으로 사용하는 경우는 드뭅니다.

## 🧠 Kotlin의 Collection 인터페이스란?
- `Collection<T>` 는 Kotlin 컬렉션 계층 구조의 루트 인터페이스입니다.
- `Iterable<T>` 를 상속하며, 읽기 전용 컬렉션의 공통 기능을 정의합니다.
- `List`, `Set`,` Map` 등은 모두 이를 기반으로 구현됩니다.

## 📦 주요 기능 — Kotlin Collection 인터페이스
| 메서드               | 설명                                      |
|----------------------|-------------------------------------------|
| `contains(element)`  | 특정 요소가 포함되어 있는지 확인           |
| `isEmpty()`          | 컬렉션이 비어 있는지 여부 확인             |
| `size`               | 현재 컬렉션의 요소 개수를 반환             |
| `iterator()`         | 컬렉션을 반복할 수 있는 `Iterator` 반환     |



## 🔗 주요 하위 인터페이스
| 인터페이스 | 특징                         | 대표 구현 클래스               |
|-------------|------------------------------|--------------------------------|
| `List<T>`   | 순서 O, 중복 O                | `listOf`, `mutableListOf`      |
| `Set<T>`    | 순서 X, 중복 X                | `setOf`, `mutableSetOf`        |
| `Map<K,V>`  | 키-값 쌍, 키 중복 X            | `mapOf`, `mutableMapOf`        |

- 이들은 모두 Collection<T> 또는 Map<K,V>를 기반으로 하며, 읽기 전용과 변경 가능한 버전이 구분되어 있습니다.

## ✅ 사용 예시
```kotlin
fun main() {
    val items: Collection<String> = listOf("apple", "banana")

    for (item in items) {
        println(item)
    }
}
```

- 여기서 items는 Collection<String> 타입이지만 실제 객체는 List
- 이렇게 하면 다형성(polymorphism) 을 활용해 유연한 코드 작성이 가능

## 🤔 왜 자주 못 봤을까?
- 대부분의 Kotlin 개발자는 List, Set, Map 같은 구체적인 타입을 직접 사용합니다.
- Collection은 공통 인터페이스로서 추상화에 사용되며, 실무에서는 특정 기능에 맞는 하위 타입을 선택하는 경우가 많습니다.
- 예: `val names: List<String> = listOf("A", "B")` → Collection보다 List가 더 구체적이고 직관적

## ✅ 예시로 설명
```kotlin
fun main() {
    val items1: Collection<String> = listOf("apple")
    val items2: Collection<String> = setOf("banana")
    val items3: Collection<String> = mutableListOf("kiwi")

    for (item in items1) println(item)
    for (item in items2) println(item)
    for (item in items3) println(item)
}
```

- items1, items2, items3는 모두 Collection<String> 타입으로 선언
- 각각은 List, Set, MutableList라는 서로 다른 구현 클래스
- 공통된 인터페이스인 Collection으로 다형성(polymorphism)을 활용해 하나처럼 다룰 수 있음

## 📌 핵심 요약
| 특징                         | 설명                                 |
|------------------------------|--------------------------------------|
| `Collection<T>`는 인터페이스 | 객체가 아니라 공통 타입 정의         |
| 다양한 구현체를 받을 수 있음 | `List`, `Set`, `Map` 등              |
| 다형성으로 유연한 코드 가능  | 하나의 변수로 여러 타입 처리 가능     |



## 🧠 핵심 개념: Sequence는 Collection의 확장이 아니다
- Kotlin의 `Sequence<T>` 는 `Collection<T>` 를 확장하지 않습니다.
- 대신, `asSequence()` 확장 함수를 통해 `Collection` 에서 `Sequence` 를 생성할 수 있습니다.
- Sequence는 지연 계산(lazy evaluation)을 위한 별도의 추상화입니다.

## ✅ 관계 요약
| 개념         | 설명 또는 예시                                |
|--------------|-----------------------------------------------|
| `Collection` | 데이터를 저장하는 구조 (`List`, `Set`, `Map`) |
| `Sequence`   | 데이터를 처리하는 구조 (`map`, `filter` 등)   |
| 연결 방식    | `collection.asSequence()` → `Sequence` 생성   |



### 🔍 예시
```kotlin
val names = listOf("Alice", "Bob", "Charlie")

val nameSequence = names.asSequence()

nameSequence
    .filter { it.startsWith("A") }
    .map { it.uppercase() }
    .forEach { println(it) }

```
- asSequence()는 List에서 Sequence를 생성
- 이후 filter, map, forEach 등으로 처리

## 📦 Sequence를 생성할 수 있는 주요 방식
| 타입              | Sequence 생성 방식                         |
|-------------------|--------------------------------------------|
| `Collection`      | `asSequence()`                             |
| `generateSequence`| 무한 시퀀스 생성 (`generateSequence { ... }`) |
| `sequenceOf(...)` | 직접 요소를 나열하여 시퀀스 생성           |


## 📌 결론
| 요약 항목                         | 설명                                               |
|----------------------------------|----------------------------------------------------|
| `Sequence`는 `Collection`이 아님 | 별도의 지연 처리 추상화                           |
| `Collection`에서 생성 가능       | `asSequence()` 사용                                |
| `Sequence`는 데이터를 저장하지 않음 | 처리 파이프라인 구성에 적합 (lazy evaluation)     |

---



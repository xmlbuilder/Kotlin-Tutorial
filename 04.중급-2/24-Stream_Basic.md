## 🚀 Kotlin 컬렉션 처리란?
Kotlin은 컬렉션이나 시퀀스를 함수형 스타일로 처리할 수 있게 해줍니다.
- 선언적: 반복문 없이 처리 흐름을 선언
- 체이닝: 여러 연산을 연결해 처리
- 병렬 처리: 기본적으로 단일 스레드지만 코루틴과 함께 사용 가능

## 🔍 예제 코드 해석
### 1. 문자열 리스트 처리
```kotlin
val fruits = listOf("apple", "banana", "kiwi", "orange")

fruits
    .filter { it.contains("a") }       // "apple", "banana", "orange"
    .map { it.uppercase() }            // "APPLE", "BANANA", "ORANGE"
    .sorted()                          // "APPLE", "BANANA", "ORANGE"
    .forEach { println(it) }           // 각 요소 출력

```

- filter → 조건에 맞는 요소만 통과
- map → 요소 변환
- sorted → 정렬
- forEach → 최종 처리 (출력)

### 2. 정수 리스트에서 홀수 개수 세기
```kotlin
val numbers = listOf(0, 1, 2, 3, 42)

val count = numbers
    .filter { it % 2 == 1 }  // 홀수만 필터링: 1, 3
    .count()                 // 개수 세기 → 2

println(count) // 출력: 2
```
### 📦 Kotlin 컬렉션 기본 메서드 요약

| 메서드     | 설명                                      |
|------------|-------------------------------------------|
| `listOf()` | 리스트 생성                               |
| `filter()` | 조건에 맞는 요소만 필터링                 |
| `map()`    | 각 요소를 변환                            |
| `sorted()` | 요소를 정렬                               |
| `forEach()`| 각 요소에 대해 작업 수행 (최종 연산)      |
| `count()`  | 조건에 맞는 요소의 개수 반환 (최종 연산)  |


## 🧠 핵심 요약
- Kotlin 컬렉션은 함수형 스타일로 데이터 흐름을 처리
- 중간 연산 (filter, map, sorted)은 컬렉션을 변형
- 최종 연산 (forEach, count)은 결과를 소비

## collect()에 해당하는 Kotlin 방식
Kotlin에서는 collect() 대신 toList(), toSet(), joinToString() 등을 사용합니다.

## 🧲 toList() — 결과를 모으는 최종 연산
```kotlin
val fruits = listOf("apple", "banana", "kiwi")
    .filter { it.contains("a") }
    .toList()
```

- 결과: `["apple", "banana"]`

## 🧮 fold() — 누적 계산
```kotlin
val sum = listOf(1, 2, 3, 4, 5)
    .fold(0) { acc, i -> acc + i }
```

- 결과: 15

## 🧹 flatMap() — 중첩 구조 평탄화
### ✅ 예제 1: 리스트 평탄화
```rust
val listOfLists = listOf(
    listOf("apple", "banana"),
    listOf("kiwi", "orange")
)

val flatList = listOfLists
    .flatMap { it }
    .toList()
```

- 결과:`["apple", "banana", "kiwi", "orange"]`

### ✅ 예제 2: 문자열 → 문자 리스트
```rust
val words = listOf("java", "stream")

val chars = words
    .flatMap { it.toList() }
    .toList()
```

- 결과: `['j','a','v','a','s','t','r','e','a','m']`

---

### 📌 Stream 고급 연산 요약 (Kotlin 대응)

| 메서드      | 역할                          | 예시 결과                      | 비고                         |
|-------------|-------------------------------|--------------------------------|------------------------------|
| `toList()`  | 스트림 결과를 리스트로 수집     | `List`, `Set` 등으로 수집       | Java의 `collect(toList())` 대응 |
| `fold()`    | 누적 계산                      | 합계, 곱셈, 문자열 연결 등     | Java의 `reduce()` 대응       |
| `flatMap()` | 중첩 구조 → 평탄화된 리스트     | 리스트 → 요소 리스트           | Java의 `flatMap()`과 동일    |



### 📌 Kotlin 컬렉션 고급 연산 요약

| 메서드         | 역할                                      | 예시 결과                            | 비고                                |
|----------------|-------------------------------------------|--------------------------------------|-------------------------------------|
| `groupBy()`    | 조건에 따라 요소를 그룹화                 | `Map<key, List<elements>>`           | key 기준으로 리스트 묶음             |
| `associateBy()`| 요소를 key-value 쌍으로 매핑              | `Map<key, element>`                  | 중복 key 발생 시 마지막 요소 유지    |
| `joinToString()`| 요소를 문자열로 연결                     | `"a, b, c"`                          | 구분자, 접두사, 접미사 지정 가능     |



### 간단 예제들
```rust
val fruits = listOf("apple", "banana", "kiwi", "apricot")

val grouped = fruits.groupBy { it.first() }
// 결과: {a=[apple, apricot], b=[banana], k=[kiwi]}

val associated = fruits.associateBy { it.length }
// 결과: {5=apple, 6=banana, 4=kiwi, 7=apricot}

val joined = fruits.joinToString(", ", prefix = "[", postfix = "]")
// 결과: "[apple, banana, kiwi, apricot]"
```

---





# Function Interface
아래는 Java의 함수형 인터페이스 예제를 Kotlin 스타일로 완전히 변환한 버전입니다.  
Kotlin에서는 Function, Consumer, Predicate, UnaryOperator 같은 인터페이스 대신 람다 표현식 자체를 사용하며, 훨씬 간결하게 작성할 수 있음.

## 🧠 Kotlin에서 함수형 인터페이스 스타일 구현
### ✅ 1. 문자열을 날짜로 변환 (Function<String, Date>)
```kotlin
val toDate: (String) -> Date? = { s ->
    try {
        SimpleDateFormat("yyyy/MM/dd").parse(s)
    } catch (e: ParseException) {
        e.printStackTrace()
        null
    }
}

val date = toDate("2015/09/27")
println(date)
```


## ✅ 2. 문자열을 대문자로 변환 (UnaryOperator<String>)
```kotlin
val toUpper: (String) -> String = { it.uppercase() }

val result = toUpper("java")
println(result) // JAVA
```


## ✅ 3. 날짜를 포맷해서 출력 (Consumer<Date>)
```kotlin
val printDate: (Date) -> Unit = { date ->
    val formatted = SimpleDateFormat("yyyy/MM/dd").format(date)
    println(formatted)
}

printDate(Date())
```


## ✅ 4. 문자열이 "Java"로 시작하는지 검사 (Predicate<String>)
```kotlin
val condition: (String) -> Boolean = { it.startsWith("Java") }

println(condition("javaScript")) // false
```


## 📌 요약: Java → Kotlin 대응
| Java 인터페이스     | Kotlin 표현 방식     | 설명               |
|---------------------|----------------------|--------------------|
| Function<T, R>      | (T) -> R             | 입력 → 출력        |
| UnaryOperator<T>    | (T) -> T             | 입력과 출력 동일   |
| Consumer<T>         | (T) -> Unit          | 반환값 없음        |
| Predicate<T>        | (T) -> Boolean       | 조건 판별          |

---
# Kotlin -> Java

Kotlin이 어떻게 일반 람다를 Java 바이트코드로 변환하고, Function 인터페이스와 연결되는지를  
이해하려면 Kotlin → JVM 컴파일 과정을 살펴봐야 함.

## 🔍 Kotlin 람다 → Java 바이트코드 변환 과정
###  ✅ 1. Kotlin은 람다를 함수 타입으로 표현함
```kotlin
val f: (String) -> Int = { it.length }
```
- 이건 Kotlin에서 Function1<String, Int> 타입으로 인식됨
- Function1, Function2, ..., FunctionN은 Kotlin의 내부 함수형 인터페이스 (Kotlin stdlib)

### ✅ 2. JVM으로 컴파일될 때는 익명 클래스 또는 invokedynamic으로 변환됨
- Kotlin은 JVM 타겟일 경우, 람다를 익명 클래스로 변환하거나 Java 8 이상에서는 invokedynamic을 사용
- 이때 Kotlin의 Function1<T, R>은 Java의 kotlin.jvm.functions.Function1로 매핑됨
- Java에서 호출할 때는 Function1.invoke()를 통해 실행됨

### ✅ 3. Java에서 Kotlin 람다를 호출할 때
```java
Function1<String, Integer> f = s -> s.length(); // ❌ Java에서는 이렇게 못 씀
```
- 대신 Kotlin에서 만든 람다를 Java에서 호출하려면 Function1<String, Integer> 타입을 사용하고 invoke()로 호출해야 함
- 예: f.invoke("hello")

### 🔧 Kotlin → Java 바이트코드 예시
```kotlin
fun main() {
    val f: (String) -> Int = { it.length }
    println(f("hello"))
}
```

- 바이트코드에서는 Function1<String, Int> 타입의 익명 클래스가 생성됨
- Java 8 이상에서는 invokedynamic으로 최적화 가능

## 📌 요약: Kotlin 함수 표현 → Java 바이트코드 대응

| Kotlin 표현         | 함수 타입            | 바이트코드 대응 (JVM 기준) |
|---------------------|----------------------|-----------------------------|
| (T) -> R            | 함수 타입            | Function1<T, R>             |
| { it.length }       | 람다 표현식          | 익명 클래스 or invokedynamic |
| f.invoke("hello")   | 함수 호출            | Function1.invoke()          |
| Java Function<T, R> | 명시적 인터페이스    | 유지됨 (java.util.function) |


## 🔍 보충 설명
- Kotlin의 (T) -> R 타입은 내부적으로 Function1<T, R> 클래스로 표현됨
- Java에서는 Function<T, R> 인터페이스를 명시적으로 사용하지만, Kotlin은 함수 타입 자체를 사용
- 바이트코드에서는 Function1.invoke() 또는 invokedynamic으로 최적화됨
- Java의 Function<T, R>는 Kotlin에서 interoperability 용도로만 사용됨

---







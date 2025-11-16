# 🧠 타입 이레이저(Type Erasure)란?
Kotlin도 Java와 동일하게 JVM 위에서 동작하기 때문에,  
제네릭은 컴파일 시점에만 존재하고 런타임에는 타입 정보가 사라지는 특성이 있습니다.  
이를 타입 이레이저(Type Erasure) 라고 합니다.
- 컴파일 시점: 타입 체크 및 제약 조건 확인
- 런타임 시점: 타입 정보는 사라지고 대부분 Any 또는 Object로 처리됨
이러한 설계는 JVM의 하위 호환성(JDK 1.5 이전) 을 유지하기 위한 것입니다.

## 🔍 왜 배열에 제네릭 타입을 직접 사용할 수 없을까?
```kotlin
val array: Array<T> = arrayOfNulls<T>(10) // ❌ 오류 발생
```
- 이유: T는 런타임에 어떤 타입인지 알 수 없기 때문에 JVM이 메모리를 안전하게 할당할 수 없습니다.
- 해결책: Array<Any?> 또는 Array<*>를 사용하고, 필요할 때 as T로 캐스팅합니다.

## ✅ 예제 코드: TArray in Kotlin
```kotlin
class TArray<T> {
    private var items: Array<Any?> = arrayOfNulls(0)

    constructor() {
        items = arrayOfNulls(10)
    }

    constructor(size: Int) {
        setSize(size)
    }

    constructor(rhs: TArray<T>) {
        if (rhs.items.isNotEmpty()) {
            items = Array(rhs.items.size) { rhs.items[it] }
        }
    }

    fun copyFrom(rhs: TArray<T>) {
        if (rhs.items.isNotEmpty()) {
            items = Array(rhs.items.size) { rhs.items[it] }
        }
    }

    fun copyFrom(data: Array<T>) {
        items = Array(data.size) { data[it] }
    }

    fun getSize(): Int = items.size

    fun getItems(): Array<Any?> = items

    fun setItems(data: Array<T>) {
        items = Array(data.size) { data[it] }
    }

    @Suppress("UNCHECKED_CAST")
    fun getData(idx: Int): T = items[idx] as T

    fun setData(idx: Int, data: T) {
        items[idx] = data
    }

    fun clear() {
        items = arrayOf()
    }

    fun setSize(size: Int) {
        items = arrayOfNulls(size)
    }

    fun resize(size: Int) {
        val tmp = arrayOfNulls<Any?>(size)
        System.arraycopy(items, 0, tmp, 0, minOf(size, items.size))
        items = tmp
    }

    fun isEmpty(): Boolean = items.isEmpty()
}
```


## 🧪 실행 예제
```kotlin
fun main() {
    val floatArray = TArray<Float>(10)
    floatArray.setData(0, 11.0f)
    floatArray.setData(1, 12.0f)
    floatArray.setData(2, 13.0f)
    floatArray.setData(3, 14.0f)
    floatArray.setData(4, 15.0f)
    floatArray.setData(5, 16.0f)
    floatArray.setData(6, 17.0f)
    floatArray.setData(7, 18.0f)
    floatArray.setData(8, 19.0f)
    floatArray.setData(9, 20.0f)

    var a = floatArray.getData(9)
    println("a = $a")

    floatArray.resize(6)
    a = floatArray.getData(5)
    println("a = $a")

    floatArray.setData(5, 10000.0f)
    for (i in 0 until floatArray.getSize()) {
        val value = floatArray.getData(i)
        println("val = $value")
    }

    val rawArray = floatArray.getItems()
    for (item in rawArray) {
        val value = item as Float
        println("val = $value")
    }
}
```
## 🔧 핵심 구조

| 구성 요소           | Kotlin에서의 타입 또는 처리 방식 | 설명 |
|--------------------|-------------------------------|------|
| `private var items`| `Array<Any?>`                 | 제네릭 타입 T는 런타임에 사라지므로 Object 기반 배열로 선언 |
| `T`                | `Array<T>` (직접 생성 불가)   | 타입 이레이저로 인해 `Array<T>`는 생성할 수 없으며 캐스팅 필요 |


### 🔁 복사 및 설정
```kotlin
fun copyFrom(data: Array<T>) {
    items = Array(data.size) { data[it] }
}
```
- 외부에서 Array<T>를 받아 내부 Array<Any?>에 복사

### 🔍 읽기
```kotlin
@Suppress("UNCHECKED_CAST")
fun getData(idx: Int): T = items[idx] as T
```

- 내부의 Any?를 T로 강제 캐스팅
- 컴파일러는 경고하지만, 개발자가 타입을 보장한다고 명시

### 🔄 타입 이레이저가 적용된 결과
컴파일 후 Kotlin의 제네릭 클래스는 다음과 같이 동작합니다:
```kotlin
class TArray {
    private var items: Array<Any?>
    fun getData(idx: Int): Any? = items[idx]
}
```

- T는 사라지고, 모든 타입은 Any?로 처리됨
- 제네릭은 컴파일러가 타입 안전성을 보장해주는 문법적 도구일 뿐, 런타임에는 존재하지 않음

## 📌 타입 이레이저 요약

| 개념 또는 제한사항         | 설명 또는 해결 방법                          |
|----------------------------|----------------------------------------------|
| 타입 정보는 런타임에 소거됨 | 컴파일 시점에만 타입 체크 수행               |
| 제네릭 배열 생성 불가       | `arrayOfNulls<T>(size)` 또는 `Array<Any?>` 사용 |
| 타입 캐스팅 필요            | 내부 저장은 `Any?`, 읽을 때 `as T`로 캐스팅 |
| 타입 안전은 컴파일러가 보장 | 런타임에는 타입 정보 없음                   |

--- 
# Kotlin Native

Kotlin/JVM에서는 Java와 마찬가지로 타입 이레이저(Type Erasure)의 제약을 받지만,  
Kotlin/Native에서는 상황이 조금 달라집니다.  
Kotlin/Native는 JVM이 아닌 LLVM 기반으로 컴파일되기 때문에,  
런타임에 제네릭 타입 정보가 유지될 수 있습니다.  
이로 인해 Kotlin Native에서는 제네릭을 다루는 방식이 JVM보다 더 유연해질 수 있음.

## 🧠 Kotlin/Native에서 가능한 차별점
### ✅ 1. 제네릭 타입 정보 유지
- Kotlin/Native는 컴파일 타임에 제네릭 타입 정보를 보존할 수 있습니다.
- 따라서 JVM에서 불가능했던 Array<T>() 생성도 가능해질 수 있습니다.
```kotlin
inline fun <reified T> createArray(size: Int): Array<T?> {
    return arrayOfNulls<T>(size)
}
```
- 위 코드는 Kotlin/JVM에서는 내부적으로 `arrayOfNulls` 를 통해 우회하지만,
- Kotlin/Native에서는 `reified` 타입 파라미터를 통해 타입 정보를 런타임까지 유지할 수 있어 더 안전한 제네릭 처리가 가능합니다.

### ✅ 2. 리플렉션 없이도 타입 안전한 객체 생성 가능
- Kotlin/Native는 Java의 리플렉션 API를 사용할 수 없지만, 대신 reified와 inline을 활용해 타입 안전한 객체 생성을 할 수 있습니다.
```kotlin
inline fun <reified T : Any> createInstance(): T {
    return T::class.constructors.first().call()
}
```
- 위 코드는 JVM에서는 제한이 있지만, Kotlin/Native에서는 T::class를 통해 타입 정보를 직접 사용할 수 있습니다.
- 단, 생성자가 반드시 존재해야 하며, 파라미터가 없는 기본 생성자일 경우에만 동작합니다.

## ✅ 3. 타입 이레이저 없는 구조 설계 가능
- Kotlin/Native는 타입 이레이저가 없기 때문에, 다음과 같은 제약이 사라집니다:

| JVM 제약 또는 키워드 | Kotlin/Native에서의 가능성 또는 대체 방식 |
|----------------------|--------------------------------------------|
| `new T[]`            | `Array<T>` 직접 생성 가능                  |
|                      | `T::class` 사용 가능                       |
|                      | `reified` 타입 파라미터로 런타임 타입 유지 |
| `as T`               | 캐스팅 없이 타입 정보 활용 가능            |



## ✨ 결론
Kotlin/Native에서는 JVM의 타입 이레이저 제약에서 벗어나:
- 제네릭 타입의 배열을 직접 생성할 수 있고
- 리플렉션 없이도 타입 안전한 객체 생성을 할 수 있으며
- 런타임에 타입 정보를 활용한 더 정교한 제네릭 로직이 가능합니다
즉, Kotlin/Native는 제네릭을 다룰 때 훨씬 더 직관적이고 안전한 코드를 작성할 수 있는 환경을 제공합니다.


## 🧪 예제 1: 제네릭 배열 직접 생성
```kotlin
inline fun <reified T> createTypedArray(size: Int): Array<T?> {
    return arrayOfNulls<T>(size)
}

fun main() {
    val intArray = createTypedArray<Int>(5)
    intArray[0] = 42
    println(intArray.joinToString()) // 출력: 42, null, null, null, null
}
```

### 🧠 설명:
- reified 키워드를 사용하면 T의 타입 정보를 런타임에 유지할 수 있어 arrayOfNulls<T>()가 가능
- Kotlin/JVM에서는 이 방식이 제한되지만 Kotlin/Native에서는 완전하게 동작

## 🧪 예제 2: 타입 기반 객체 생성
```rust
inline fun <reified T : Any> createInstance(): T {
    return T::class.constructors.first().call()
}

class Person {
    val name = "JungHwan"
}

fun main() {
    val person = createInstance<Person>()
    println(person.name) // 출력: JungHwan
}
```

### 🧠 설명:
- T::class.constructors를 통해 타입 기반으로 객체를 생성
- 리플렉션 없이도 타입 안전하게 인스턴스를 만들 수 있음

## 🧪 예제 3: 타입 기반 분기 처리
```kotlin
inline fun <reified T> printTypeInfo(value: T) {
    when (T::class) {
        Int::class -> println("정수입니다: $value")
        String::class -> println("문자열입니다: $value")
        else -> println("알 수 없는 타입입니다: $value")
    }
}

fun main() {
    printTypeInfo(123)       // 출력: 정수입니다: 123
    printTypeInfo("Hello")   // 출력: 문자열입니다: Hello
    printTypeInfo(3.14)      // 출력: 알 수 없는 타입입니다: 3.14
}

### 🧠 설명:
- reified를 통해 타입 정보를 유지하고 when 문에서 타입 분기 가능
- JVM에서는 이 방식이 제한되지만 Native에서는 더 유연하게 동작

## 🧪 예제 4: 제네릭 타입을 활용한 캐시 구현
```kotlin
class TypeCache<T : Any> {
    private val map = mutableMapOf<String, T>()

    fun put(key: String, value: T) {
        map[key] = value
    }

    fun get(key: String): T? = map[key]
}
```
```kotlin
fun main() {
    val stringCache = TypeCache<String>()
    stringCache.put("greeting", "안녕하세요")
    println(stringCache.get("greeting")) // 출력: 안녕하세요
}
```

### 🧠 설명:
- 제네릭 타입을 활용한 캐시 구조
- Kotlin/Native에서는 타입 정보가 유지되므로 더 안전하게 동작

이런 예제들은 Kotlin/Native의 타입 시스템을 활용해 JVM에서는 어려운 제네릭 기능을 구현할 수 있다는 점을 잘 보여줍니다.

---


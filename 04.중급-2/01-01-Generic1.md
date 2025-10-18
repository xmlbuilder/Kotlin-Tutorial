# 📦 제네릭(Generic) 개념 정리
## ✅ 제네릭이 필요한 이유

Kotlin 역시 `제네릭(Generic)` 을 지원하며, 코드 재사용성과 타입 안전성을 동시에 만족시키기 위한 핵심 기능입니다.

### 1️⃣ 타입별 클래스의 한계
```kotlin
class IntBox {
    private var value: Int? = null
    fun set(value: Int) { this.value = value }
    fun get(): Int? = value
}

class StringBox {
    private var value: String? = null
    fun set(value: String) { this.value = value }
    fun get(): String? = value
}
```

- 타입마다 클래스를 따로 만들어야 함
- DoubleBox, BooleanBox 등 수십 개의 클래스가 필요할 수 있음

### 2️⃣ Any를 활용한 다형성
```kotlin
class AnyBox {
    private var value: Any? = null
    fun set(value: Any) { this.value = value }
    fun get(): Any? = value
}


val box = AnyBox()
box.set(10)
val value = box.get() as Int // 다운캐스팅 필요
```

#### ❌ 문제점
- 타입 캐스팅 필요 → 런타임 오류 발생 가능
- 타입 안전성 부족 → 잘못된 타입 입력 가능

### 3️⃣ 제네릭 도입
```kotlin
class GenericBox<T> {
    private var value: T? = null
    fun set(value: T) { this.value = value }
    fun get(): T? = value
}


val intBox = GenericBox<Int>()
intBox.set(10)
val value = intBox.get() // 캐스팅 불필요

val strBox = GenericBox<String>()
strBox.set("hello")
val text = strBox.get()
```


### 4️⃣ 타입 추론
```kotlin
val box1: GenericBox<Int> = GenericBox()
val box2 = GenericBox<Int>() // 타입 추론
```

- Kotlin 컴파일러가 좌측 타입 정보를 기반으로 우측 타입을 추론함

## 🧠 정리

| 방식            | 코드 재사용 | 타입 안전성 | 캐스팅 필요 | 오류 가능성 | 설명 요약                          |
|----------------|-------------|--------------|---------------|----------------|-----------------------------------|
| 타입별 클래스   | ❌          | ✅           | ❌            | 낮음           | 타입마다 클래스 생성 필요          |
| Any 활용        | ✅          | ❌           | ✅            | 높음           | 모든 타입 처리 가능하지만 위험함   |
| 제네릭 사용     | ✅          | ✅           | ❌            | 낮음           | 타입 지정 가능, 안전하고 재사용 가능 |

## 💡 요약
- 타입별 클래스는 안전하지만 확장성이 떨어짐
- `Any` 활용은 유연하지만 타입 오류 위험이 큼
- 제네릭 사용은 재사용성과 안전성을 모두 만족시킴

## 🧠 제네릭 용어와 관례

| 구분             | 설명 또는 예시                                  |
|------------------|--------------------------------------------------|
| 제네릭 타입       | `GenericBox<T>` → 타입 매개변수를 사용하는 클래스 |
| 타입 매개변수     | `T`, `K`, `V` → 타입을 대표하는 변수 이름         |
| 타입 인자         | `Int`, `String` → 실제 지정된 타입               |
| 명명 관례         | `T`, `E`, `K`, `V`, `N` → Type, Element, Key, Value, Number 등 |

## 💡 추가 설명
- GenericBox<T>에서 T는 타입 매개변수
- GenericBox<Int>에서 Int는 타입 인자
- TEKVN은 자주 쓰이는 타입 매개변수 약어 모음이에요:
- T: Type
- E: Element
- K: Key
- V: Value
- N: Number


---


## 📌 함수와 제네릭의 비유

| 구분         | 함수 예시                  | 제네릭 예시               |
|--------------|----------------------------|---------------------------|
| 정의 시점    | `fun greet(msg: String)`   | `class Box<T>`            |
| 사용 시점    | `greet("hello")`           | `Box<String>()`           |
| 전달 대상    | `"hello"` (값 인자)        | `String` (타입 인자)      |


## 💡 핵심 비유
- 함수는 값을 나중에 전달하기 위해 매개변수를 사용
- 제네릭은 타입을 나중에 결정하기 위해 타입 매개변수를 사용

## 🚫 Raw Type (Kotlin에서는 없음)
- Kotlin은 자바의 `Raw Type` 을 `지원하지 않음`
- 타입을 생략하면 Any?로 추론되므로 명시적 타입 지정 권장

## 🐾 제네릭 활용 예제: Animal 클래스
### 1. 기본 클래스 구조
```kotlin
open class Animal(val name: String, val size: Int) {
    open fun sound() = println("동물 울음 소리")
    override fun toString() = "Animal(name='$name', size=$size)"
}

class Dog(name: String, size: Int) : Animal(name, size) {
    override fun sound() = println("멍멍")
}

class Cat(name: String, size: Int) : Animal(name, size) {
    override fun sound() = println("냐옹")
}
```


### 2. 제네릭 Box 클래스
```kotlin
class Box<T> {
    private var value: T? = null
    fun set(value: T) { this.value = value }
    fun get(): T? = value
}
```


### 3. 다양한 타입 저장
```kotlin
val dogBox = Box<Dog>()
dogBox.set(Dog("멍멍이", 100))
println("findDog = ${dogBox.get()}")

val catBox = Box<Cat>()
catBox.set(Cat("냐옹이", 50))
println("findCat = ${catBox.get()}")

val animalBox = Box<Animal>()
animalBox.set(Dog("멍멍이", 100))
animalBox.set(Cat("냐옹이", 50))
println("findAnimal = ${animalBox.get()}")
```


## ✅ 요약
| 방식             | 저장 가능 타입       | 꺼낼 때 타입 | 타입 안전성 | 캐스팅 필요 | 코드 재사용 | 설명 요약                          |
|------------------|----------------------|--------------|--------------|---------------|--------------|-----------------------------------|
| Box<Dog>         | Dog만 가능           | Dog          | ✅           | ❌            | ✅           | 특정 타입만 저장, 안전하고 명확함 |
| Box<Animal>      | Dog, Cat 모두 가능   | Animal       | ✅           | ❌            | ✅           | 다형성 활용, 꺼낼 때는 Animal로 반환 |
| Box<Any>         | 모든 타입 가능       | Any          | ❌           | ✅            | ✅           | 유연하지만 캐스팅 필요, 안전성 낮음 |


# 🧪 문제와 풀이 1: 제네릭 기본
## ✅ 문제1 - 제네릭 기본 1: Container 클래스
### 📌 문제 설명
- Container<T> 클래스는 하나의 값을 저장하고 꺼낼 수 있어야 함
- isEmpty() 메서드를 통해 값이 비어 있는지 확인 가능해야 함
### 💡 테스트 코드
```kotlin
val stringContainer = Container<String>()
println("빈값 확인1: ${stringContainer.isEmpty()}")
stringContainer.set("data1")
println("저장 데이터: ${stringContainer.get()}")
println("빈값 확인2: ${stringContainer.isEmpty()}")

val intContainer = Container<Int>()
intContainer.set(10)
println("저장 데이터: ${intContainer.get()}")
```

### ✅ 실행 결과
```
빈값 확인1: true  
저장 데이터: data1  
빈값 확인2: false  
저장 데이터: 10
```

### 🧩 정답 코드
```kotlin
class Container<T> {
    private var item: T? = null
    fun set(value: T) { item = value }
    fun get(): T? = item
    fun isEmpty(): Boolean = item == null
}
```


## ✅ 문제2 - 제네릭 기본 2: Pair 클래스
### 📌 문제 설명
- Pair<T1, T2> 클래스는 두 개의 값을 저장하고 꺼낼 수 있어야 함
- toString() 메서드를 통해 객체 내용을 문자열로 출력 가능해야 함
### 💡 테스트 코드
```kotlin
val pair1 = Pair(1, "data")
println(pair1.first)
println(pair1.second)
println("pair1 = $pair1")

val pair2 = Pair("key", "value")
println(pair2.first)
println(pair2.second)
println("pair2 = $pair2")
```

#### ✅ 실행 결과
```
1  
data  
pair1 = Pair(first=1, second=data)  
key  
value  
pair2 = Pair(first=key, second=value)
```

### 🧩 정답 코드
```kotlin
data class Pair<T1, T2>(
    var first: T1,
    var second: T2
)
```

----

# 타입 소거

Kotlin에서는 제네릭 클래스 내부에서 타입 매개변수로 배열을 정의할 수 있습니다.  
다만, JVM의 타입 소거(type erasure) 특성 때문에 몇 가지 제약이 있음.

## ✅ 기본 예시: 제네릭 배열 선언
```kotlin
class Box<T>(size: Int) {
    val items: Array<T?> = arrayOfNulls(size)
}
```

- Array<T?>로 선언하면 null을 허용하는 배열을 만들 수 있어요.
- arrayOfNulls(size)는 타입 정보를 런타임에 유지하지 않기 때문에 안전하게 초기화 가능.

### ⚠️ 제약 사항
- Kotlin은 JVM 위에서 동작하기 때문에 런타임에 T의 타입 정보를 알 수 없습니다.
- 따라서 Array<T>를 직접 생성하려면 타입 정보를 명시적으로 전달하거나 reified 키워드를 사용한 인라인 함수가 필요합니다.

## 🧪 고급 예시: reified를 활용한 배열 생성
```kotlin
inline fun <reified T> createArray(size: Int): Array<T?> {
    return arrayOfNulls<T>(size)
}
```

- reified는 인라인 함수에서만 사용 가능
- T의 타입 정보를 런타임에 유지할 수 있어 arrayOfNulls<T>()가 가능해짐

## 📌 요약
| 방식                  | 가능 여부 | 캐스팅 필요 | 설명 요약                                      |
|-----------------------|-----------|--------------|------------------------------------------------|
| Array<T?> + arrayOfNulls | ✅        | ❌           | 가장 일반적인 방식, null 허용 배열 생성 가능     |
| Array<T> 직접 생성     | ❌        | —            | 타입 소거 때문에 불가능, 컴파일 오류 발생        |
| reified T + 인라인 함수 | ✅        | ❌           | 런타임 타입 유지 가능, arrayOfNulls<T>() 사용 가능 |


## ✅ Case 1: Array<T?> + arrayOfNulls(size) — 가능
```kotlin
class Box<T>(size: Int) {
    private val items: Array<T?> = arrayOfNulls(size)

    fun set(index: Int, value: T) {
        items[index] = value
    }

    fun get(index: Int): T? = items[index]

    fun isEmpty(index: Int): Boolean = items[index] == null
}
```

## 💡 설명
- arrayOfNulls(size)는 JVM에서 안전하게 Array<T?>를 생성할 수 있는 방식
- null 허용 배열이므로 초기화 가능
- 타입 소거 문제 없음

## ❌ Case 2: Array<T> 직접 생성 — 불가능 (JVM에서 컴파일 오류)
```kotlin
class Box<T>(size: Int) {
    // ❌ 오류 발생: Cannot create a generic array of T
    private val items: Array<T> = Array(size) { TODO("Cannot instantiate T") }
}
```

### ⚠️ 설명
- JVM은 런타임에 T의 타입 정보를 알 수 없기 때문에 Array<T>를 직접 생성할 수 없음
- 컴파일러가 타입 소거로 인해 T의 구체적인 타입을 추론할 수 없어서 오류 발생

## ✅ Case 3: reified T + 인라인 함수 — 가능 (단, 클래스 외부에서 생성)
```kotlin
inline fun <reified T> createBox(size: Int): Array<T?> {
    return arrayOfNulls<T>(size)
}

// 사용 예
val intBox = createBox<Int>(5)
intBox[0] = 10
println(intBox[0])
```

### 💡 설명
- reified는 인라인 함수에서만 사용 가능
- T의 타입 정보를 런타임에 유지할 수 있어 arrayOfNulls<T>() 호출 가능
- 클래스 내부에서는 reified 사용 불가 → 외부 헬퍼 함수로 처리해야 함

## 📌 요약
| 방식                    | 가능 여부 | JVM 지원 | 캐스팅 필요 | 설명 요약                                      |
|-------------------------|-----------|----------|--------------|------------------------------------------------|
| Array<T?> + arrayOfNulls | ✅        | ✅       | ❌           | null 허용 배열 생성 가능, 가장 일반적인 방식     |
| Array<T> 직접 생성       | ❌        | ❌       | —            | 타입 소거로 인해 생성 불가, 컴파일 오류 발생     |
| reified T + 인라인 함수  | ✅        | ✅       | ❌           | 런타임 타입 유지 가능, 클래스 외부에서만 사용 가능 |



---

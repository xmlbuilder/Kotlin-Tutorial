# Generic / Reflection / 제네릭 상한 경계

이 코드는 **제네릭(Generic)** 과 **리플렉션(Reflection)**, 그리고 **제네릭의 상한 경계(Bounded Type Parameters)** 를  
활용한 예제입니다.
아래는 자바 예제에서 설명했던 내용을 Kotlin 문법과 관점으로 재구성한 문서입니다.  
제네릭(Generic), 리플렉션(Reflection), 상한 경계(Bounded Type Parameters)를 Kotlin 스타일로 해석하고,  
각 클래스와 메서드의 역할을 단계별로 설명합니다.

## 🧬 Kotlin으로 구현한 제네릭 + 리플렉션 + 상한 경계 예제
## 🧩 1. Generator 객체
### 📦 목적
Kotlin에서 리플렉션을 사용해 제네릭 타입의 객체를 생성하는 유틸리티입니다.
### 🔍 코드 분석
```kotlin
object Generator {
    fun <T> genericMethod(cls: Class<T>): T? {
        return try {
            val constructor = cls.getDeclaredConstructor()
            constructor.isAccessible = true
            constructor.newInstance()
        } catch (e: Exception) {
            e.printStackTrace()
            null
        }
    }
}
```

### 🧠 동작 설명
- Class<T> 타입을 받아 해당 클래스의 기본 생성자를 리플렉션으로 호출
- newInstance()를 통해 객체를 생성
- Kotlin에서도 Java 리플렉션 API를 그대로 사용할 수 있음

## 🐾 2. Animal, Cat, Dog 클래스
### 📦 목적
다형성을 활용한 추상 클래스와 그 구현체들입니다.
```kotlin
abstract class Animal {
    abstract fun getSound(): String
}
```
```kotlin
class Cat : Animal() {
    override fun getSound(): String = "Meow"
}
```
```kotlin
class Dog : Animal() {
    override fun getSound(): String = "Woof"
}
```

### 🔍 설명
- Animal은 추상 클래스이며, 모든 하위 클래스는 getSound()를 구현해야 함
- Cat, Dog은 각각 고양이와 개의 울음소리를 반환

## 📦 3. AnimalContainer<T : Animal>
### 📦 목적
제네릭을 사용해 Animal의 하위 타입만 담을 수 있는 컨테이너 클래스입니다.
```kotlin
class AnimalContainer<T : Animal> {
    private val col: MutableList<T> = mutableListOf()

    fun add(t: T) {
        col.add(t)
    }

    fun printAllSounds() {
        for (t in col) {
            println(t.getSound())
        }
    }
}
```

### 🔍 동작 설명
- T : Animal → T는 반드시 Animal을 상속한 타입이어야 함
- add()로 동물을 추가하고, printAllSounds()로 모든 동물의 소리를 출력

## 🧪 4. 실행 예제: BoundedGenericTest
```rust
fun main() {
    val animals = AnimalContainer<Animal>()

    val cat = Generator.genericMethod(Cat::class.java)
    val dog = Generator.genericMethod(Dog::class.java)

    if (cat != null) animals.add(cat)
    if (dog != null) animals.add(dog)

    animals.printAllSounds() // 출력: Meow, Woof
}
```

## 📦 목적
- AnimalContainer를 Animal 타입으로 생성하여 다양한 하위 타입을 담는 예제
- Generator를 통해 Cat, Dog 객체를 리플렉션으로 생성하고 컨테이너에 추가

## 🧠 전체 구조 요약

| 클래스/함수명             | 상속 또는 제약 조건 | 주요 기능 또는 메서드     | 설명                           |
|--------------------------|---------------------|---------------------------|--------------------------------|
| `Generator`              | 없음                | `genericMethod()`         | 리플렉션으로 제네릭 객체 생성 |
| `Animal`                 | 없음                | `getSound()` (추상 메서드) | 모든 동물 클래스의 공통 인터페이스 |
| `Cat`, `Dog`             | `Animal`            | `getSound()` 구현         | 각각 "Meow", "Woof" 반환       |
| `AnimalContainer<T>`     | `T : Animal`        | `add()`, `printAllSounds()` | Animal 하위 타입만 담는 컨테이너 |
| `main()`                 | 없음                | 실행 흐름                 | 객체 생성 및 소리 출력         |


## ✅ 핵심 포인트 요약

| 구성 요소           | Kotlin에서의 역할 또는 설명         | 비고 |
|--------------------|--------------------------------------|------|
| `genericMethod()`  | 리플렉션 기반 제네릭 객체 생성       | `Class<T>`를 받아 인스턴스 생성 |
| `T : Animal`       | 상한 경계 제네릭 선언                | `Animal`을 상속한 타입만 허용 |
| `MutableList<T>`   | 동물 객체 저장소                     | `add()`로 객체 추가 가능       |
| `printAllSounds()` | 다형성을 활용한 동작 출력             | 각 객체의 `getSound()` 호출    |

---



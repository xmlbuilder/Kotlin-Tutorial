# 타입 매개변수 제한 흐름 요약
## 1️⃣ 타입 고정
- 구조: 각각 Dog, Cat 타입에 고정된 병원 클래스
- 장점: 타입 안전성 O (다른 동물 넣으면 컴파일 오류)
- 단점: 코드 중복 심함, 재사용성 X
```kotlin
val dogHospital = DogHospital()
dogHospital.set(Dog(...)) // ✅
dogHospital.set(Cat(...)) // ❌ 컴파일 오류
```

## 2️⃣ 다형성 기반
- 구조: Animal 타입으로 통합
- 장점: 코드 재사용 O
- 단점: 타입 안전성 X (개 병원에 고양이 넣어도 컴파일 오류 없음)
```kotlin
val dogHospital = AnimalHospitalV1()
dogHospital.set(Cat(...)) // ⚠️ 컴파일은 되지만 논리 오류 가능
val dog = dogHospital.getBigger(...) as Dog // ⚠️ 다운캐스팅 필요
```

## 3️⃣ 제네릭 도입
- 구조: 제네릭 <T> 사용
- 장점: 코드 재사용 O
- 단점: 타입 안전성 X, 기능 제한 O
- T가 어떤 타입인지 알 수 없기 때문에 getName(), sound() 호출 불가
- Int, Any도 타입 인자로 들어올 수 있음
```kotlin
val dogHospital = AnimalHospitalV2<Dog>()
val intHospital = AnimalHospitalV2<Int>() // ❌ 논리 오류
```

| 버전                      | 코드 재사용성 | 타입 안전성 | 기능 사용 가능성 | 문제 요약                         | 예시                             |
|---------------------------|----------------|--------------|------------------|-----------------------------------|----------------------------------|
| DogHospital / CatHospital | ❌             | ✅           | ✅               | 클래스 중복 많음                  | `DogHospital.set(Cat())` ❌       |
| AnimalHospitalV1          | ✅             | ❌           | ✅               | 타입 혼동 가능, 다운캐스팅 필요   | `getBigger(...) as Dog` ⚠️       |
| AnimalHospitalV2<T>       | ✅             | ❌           | ❌               | 타입 제한 없음, 기능 호출 불가    | `AnimalHospitalV2<Int>()` ❌     |


## ✅ 해결 방향: 타입 매개변수 제한
다음 단계에서는 제네릭에 타입 제한을 걸어야 합니다:
```kotlin
class AnimalHospitalV3<T : Animal> { ... }
```

- T는 반드시 Animal 또는 그 자식 타입만 가능
- getName(), sound() 등 Animal의 기능을 안전하게 사용 가능
- Int, Any 같은 무관한 타입은 컴파일 오류 발생

## 🧠 타입 매개변수 제한 요약:
### 📌 핵심 문법
```kotlin
class AnimalHospitalV3<T : Animal> { ... }
```

- T : Animal → T는 반드시 Animal 또는 그 자식 클래스여야 함
- 컴파일러는 T가 Animal의 기능을 가진다고 확신할 수 있음
- getName(), getSize(), sound() 같은 메서드 사용 가능
  
| 항목                        | 타입 제한 없음 (`T`)         | 타입 제한 있음 (`T : Animal`)             |
|-----------------------------|------------------------------|--------------------------------------------|
| `dogHospital.set(Cat())`    | 컴파일 가능 (논리 오류 가능) | ❌ 컴파일 오류 → 잘못된 타입 입력 방지     |
| 타입 매개변수 선언          | `T`                          | `T : Animal`                               |
| 기능 호출 가능 여부         | `Animal` 기능 사용 불가       | `getName()`, `sound()`, `getSize()` 가능   |
| 허용 타입 예시              | `Dog`, `Cat`, `Animal`       | `Dog`, `Cat`, `Animal`                     |
| 허용되지 않는 타입 예시     | `Int`, `Any`                 | ❌ 컴파일 오류 → 논리 오류 사전 차단       |


## 🧪 실행 예시
```kotlin
val dogHospital = AnimalHospitalV3<Dog>()
dogHospital.set(Dog("멍멍이1", 100))
dogHospital.checkup()
val biggerDog = dogHospital.getBigger(Dog("멍멍이2", 200))
```

- Dog 타입만 받도록 제한되어 있어 안전하게 작동
- getBigger()는 Dog 타입을 반환하므로 캐스팅 필요 없음

## ❗ 기존 문제와 해결
| 문제 유형         | 타입 제한 없음 (`T`)       | 개선 방식 (`T : Animal`)       | 설명                                       |
|------------------|-----------------------------|----------------------------------|--------------------------------------------|
| 타입 안전성      | `T`                          | `T : Animal`                     | 아무 타입 가능 → Animal 자식만 허용        |
| 기능 사용 가능성 | `T`는 `Any`로 처리됨         | `T`는 `Animal`으로 처리됨        | `Any` 기능만 사용 가능 → Animal 기능 사용 가능 |
| 타입 인자 제한   | `Int`, `Any`도 허용됨        | `Int`, `Any`는 컴파일 오류 발생  | 무관한 타입 차단 → 논리 오류 사전 방지     |


## 🧾 결론
제네릭에 타입 매개변수 상한 (`:`) 을 지정함으로써:
- ✅ 타입 안전성 확보
- ✅ 코드 재사용성 향상
- ✅ 기능 호출 가능성 확보  
  → 실무에서 제네릭을 사용할 때 꼭 기억해야 할 핵심 패턴입니다.

---

## 📦 제네릭 함수란?
- 정의: 함수 단위에서 타입 매개변수를 선언하여 다양한 타입을 처리
- 형식: fun <T> methodName(t: T): T
- 타입 인자 전달 시점: 함수 호출 시점에 타입이 결정됨
| 항목                 | 제네릭 타입 (`class Box<T>`)     | 제네릭 함수 (`fun <T> method(t: T): T`) |
|----------------------|----------------------------------|------------------------------------------|
| 선언 위치            | 클래스 선언부                    | 함수 선언부                              |
| 적용 범위            | 클래스 전체                      | 해당 함수만                              |
| 타입 인자 전달 시점  | 객체 생성 시점                   | 함수 호출 시점                           |
| 타입 추론 가능 여부  | ❌ (명시 필요)                   | ✅ (컴파일러가 인자 기반으로 추론 가능)  |
| static/top-level 적용| ❌ 불가능                        | ✅ 가능                                   |



## 🧪 예제 요약
### ✅ 기본 함수
```kotlin
fun objMethod(obj: Any): Any {
    return obj
}
```

- 어떤 타입이든 Any로 처리
- 타입 안전성 없음

### ✅ 제네릭 함수
```kotlin
fun <T> genericMethod(t: T): T {
    return t
}
```

- 호출 시점에 타입 결정
- 타입 추론 가능

### ✅ 타입 제한된 제네릭 함수
```kotlin
fun <T : Number> numberMethod(t: T): T {
    return t
}
```

- Number와 그 자식만 허용 (Int, Double, Long 등)
- String 등은 컴파일 오류

### 🧠 타입 추론
```kotlin
val result: Int = genericMethod(10) // 타입 추론됨
```

- 컴파일러가 전달된 인자와 반환 타입을 보고 자동으로 타입 결정
- <Int> 생략 가능

### 🐾 제네릭 함수 활용 예: 기능 분리
```kotlin
fun <T : Animal> checkup(t: T) {
    println("${t.name} 건강검진 완료")
}

fun <T : Animal> getBigger(t1: T, t2: T): T {
    return if (t1.size > t2.size) t1 else t2
}
```

- Animal을 상한으로 제한하여 기능 사용 가능
- getName(), sound() 등 안전하게 호출 가능

### ⚠️ 제네릭 타입 vs 제네릭 함수 우선순위
```kotlin
class ComplexBox<T : Animal> {
    fun <T> printAndReturn(t: T): T {
        return t
    }
}
```

- 클래스의 T와 함수의 <T>는 서로 다른 타입 매개변수
- 함수의 <T>가 우선 적용됨 → 상한 없음 → Any로 취급됨
- t.name 호출 불가

### ✅ 해결 방법: 이름 충돌 피하기
```kotlin
fun <Z> printAndReturn(z: Z): Z {
    return z
}
```

- 함수의 타입 매개변수 이름을 다르게 설정하여 혼동 방지

| 항목                     | 기본 제네릭 함수 `<T>`             | 타입 제한된 제네릭 함수 `<T : Animal>` |
|--------------------------|------------------------------------|----------------------------------------|
| 함수 선언 형식           | `fun <T> method(t: T): T`          | `fun <T : Animal> method(t: T): T`     |
| 타입 제한 여부           | 없음                               | `Animal` 또는 그 하위 타입만 허용       |
| 기능 호출 가능 여부      | 제한적 (`Any`로 처리됨)             | `getName()`, `sound()` 등 호출 가능     |
| 타입 안전성              | 낮음                               | 높음                                   |
| 사용 예시                | `genericMethod(123)`               | `checkup(Dog("멍멍이", 100))`           |

---

## 🧭 와일드카드란?
- `*` 기호는 Java에서 제네릭 타입 인자에 유연성을 부여하는 방식
- Kotlin에서는 `out`, `in` 키워드를 사용해 동일한 개념을 표현함
- 이미 만들어진 제네릭 타입을 매개변수로 받을 때 사용
- 제네릭 타입이나 제네릭 함수 자체를 정의하는 것이 아님

## 와일드카드 종류
| 와일드카드 종류       | Kotlin 문법           | 허용되는 타입 예시                        |
|----------------------|------------------------|-------------------------------------------|
| 비제한 와일드카드    | `Box<*>`               | `Box<Any>`, `Box<Dog>`, `Box<Cat>`        |
| 상한 와일드카드      | `Box<out Animal>`      | `Box<Animal>`, `Box<Dog>`, `Box<Cat>`     |
| 하한 와일드카드      | `Box<in Animal>`       | `Box<Animal>`, `Box<Any>`                 |


## 제네릭 함수 vs 와일드카드
| 항목                   | 제네릭 메서드 `<T>`                  | Kotlin `*`, `out`, `in`             |
|------------------------|---------------------------------------|------------------------------------------|
| 타입 선언 위치         | `<T>` 메서드 선언부에 직접 선언       | 타입 인자 위치에서 `*`, `out`, `in` 사용 |
| 타입 이름              | `T`                                   | `*`, `out`, `in`                         |
| 기능 호출 가능 여부    | `getName()`, `sound()` 가능 (`T : Animal`) | `Animal` 기능 호출 가능 (`out Animal`)   |
| 반환 타입              | `T`                                   | `Animal`                                 |


## 📦 예제 비교
### ✅ 제네릭 함수
```kotlin
fun <T : Animal> printAndReturnGeneric(box: Box<T>): T {
    val unit = box.get()
    println("이름: ${unit.name}")
    return unit
}
```

- val dog: Dog = printAndReturnGeneric(dogBox) → 정확한 타입 반환

### ✅ 와일드카드 (out)
```kotlin
fun printAndReturnWildcard(box: Box<out Animal>): Animal {
    val unit = box.get()
    println("이름: ${unit.name}")
    return unit
}
```

- val animal: Animal = printAndReturnWildcard(dogBox) → 상한 타입으로 반환

### 📌 하한 와일드카드 예시 (in)
```kotlin
fun writeBox(box: Box<in Animal>) {
    box.set(Dog("멍멍이", 100))
}
```

- Box<Any> ✅
- Box<Animal> ✅
- Box<Dog> ❌
→ Animal 이상의 타입만 허용, 하위 타입은 불가

## 🧾 정리
| Kotlin 문법         | 의미 설명                     |
|---------------------|------------------------------|
| `Box<*>`            | `*`는 `out Any?`와 동일 → 모든 타입 허용 |
| `Box<out Type>`     | `Type` 및 그 하위 타입만 허용 |
| `Box<in Type>`      | `Type` 및 그 상위 타입만 허용 |


# 🧽 타입 이레이저란?
- "이레이저(Eraser)"는 지우개라는 뜻
- 자바의 제네릭은 컴파일 시점에만 존재하고, .class 파일에는 타입 정보가 없음
- Kotlin도 JVM 위에서 동작하므로 동일한 타입 이레이저가 적용됨

## 🔄 컴파일 전후 변화 예시
### 📦 컴파일 전
```java
class GenericBox<T> {
    private var value: T? = null
    fun set(value: T) { this.value = value }
    fun get(): T? = value
}
```

### 🧱 컴파일 후 (JVM 바이트코드)
```kotlin
class GenericBox {
    private Any value;
    public void set(Any value) { this.value = value; }
    public Any get() { return value; }
}
```

- T → Any 변환됨
- 반환 시 컴파일러가 자동으로 다운캐스팅 코드 추가

## 🐾 타입 제한이 있는 경우
### 컴파일 전
```kotlin
class AnimalHospitalV3<T : Animal> {
    fun getBigger(target: T): T { ... }
}
```

### 컴파일 후
```kotlin
class AnimalHospitalV3 {
    Animal getBigger(Animal target) { ... }
}
```

- T : Animal → Animal로 대체됨
- 반환 시 Dog로 캐스팅 필요 → 컴파일러가 자동 추가

## 🚫 런타임 제약
```kotlin
class EraserBox<T> {
    fun instanceCheck(param: Any): Boolean {
        return param is T // ❌ 오류
    }

    fun create(): T {
        return T() // ❌ 오류
    }
}
```

- T는 런타임에 Any로 처리됨
- is T → 항상 Any와 비교됨 → 의미 없음
- T() → 생성 불가

## ✅ 요약
| 항목         | 컴파일 전 (`T`)            | 컴파일 후 (`타입 이레이저`)     |
|--------------|----------------------------|----------------------------------|
| 타입 매개변수 | `T`                        | `Any`                            |
| 타입 변환     | `T → Any`                  | `T`는 런타임에 `Any`로 처리됨    |
| 타입 제한     | `T : Animal`               | `Animal`                         |
| 제네릭 타입  | `Box<Int>`                 | `Box`                            |
| `is T` 사용  | `T`                        | `Any` → 비교 불가                |
| `T()` 생성   | `T()`                      | `Any()` → 생성 불가              |

---

# 🧪 문제와 풀이 요약
## 상속 전제
```kotlin
open class BioUnit(val name: String, val hp: Int)

class Marine(name: String, hp: Int) : BioUnit(name, hp)
class Zealot(name: String, hp: Int) : BioUnit(name, hp)
class Zergling(name: String, hp: Int) : BioUnit(name, hp)

```

## 1️⃣ 제네릭 함수 + 상한 제한
### 📌 목표: 두 유닛 중 체력이 높은 유닛 반환
```kotlin
fun <T : out BioUnit> maxHp(t1: T, t2: T): T {
    return if (t1.hp > t2.hp) t1 else t2
}
```

- 타입 안전성 확보
- 다양한 유닛 타입에 재사용 가능

## 2️⃣ 제네릭 타입 + 상한 제한
### 📌 목표: 셔틀에 유닛을 태우고 정보 출력
```kotlin
class Shuttle<T : out BioUnit> {
    private var unit: T? = null

    fun inUnit(t: T) { unit = t }
    fun outUnit(): T? = unit
    fun showInfo() {
        unit?.let { println("이름: ${it.name}, HP: ${it.hp}") }
    }
}
```

- inUnit(), outUnit()으로 유닛 관리
- showInfo()에서 getName(), getHp() 호출 가능

## 3️⃣ 제네릭 함수 vs 와일드카드
| 항목                 | 제네릭 함수 `<T : BioUnit>`       | Kotlin 변성 `Box<out BioUnit>`         |
|----------------------|------------------------------------|----------------------------------------|
| 선언 위치            | 함수 선언부에 `<T>`                | 타입 인자 위치에서 `out` 사용          |
| 타입 지정 방식       | 호출 시점에 타입 인자 추론 또는 명시 | 컴파일 시점에 타입 추론               |
| 반환 타입            | `T`                                | `BioUnit`                              |
| 기능 호출 가능 여부  | `T.name`, `T.hp` 가능               | `BioUnit.name`, `BioUnit.hp` 가능      |
| 타입 제어 유연성     | 높음 (입력과 출력 타입 일치 가능)   | 낮음 (출력 타입은 상한 타입으로 고정됨) |

---

# 실무 사용

## 📦 실무 활용 정리 (Kotlin 기준)
| 항목               | Kotlin 컬렉션 예시                         | 변성 키워드 (`in`, `out`) 사용 예시         |
|--------------------|--------------------------------------------|---------------------------------------------|
| 주요 컬렉션 타입    | `List`, `Map`, `Set`, `Queue`              | 제네릭 타입에 따라 `in`, `out` 적용 가능     |
| 제네릭 선언 방식    | `List<T>`, `Map<K, V>`, `Set<T>`, `Queue<T>` | `List<out T>`, `MutableList<in T>` 등        |
| 변성 키워드 목적    | 타입 안전한 읽기/쓰기 제어                 | `out` → 읽기 전용, `in` → 쓰기 전용          |

## ✅ 결론
- Kotlin에서도 제네릭은 타입 안정성과 재사용성을 높여주는 핵심 도구
- 실무에서는 복잡한 설계보다 간단하고 명확한 활용이 중요
- 지금까지 학습한 제네릭 문법과 패턴은 실무에서 충분히 활용 가능
- 특히 Kotlin의 in, out, * 와 같은 변성(variance) 개념은 와일드카드의 대체 개념으로 자주 등장

---

# Kotlin Native Generic

Kotlin Native에서는 JVM 기반이 아니기 때문에 Java나 Kotlin/JVM에서 발생하는 타입 이레이저(type erasure) 문제가 존재하지 않습니다.  
즉, Kotlin Native에서는 제네릭 타입 정보가 런타임까지 유지됩니다.
## ✅ Kotlin Native에서의 제네릭 특징
- 타입 이레이저 없음 → 런타임에 타입 정보를 확인할 수 있음
- is T, T::class, T::class.simpleName 같은 표현이 정상 작동
- new T[10] 같은 배열 생성도 가능하지만, Kotlin에서는 일반적으로 arrayOfNulls<T>(size) 또는 Array<T?>(size) { null } 형태로 사용
### 📦 예시: Kotlin Native에서 타입 유지
```kotlin
inline fun <reified T> createArray(size: Int): Array<T?> {
    println("타입: ${T::class.simpleName}")
    return arrayOfNulls<T>(size)
}
val arr = createArray<String>(5) // 타입: String
```

- reified 키워드를 사용하면 타입 인자를 런타임에 참조 가능
- Kotlin/JVM에서는 이 기능이 inline 함수에만 제한되지만, Kotlin Native에서는 더 자유롭게 활용 가능

## ⚠️ JVM과의 차이
| 항목           | Kotlin/JVM                      | Kotlin Native                      |
|----------------|----------------------------------|------------------------------------|
| 타입 이레이저   | `T → Object`                    | 타입 정보 유지됨 (`T` 그대로 존재) |
| `is T` 사용     | ❌ 불가능                        | ✅ 가능                             |
| `new T[10]`     | `new Object[10]`                | `arrayOfNulls<T>(10)` 가능         |
| `reified` 사용 | `inline` 함수에서만 가능         | 일반 함수에서도 타입 확인 가능     |


---

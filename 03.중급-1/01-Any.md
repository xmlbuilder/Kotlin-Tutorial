# 🧱 Any 클래스와 다형성

## 1️⃣ kotlin 패키지란?
- 코틀린의 기본 구성 요소를 담고 있는 패키지
- 대부분의 핵심 클래스는 kotlin 패키지에 포함
- Any, String, Int, Unit 등은 자동으로 import됨

## 📦 kotlin 패키지 — 대표 클래스 요약
| 클래스 / 함수                  | 설명                                                                 |
|-------------------------------|----------------------------------------------------------------------|
| `Any`                         | 모든 코틀린 클래스의 최상위 부모. `toString()`, `equals()` 등 제공     |
| `String`                      | 문자열을 다루는 클래스. 불변(immutable) 객체                          |
| `Int`, `Double`, `Boolean` 등 | 기본 타입이지만 객체처럼 동작. 메서드와 연산자 오버로딩 지원           |
| `Unit`                        | 반환값이 없는 함수의 반환 타입. 자바의 `void`와 유사                   |
| `Nothing`                     | 정상적으로 반환되지 않는 함수의 반환 타입. 예: `throw` 이후             |
| `KClass`                      | 클래스의 메타 정보를 담는 타입. 리플렉션에 사용됨 (`::class`)          |
| `measureTimeMillis`           | 코드 실행 시간을 측정하는 함수. 성능 테스트에 유용                     |


## 2️⃣ Any 클래스의 역할
### ✅ 묵시적 상속
```kotlin
class Parent {
    fun parentMethod() = println("Parent")
}
```

- 위 코드는 사실상 : Any가 생략된 형태
- 부모 클래스가 없으면 코틀린이 자동으로 Any를 상속시킴
### ✅ 명시적 상속
```kotlin
open class Parent
class Child : Parent()
```

- 이미 Parent를 상속했기 때문에 Any는 자동으로 상속됨

## 3️⃣ Any 클래스의 주요 기능
| 메서드        | 설명                                                                 |
|---------------|----------------------------------------------------------------------|
| `toString()`  | 객체의 정보를 문자열로 반환. 디버깅이나 출력 시 유용함               |
| `equals()`    | 두 객체가 같은지 비교. 내용 기반 비교를 위해 오버라이딩 가능          |
| `hashCode()`  | 객체의 해시코드를 반환. 컬렉션에서 키로 사용할 때 필요                |


### 예제
```kotlin
val child = Child()
println(child.toString()) // Any의 메서드 호출
```


## 4️⃣ Any 다형성
### ✅ 모든 객체를 참조할 수 있음
```kotlin
val dog: Any = Dog()
val car: Any = Car()
```
- Any는 모든 클래스의 부모 → 어떤 객체든 담을 수 있음

### ✅ 다형성 활용 예제
```kotlin
fun action(obj: Any) {
    when (obj) {
        is Dog -> obj.sound()
        is Car -> obj.move()
    }
}
```
- Any 타입으로 받아서 스마트 캐스트 후 실제 타입의 메서드 호출
### ❌ 다형성의 한계
```kotlin
obj.sound() // 오류: Any에는 sound()가 없음
```
- 해결: is 체크 + 스마트 캐스트

## 🧠 정리 — kotlin 패키지와 Any 클래스

| 항목               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| kotlin 패키지      | 코틀린의 기본 클래스들이 포함된 핵심 패키지. 자동 import됨             |
| Any 클래스         | 모든 코틀린 클래스의 최상위 부모. 공통 기능과 다형성 지원               |
| 상속 구조          | 부모 클래스가 없으면 묵시적으로 `Any`를 상속                           |
| 주요 메서드        | `toString()`, `equals()`, `hashCode()` — 정보 출력, 비교, 해시 지원       |
| 다형성 지원        | `Any`는 모든 객체를 참조할 수 있어 다양한 타입을 하나로 처리 가능         |
| 다형성의 한계      | `Any` 타입으로는 자식 클래스의 고유 메서드 호출 불가 → 스마트 캐스트 필요 |

---

# 🧠 Kotlin의 Any 클래스와 OCP 원칙
## 1️⃣ Any가 없다면?
- 서로 다른 클래스(Car, Dog 등)에 공통 부모가 없다면 객체 정보를 출력하기 위해 클래스마다 전용 메서드를 만들어야 함
```kotlin
class BadObjectPrinter {
    fun print(car: Car) = println(car.carInfo())
    fun print(dog: Dog) = println(dog.dogInfo())
}
```


### ❌ 문제점
- 구체적인 클래스에 의존
- 클래스가 늘어날수록 메서드도 계속 추가되어야 함 → 유지보수 어려움

## 2️⃣ Any와 toString()의 역할
```kotlin
class ObjectPrinter {
    fun print(obj: Any) = println("객체 정보 출력: ${obj.toString()}")
}
```

### ✅ 장점
- 추상적인 Any에 의존
- toString() 오버라이딩을 통해 각 객체의 정보를 런타임에 출력 가능

## 3️⃣ OCP 원칙과 ObjectPrinter
| 항목               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| OCP 원칙           | 확장에는 열려(Open), 변경에는 닫혀(Closed) → 유지보수에 유리           |
| 추상적 의존        | `Any` 타입에 의존하여 다양한 객체를 처리 가능                          |
| 핵심 메서드        | `toString()` — 모든 객체가 오버라이딩 가능                             |
| 다형적 참조        | `Any` 타입으로 다양한 객체를 참조 가능                                 |
| 런타임 정보 출력   | `toString()` 오버라이딩을 통해 객체의 고유 정보 출력 가능               |
| ObjectPrinter 클래스 | `Any`에 의존하여 모든 객체를 받아 `toString()`으로 출력하는 유틸리티 클래스 |



## 4️⃣ equals() — 동일성과 동등성
| 비교 방식 | 설명                                                                 |
|-----------|----------------------------------------------------------------------|
| `===`     | 동일성 비교. 두 객체의 참조값(메모리 주소)이 같은지 확인              |
| `==`      | 동등성 비교. 내부적으로 `equals()`를 호출하여 논리적으로 같은지 확인   |
| `equals()`| 논리적 동등성 비교를 위한 메서드. 오버라이딩하여 의미 기반 비교 가능    |


### 예제
```kotlin
val user1 = User("id-100") // 참조 x001
val user2 = User("id-100") // 참조 x002

println(user1 === user2) // false (동일성: 참조값 다름)
println(user1 == user2)  // false (동등성: equals 미오버라이딩)
```

## 📚 요약 — Kotlin의 Any 클래스와 객체지향 핵심 개념

| 항목                  | 설명                                                                 |
|-----------------------|----------------------------------------------------------------------|
| kotlin 패키지         | 코틀린의 기본 클래스들이 포함된 핵심 패키지. 자동 import됨             |
| Any 클래스            | 모든 코틀린 클래스가 상속받는 최상위 부모 클래스                       |
| toString()            | 객체 정보를 문자열로 반환. 디버깅, 로깅에 유용                           |
| equals()              | 논리적 동등성 비교. 오버라이딩을 통해 의미 기반 비교 가능               |
| hashCode()            | 객체의 해시코드 반환. 컬렉션에서 키로 사용할 때 필요                    |
| 다형성 지원           | 다양한 타입의 객체를 하나의 `Any` 타입으로 참조 가능                     |
| 다형성의 한계         | `Any` 타입으로는 자식 클래스의 고유 메서드 호출 불가 → 스마트 캐스트 필요 |
| OCP 원칙              | 확장에는 열려 있고(Open), 변경에는 닫혀 있음(Closed) → 유지보수에 유리     |

----

# 🧠 Kotlin의 equals() 메서드와 동등성 비교 정리
## 1️⃣ 기본 equals() 동작 — 동일성 비교
```kotlin
val user1 = User("id-100")
val user2 = User("id-100")

println(user1 === user2) // 참조값 비교 → false
```

- ===는 참조값(주소)을 비교 → 동일성(Identity)
- ==는 내부적으로 equals()를 호출 → 동등성(Equality)
### 실행 흐름 예시
```
user1 == user2
    → user1.equals(user2)
    → "id-100" == "id-100"
    → true
```


## 2️⃣ 동등성 비교의 필요성
- 동등성(Equality) 은 객체의 논리적 동일함을 의미
- 예: 회원 번호, 주민등록번호, 이메일 등으로 비교
- 클래스마다 동등성 기준이 다르므로 equals()를 오버라이딩해야 함

## 3️⃣ 간단한 equals() 구현 예 — UserV2
```kotlin
data class UserV2(val id: String)
```

- data class는 equals(), hashCode(), toString() 자동 생성
- id 값이 같으면 논리적으로 같은 객체로 간주
### 결과 예시
```kotlin
val user1 = UserV2("id-100")
val user2 = UserV2("id-100")

println(user1 === user2) // false (동일성)
println(user1 == user2)  // true (동등성)
```


## 4️⃣ 정확한 equals() 구현 예 — 수동 정의
```kotlin
class User(val id: String) {
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (other !is User) return false
        return id == other.id
    }

    override fun hashCode(): Int {
        return id.hashCode()
    }
}
```

- Any? 타입으로 받고 스마트 캐스트
- hashCode()도 함께 오버라이딩해야 컬렉션에서 일관성 유지

## 5️⃣ equals() 구현 시 지켜야 할 규칙
| 규칙 이름  | 설명                                                                 |
|------------|----------------------------------------------------------------------|
| 반사성     | `x.equals(x)`는 항상 `true`                                          |
| 대칭성     | `x.equals(y)`가 `true`이면 `y.equals(x)`도 `true`                    |
| 추이성     | `x.equals(y)`와 `y.equals(z)`가 `true`이면 `x.equals(z)`도 `true`    |
| 일관성     | 객체의 상태가 변하지 않으면 `equals()` 결과도 변하지 않아야 함       |
| null 비교  | `x.equals(null)`은 항상 `false`                                      |



## 6️⃣ 실습 예제 — Rectangle 클래스
### 요구사항
- width, height가 같으면 동등한 객체로 간주
- toString()과 equals() 오버라이딩 필요
### 코드
```kotlin
class Rectangle(val width: Int, val height: Int) {
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (other !is Rectangle) return false
        return width == other.width && height == other.height
    }

    override fun hashCode(): Int {
        return 31 * width + height
    }

    override fun toString(): String {
        return "Rectangle(width=$width, height=$height)"
    }
}
```

### 실행 결과
```kotlin
val r1 = Rectangle(100, 20)
val r2 = Rectangle(100, 20)

println(r1.toString()) // Rectangle(width=100, height=20)
println(r2.toString()) // Rectangle(width=100, height=20)
println(r1 === r2)     // false
println(r1 == r2)      // true
```


## 7️⃣ Any 클래스의 기타 메서드 요약
| 메서드             | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| `equals()`         | 논리적 동등성 비교. 오버라이딩 필요                                   |
| `hashCode()`       | 객체의 해시값 반환. `equals()`와 함께 사용. 컬렉션에서 중요             |
| `toString()`       | 객체 정보를 문자열로 반환. 디버깅, 로깅에 유용                           |
| `javaClass`        | 객체의 클래스 정보 반환. 리플렉션 등에 사용                            |






# 🧠 Kotlin Class 요약
## 📌 개요
Kotlin에서 KClass는 클래스의 메타데이터(정보)를 다루기 위한 도구로,  
실행 중인 애플리케이션에서 클래스의 구조와 동작을 동적으로 조회하고 조작할 수 있게 해줍니다.  

## 🔧 주요 기능
- 타입 정보 조회: 클래스 이름, 슈퍼클래스, 인터페이스, 접근 제한자 등 확인 가능
- 리플렉션 (Reflection):
- 프로퍼티, 함수, 생성자 조회
- 객체 생성 및 함수 호출 가능
- 동적 로딩 및 인스턴스 생성:
- Class.forName()으로 클래스 로딩
- getDeclaredConstructor().newInstance()로 객체 생성
- 애노테이션 처리: 클래스에 적용된 애노테이션을 조회하고 활용 가능
## 🔍 KClass 객체 얻는 방법
```kotlin
val clazz = String::class // 클래스에서 직접
val clazz = "Hello".javaClass.kotlin // 인스턴스에서
val clazz = Class.forName("java.lang.String").kotlin // 문자열로
```

### 클래스 생성 코드
```kotlin
package lang.clazz
class Hello {
    fun hello(): String {
        return "hello!"
    }
}

package lang.clazz
fun main() {
    val helloClass = Class.forName("lang.clazz.Hello").kotlin
    val hello = helloClass.constructors.first().call() as Hello
    val result = hello.hello()
    println("result = $result")
}
```

### 🧪 예제 코드 요약
```kotlin
val clazz = String::class
clazz.members.forEach { println("Member: $it") }
println("Superclass: ${clazz.java.superclass.name}")
clazz.java.interfaces.forEach { println("Interface: ${it.name}") }

val helloClass = Class.forName("lang.clazz.Hello").kotlin
val hello = helloClass.constructors.first().call() as Hello
println("result = ${hello.hello()}")
```

### 🧑‍💻 용어 팁
- class는 Kotlin 예약어이므로 변수명으로 사용할 수 없음 → 관례적으로 clazz 사용

### 🪞 리플렉션이란?
클래스의 구조를 런타임에 분석하고 조작하는 기능.  
객체 생성, 함수 호출, 애노테이션 처리 등 다양한 동적 작업이 가능하며, 프레임워크에서 널리 활용됨.


---

# 🧪 Kotlin 리플렉션 예제 단계별 설명
### 🔹 1. 클래스 정보 조회
```kotlin
val clazz = String::class
```

- String::class는 KClass<String> 타입의 객체를 반환합니다.
- 이는 Kotlin에서 클래스의 메타데이터를 다루는 기본 방식입니다.
- KClass는 자바의 Class와 유사하지만 Kotlin에 최적화되어 있습니다.

### 🔹 2. 클래스의 멤버 조회
```kotlin
clazz.members.forEach { println("Member: $it") }
```
- members는 클래스에 정의된 모든 프로퍼티, 함수, 생성자 등을 포함합니다.
- 이 코드는 String 클래스의 모든 멤버를 출력합니다.

### 🔹 3. 슈퍼클래스 정보 조회
```kotlin
println("Superclass: ${clazz.java.superclass.name}")
```

- clazz.java는 KClass를 자바의 Class로 변환합니다.
- superclass는 해당 클래스의 부모 클래스를 반환합니다.
- 예: String 클래스의 슈퍼클래스는 Object

### 🔹 4. 인터페이스 정보 조회
```kotlin
clazz.java.interfaces.forEach { println("Interface: ${it.name}") }
```

- interfaces는 클래스가 구현한 인터페이스 목록을 반환합니다.
- 예: String 클래스는 Serializable, Comparable 등을 구현함

## 🎯 동적 클래스 로딩 및 인스턴스 생성
### 🔹 5. 클래스 로딩
```kotlin
val helloClass = Class.forName("lang.clazz.Hello").kotlin
```

- Class.forName()은 문자열로 클래스 이름을 받아 자바의 Class 객체를 반환합니다.
- .kotlin을 붙이면 이를 Kotlin의 KClass로 변환합니다.
- 즉, lang.clazz.Hello 클래스를 런타임에 동적으로 로딩합니다.

### 🔹 6. 생성자 호출 및 인스턴스 생성
```kotlin
val hello = helloClass.constructors.first().call() as Hello
```
- constructors.first()는 클래스의 첫 번째 생성자를 선택합니다.
- call()은 해당 생성자를 호출하여 인스턴스를 생성합니다.
- 생성된 객체는 Hello 타입으로 캐스팅됩니다.

### 🔹 7. 메서드 호출
```kotlin
val result = hello.hello()
println("result = $result")
```

- 생성된 Hello 객체의 hello() 메서드를 호출합니다.
- 결과는 "hello!" 문자열이며 콘솔에 출력됩니다.

### ✅ 전체 흐름 요약
| 단계 | 설명                           |
|------|--------------------------------|
| 1    | 클래스 메타정보 조회 (`KClass`) |
| 2    | 멤버, 슈퍼클래스, 인터페이스 출력 |
| 3    | 문자열로 클래스 로딩 (`Class.forName`) |
| 4    | 생성자 호출로 인스턴스 생성     |
| 5    | 생성된 객체의 메서드 실행       |


---

# ⚙️ Kotlin System 클래스 요약
## 📌 개요
Kotlin은 자바의 System 클래스를 그대로 활용하여 시스템 관련 기능을 제공합니다.
대부분의 메서드는 정적(static)으로 선언되어 있어 바로 호출할 수 있습니다.
## 🔧 주요 기능

### 🕒 시간 측정
- System.currentTimeMillis() : 현재 시간을 밀리초 단위로 반환
- System.nanoTime() : 현재 시간을 나노초 단위로 반환

### 🌱 환경 변수
- System.getenv() : 운영체제에서 설정한 환경 변수들을 Map 형태로 반환
🛠️ 시스템 속성
- System.getProperties() : 시스템 전체 속성 정보를 Properties 객체로 반환
- System.getProperty("key") : 특정 시스템 속성 조회

### 📤 표준 스트림
- System.in, out, err` : 각각 표준 입력, 출력, 오류 스트림

### 🚀 프로그램 종료
- System.exit(status) : 프로그램 종료
- 0 : 정상 종료
- 0 이외 : 오류 종료

### 📦 배열 고속 복사
- System.arraycopy() : 배열을 빠르게 복사하는 메서드

### 🧪 예제 요약
```kotlin
val currentTimeMillis = System.currentTimeMillis()
val currentTimeNano = System.nanoTime()

println("env = ${System.getenv()}")
println("properties = ${System.getProperties()}")
println("Java version = ${System.getProperty("java.version")}")

val originalArray = charArrayOf('h', 'e', 'l', 'l', 'o')
val copiedArray = CharArray(5)
System.arraycopy(originalArray, 0, copiedArray, 0, originalArray.size)

println("copiedArray = ${copiedArray.contentToString()}")

System.exit(0)
```

---

# 🧮 Kotlin Math 클래스 요약
## 📌 개요
Kotlin은 자바의 Math 클래스를 그대로 활용하여 다양한 수학 연산을 제공합니다.

## 🔢 주요 메서드 분류
### 1. 기본 연산
- abs(x) : 절대값
- max(a, b) : 최대값
- min(a, b) : 최소값
### 2. 지수 및 로그
- exp(x) : e^x 계산
- log(x) : 자연 로그
- log10(x) : 로그 10
- pow(a, b) : a의 b 제곱
### 3. 반올림 및 정밀도
- ceil(x) : 올림
- floor(x) : 내림
- rint(x) : 가장 가까운 정수로 반올림
- round(x) : 반올림
### 4. 삼각 함수
- sin(x), cos(x), tan(x)
### 5. 기타 유용한 기능
- sqrt(x) : 제곱근
- cbrt(x) : 세제곱근
- Math.random() : 0.0 ~ 1.0 사이의 난수
💡 고정밀 계산이 필요할 경우 BigDecimal 사용을 고려하세요.
### 예제 코드
```kotlin
fun main() {
    println("max(10, 20): ${Math.max(10, 20)}")
    println("min(10, 20): ${Math.min(10, 20)}")
    println("abs(-10): ${Math.abs(-10)}")

    println("ceil(2.1): ${Math.ceil(2.1)}")
    println("floor(2.7): ${Math.floor(2.7)}")
    println("round(2.5): ${Math.round(2.5)}")

    println("sqrt(4): ${Math.sqrt(4.0)}")
    println("random(): ${Math.random()}")
}
```

---

# 🎲 Kotlin Random 클래스 요약
## 📌 개요
Kotlin은 kotlin.random.Random 클래스를 제공하며, 자바의 java.util.Random도 사용 가능
더 정교한 난수 제어가 가능하며 다양한 타입의 난수를 생성할 수 있습니다.
## 🔧 주요 메서드
- nextInt() : 전체 범위의 int 난수
- nextDouble() : 0.0 ~ 1.0 사이의 double 난수
- nextBoolean() : true 또는 false
- nextInt(bound) : 0 ~ bound 미만의 int 난수

## 🌱 씨드(Seed) 개념
- Random(seed) : 동일한 seed 사용 시 항상 같은 결과
- 테스트 코드, 게임 지형 생성 등에 유용
```kotlin
val random = java.util.Random(1)
println(random.nextInt()) // 항상 같은 값 출력
```

### 예제
```kotlin
fun main() {
    val random = java.util.Random()

    println("randomInt: ${random.nextInt()}")
    println("randomDouble: ${random.nextDouble()}")
    println("randomBoolean: ${random.nextBoolean()}")

    println("0 ~ 9: ${random.nextInt(10)}")
    println("1 ~ 10: ${random.nextInt(10) + 1}")
}
```
---

# 🧪 Kotlin 문제와 풀이 1: Wrapper 클래스 활용
## ✅ 문제 1: toInt()로 문자열 숫자 더하기
### 🎯 목표: 문자열 "10"과 "20"을 정수로 변환하여 합산
### 🔧 핵심 메서드: toInt()
```kotlin
fun main() {
    val str1 = "10"
    val str2 = "20"
    val num1 = str1.toInt()
    val num2 = str2.toInt()
    val sum = num1 + num2
    println("두 수의 합: $sum") // 출력: 두 수의 합: 30
}
```


## ✅ 문제 2: toDouble()로 배열의 합 구하기
### 🎯 목표: 문자열 배열 {"1.5", "2.5", "3.0"}의 합산
### 🔧 핵심 메서드: toDouble()
```kotlin
fun main() {
    val array = arrayOf("1.5", "2.5", "3.0")
    var sum = 0.0
    for (s in array) {
        sum += s.toDouble()
    }
    println("sum = $sum") // 출력: sum = 7.0
}
```


## ✅ 문제 3: 박싱/언박싱 (수동)
### 🎯 목표: 문자열 → Int, Int → Integer, Integer → Int
### ⚠️ 오토 박싱/언박싱 사용 금지
```kotlin
fun main() {
    val str = "100"
    val integer1: Int = str.toInt() // String → Int
    println("integer1 = $integer1")

    val boxed: java.lang.Integer = Integer.valueOf(integer1) // Int → Integer
    println("boxed = $boxed")

    val unboxed: Int = boxed.toInt() // Integer → Int
    println("unboxed = $unboxed")
}
```


## ✅ 문제 4: 오토 박싱/언박싱
### 🎯 목표: 동일한 변환을 Kotlin의 자동 박싱/언박싱으로 처리
```kotlin
fun main() {
    val str = "100"
    val integer1 = str.toInt() // String → Int
    println("integer1 = $integer1")

    val boxed: Int? = integer1 // 오토 박싱
    println("boxed = $boxed")

    val unboxed: Int = boxed!! // 오토 언박싱
    println("unboxed = $unboxed")
}
```

---

# 🎲 Kotlin 문제와 풀이 2: 로또 번호 자동 생성기
## ✅ 문제 설명
- 1~45 사이의 숫자 중 중복 없이 6개를 랜덤으로 추출
- 실행할 때마다 결과가 달라야 함
### 🔧 핵심 구현
- Random.nextInt(45) + 1 : 1~45 범위의 난수 생성
- contains() : 중복 검사
- generateLottoNumbers() : 로또 번호 생성

```kotlin
import kotlin.random.Random

class LottoGenerator {
    private val random = Random.Default

    fun generate(): List<Int> {
        val lottoNumbers = mutableSetOf<Int>()
        while (lottoNumbers.size < 6) {
            val number = random.nextInt(1, 46) // 1 ~ 45
            lottoNumbers.add(number) // 중복 자동 제거
        }
        return lottoNumbers.toList()
    }
}
```

#### 실행 코드
```kotlin
fun main() {
    val generator = LottoGenerator()
    val lottoNumbers = generator.generate()
    println("로또 번호: ${lottoNumbers.joinToString(" ")}")
}
```

#### 🧪 실행 예시
```
로또 번호: 11 19 21 35 29 16
```
#### 💡 Kotlin의 Set(`mutableSetOf`) 은 중복을 허용하지 않기 때문에 중복 검사 로직이 간단해집니다.




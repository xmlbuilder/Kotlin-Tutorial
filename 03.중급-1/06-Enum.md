# Enum Type

아래는 Kotlin의 열거형(Enum Type)이 왜 등장했는지, 어떤 문제를 해결하기 위해 만들어졌는지를 예제 흐름에 따라 설명한 문서입니다.  
타입 안정성과 값 제한의 필요성을 중심으로, Enum의 탄생 배경과 의미를 명확하게 전달하도록 구성했습니다:

## 🧭 자바 Enum 타입의 탄생 배경과 필요성

## 📌 비즈니스 요구사항
고객은 3개의 등급으로 나뉘며, 상품 구매 시 등급에 따라 할인율이 적용됩니다.
할인 금액은 소수점 이하를 버린 정수로 계산됩니다:
| 등급     | 할인율 | 예시 구매 금액 | 할인 금액 |
|----------|--------|----------------|------------|
| BASIC    | 10%    | 10,000원        | 1,000원     |
| GOLD     | 20%    | 10,000원        | 2,000원     |
| DIAMOND  | 30%    | 10,000원        | 3,000원     |


예: GOLD 고객이 10,000원을 구매하면 할인 금액은 2,000원입니다.

## 구현 코드
```kotlin
class DiscountService {
    fun discount(grade: String, price: Int): Int {
        val discountPercent = when (grade) {
            "BASIC" -> 10
            "GOLD" -> 20
            "DIAMOND" -> 30
            else -> {
                println("$grade: 할인X")
                0
            }
        }
        return price * discountPercent / 100
    }
}
```


## 🚨 문제 1: 문자열로 등급을 표현한 방식의 한계
### 🔧 구현 예시
```kotlin
discount("GOLD", 10000) // → 2000
```

### ⚠️ 발생 가능한 문제
- "VIP" : 존재하지 않는 등급
- "DIAMONDD" : 오타
- "gold" : 대소문자 불일치

### ❌ 문자열 사용의 단점
- 타입 안정성 부족: 오타나 잘못된 값이 컴파일 시점에 감지되지 않음
- 데이터 일관성 저하: "GOLD", "gold", "Gold" 등 다양한 표현 가능
- 값 제한 불가능: 어떤 문자열이든 전달 가능
- 런타임 오류 발생 가능성: 잘못된 값은 실행 중에야 확인됨

## 🛠️ 대안 1: 문자열 상수 사용
```kotlin
object StringGrade {
    const val BASIC = "BASIC"
    const val GOLD = "GOLD"
    const val DIAMOND = "DIAMOND"
}
```

### 👍 장점
- 오타 방지
- 코드 가독성 향상
### 👎 여전히 남는 문제
- 잘못된 값 전달 가능
- 컴파일러가 유효성 확인 불가
- 상수 정의 위치를 개발자가 직접 알아야 함

## 🧱 해결책: 타입 안전 열거형 패턴
### 🔨 직접 구현한 타입 안전 열거형
```kotlin
class ClassGrade private constructor(val discountPercent: Int) {
    companion object {
        val BASIC = ClassGrade(10)
        val GOLD = ClassGrade(20)
        val DIAMOND = ClassGrade(30)
    }
}
```

### ✅ 장점
- ClassGrade 타입으로만 값 전달 가능
- BASIC, GOLD, DIAMOND 외의 값은 생성 불가
- == 비교로 참조값 일치 확인 가능

---

# 🧠 Kotlin Enum 클래스의 등장
## ✅ Enum 정의
```kotlin
enum class Grade(val discountPercent: Int) {
    BASIC(10),
    GOLD(20),
    DIAMOND(30)
}
```

### 💸 할인 서비스 예제
```kotlin
fun discount(grade: Grade, price: Int): Int {
    return when (grade) {
        Grade.BASIC -> price * 10 / 100
        Grade.GOLD -> price * 20 / 100
        Grade.DIAMOND -> price * 30 / 100
    }
}
```


### 🧪 실행 예제
```kotlin
fun main() {
    val price = 10000
    println("BASIC 등급의 할인 금액: ${discount(Grade.BASIC, price)}")
    println("GOLD 등급의 할인 금액: ${discount(Grade.GOLD, price)}")
    println("DIAMOND 등급의 할인 금액: ${discount(Grade.DIAMOND, price)}")
}
```


## ✅ Enum의 장점 요약
| 항목             | 설명                                                                 |
|------------------|----------------------------------------------------------------------|
| 타입 안정성 향상 | 잘못된 값은 컴파일 시점에 오류로 감지됨                              |
| 데이터 일관성    | enum 상수만 사용되므로 오타 없이 일관된 값 유지 가능                   |
| 코드 간결성      | 조건문 없이 속성 조회 및 로직 처리 가능                                |
| 확장성           | 새로운 값 추가 시 enum에 상수만 추가하면 됨                            |
| switch 문 지원   | Kotlin에서는 `when` 문으로 enum 분기 처리 가능                         |
| 외부 생성 불가   | enum은 자동으로 생성자 은닉 처리됨                                     |


## 🔧 열거형 리팩토링 흐름 정리
### 🧱 리팩토링 1: 클래스 기반 열거형 개선
```kotlin
class ClassGrade private constructor(val discountPercent: Int) {
    companion object {
        val BASIC = ClassGrade(10)
        val GOLD = ClassGrade(20)
        val DIAMOND = ClassGrade(30)
    }
}

class DiscountService {
    fun discount(classGrade: ClassGrade, price: Int): Int {
        return price * classGrade.discountPercent / 100
    }
}
```


### 🧱 리팩토링 2: Kotlin Enum으로 구조 개선
```kotlin
enum class Grade(val discountPercent: Int) {
    BASIC(10),
    GOLD(20),
    DIAMOND(30);

    fun discount(price: Int): Int {
        return price * discountPercent / 100
    }
}
```


### 🧱 리팩토링 3: 서비스 제거 및 출력 개선
```kotlin
fun printDiscount(grade: Grade, price: Int) {
    println("${grade.name} 등급의 할인 금액: ${grade.discount(price)}")
}

fun main() {
    val price = 10000
    Grade.values().forEach { printDiscount(it, price) }
}
```

---

## 🧪 문제와 풀이 1: 인증 등급 열거형
### ✅ 문제 1: 인증 등급 만들기
```kotlin
enum class AuthGrade(val level: Int, val description: String) {
    GUEST(1, "손님"),
    LOGIN(2, "로그인 회원"),
    ADMIN(3, "관리자")
}
```


### ✅ 문제 2: 인증 등급 조회
```kotlin
fun main() {
    AuthGrade.values().forEach {
        println("grade=${it.name}, level=${it.level}, 설명=${it.description}")
    }
}
```


### ✅ 문제 3: 인증 등급 활용
```kotlin
fun main() {
    print("당신의 등급을 입력하세요[GUEST, LOGIN, ADMIN]: ")
    val input = readlnOrNull()?.uppercase()
    try {
        val grade = AuthGrade.valueOf(input!!)
        println("당신의 등급은 ${grade.description}입니다.")
        println("==메뉴 목록==")
        if (grade.level > 0) println("- 메인 화면")
        if (grade.level > 1) println("- 이메일 관리 화면")
        if (grade.level > 2) println("- 관리자 화면")
    } catch (e: IllegalArgumentException) {
        println("잘못된 등급입니다.")
    }
}
```


## 🌐 문제와 풀이 2: HTTP 상태 코드 열거형
### ✅ HttpStatus 열거형
```kotlin
enum class HttpStatus(val code: Int, val message: String) {
    OK(200, "OK"),
    BAD_REQUEST(400, "Bad Request"),
    NOT_FOUND(404, "Not Found"),
    INTERNAL_SERVER_ERROR(500, "Internal Server Error");

    fun isSuccess(): Boolean = code in 200..299

    companion object {
        fun findByCode(code: Int): HttpStatus? {
            return values().find { it.code == code }
        }
    }
}
```


### ✅ 활용 예제
```kotlin
fun main() {
    print("HTTP CODE: ")
    val input = readlnOrNull()?.toIntOrNull()
    if (input != null) {
        val status = HttpStatus.findByCode(input)
        if (status == null) {
            println("정의되지 않은 코드")
        } else {
            println("${status.code} ${status.message}")
            println("isSuccess = ${status.isSuccess()}")
        }
    } else {
        println("숫자를 입력해주세요.")
    }
}
```


## ✅ 열거형 설계의 장점 요약
| 항목               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| 타입 안정성        | 잘못된 값은 컴파일 시점에 오류로 감지됨                              |
| 데이터 일관성      | 열거형 상수만 사용되므로 오타 없이 일관된 값 유지 가능                   |
| 캡슐화             | 열거형 내부에서 데이터와 로직을 함께 관리                               |
| 확장성             | 새로운 값 추가 시 기존 로직 수정 없이 자동 반영 가능                    |
| 코드 간결성        | 조건문 없이 속

---


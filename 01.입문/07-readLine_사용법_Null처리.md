# Kotlin 사용자 입력 처리
## 🧾 기본 입력 방법
```kotlin
val input = readLine() // 한 줄 입력 받기
```
## 📌 주요 입력 처리 방식
| 입력 대상     | 입력 처리 방식             | 변환 결과 타입 |
|---------------|----------------------------|----------------|
| 문자열 입력   | `readLine()`               | `String?`      |
| 정수 입력     | `readLine()!!.toInt()`     | `Int`          |
| 실수 입력     | `readLine()!!.toDouble()`  | `Double`       |


## 🔁 반복 입력 예제
### 예제 1: 문자열 반복 입력
```kotlin

while (true) {
    print("문자열을 입력하세요(exit: 종료): ")
    val str = readLine()
    if (str == "exit") {
        println("프로그램을 종료합니다.")
        break
    }
    println("입력한 문자열: $str")
}
```


## 🧪 문제와 풀이 1
### 이름과 나이 입력
```kotlin
print("당신의 이름을 입력하세요: ")
val name = readLine()

print("당신의 나이를 입력하세요: ")
val age = readLine()!!.toInt()

println("당신의 이름은 $name 이고, 나이는 $age 살입니다.")
```


### 홀수 짝수 판별
```kotlin
print("하나의 정수를 입력하세요: ")
val number = readLine()!!.toInt()

if (number % 2 == 0) {
    println("입력한 숫자 $number 는 짝수입니다.")
} else {
    println("입력한 숫자 $number 는 홀수입니다.")
}
```

### 음식 주문 총 가격 계산
```kotlin
print("음식 이름을 입력해주세요: ")
val foodName = readLine()

print("음식의 가격을 입력해주세요: ")
val foodPrice = readLine()!!.toInt()

print("음식의 수량을 입력해주세요: ")
val foodQuantity = readLine()!!.toInt()

val totalPrice = foodPrice * foodQuantity
println("$foodName $foodQuantity 개를 주문하셨습니다. 총 가격은 $totalPrice 원입니다.")
```

### 구구단 출력
```kotlin
print("구구단의 단 수를 입력해주세요: ")
val n = readLine()!!.toInt()

println("${n}단의 구구단:")
for (i in 1..9) {
    println("$n x $i = ${n * i}")
}
```


## 🧪 문제와 풀이 2
### 변수 값 교환
```kotlin
var a = 10
var b = 20

val temp = a
a = b
b = temp

println("a = $a")
println("b = $b")
```

### 사이 숫자 출력
```kotlin
print("첫 번째 숫자를 입력하세요: ")
var num1 = readLine()!!.toInt()

print("두 번째 숫자를 입력하세요: ")
var num2 = readLine()!!.toInt()

if (num1 > num2) {
    val temp = num1
    num1 = num2
    num2 = temp
}


print("두 숫자 사이의 모든 정수: ")
for (i in num1..num2) {
    print(i)
    if (i != num2) print(",")
}
println()
```


## 🧪 문제와 풀이 3
### 이름과 나이 반복 입력
```kotlin
while (true) {
    print("이름을 입력하세요 (종료를 입력하면 종료): ")
    val name = readLine()
    if (name == "종료") {
        println("프로그램을 종료합니다.")
        break
    }
    print("나이를 입력하세요: ")
    val age = readLine()!!.toInt()
    println("입력한 이름: $name, 나이: $age")
}
```

### 상품 가격 계산 반복
```kotlin
while (true) {
    print("상품의 가격을 입력하세요 (-1을 입력하면 종료): ")
    val price = readLine()!!.toInt()
    if (price == -1) {
        println("프로그램을 종료합니다.")
        break
    }
    print("구매하려는 수량을 입력하세요: ")
    val quantity = readLine()!!.toInt()
    println("총 비용: ${price * quantity}")
}
```


## 🧪 문제와 풀이 4
### 입력한 숫자의 합계와 평균
```kotlin
var sum = 0
var count = 0

println("숫자를 입력하세요. 입력을 중단하려면 -1을 입력하세요:")
while (true) {
    val input = readLine()!!.toInt()
    if (input == -1) break
    sum += input
    count++
}

val average = sum.toDouble() / count
println("입력한 숫자들의 합계: $sum")
println("입력한 숫자들의 평균: $average")
```


### 상품 구매 프로그램
```kotlin

var totalCost = 0
while (true) {
    println("1: 상품 입력, 2: 결제, 3: 프로그램 종료")
    val option = readLine()!!.toInt()

    when (option) {
        1 -> {
            print("상품명을 입력하세요: ")
            val product = readLine()!!

            print("상품의 가격을 입력하세요: ")
            val price = readLine()!!.toInt()

            print("구매 수량을 입력하세요: ")
            val quantity = readLine()!!.toInt()

            totalCost += price * quantity
            println("상품명: $product 가격: $price 수량: $quantity 합계: ${price * quantity}")
        }
        2 -> {
            println("총 비용: $totalCost")
            totalCost = 0
        }
        3 -> {
            println("프로그램을 종료합니다.")
            break
        }
        else -> println("올바른 옵션을 선택해주세요.")
    }
}
```

## 📥 정리
- Kotlin에서는 readLine()으로 문자열 입력을 받고, toInt(), toDouble()으로 변환
- 예외 처리를 위해 toIntOrNull() 같은 안전한 변환 함수도 활용 가능
- 반복문과 조건문을 활용하면 자바보다 더 간결하고 직관적인 입력 프로그램을 만들 수 있음

---

# 단정 연산자

!!는 non-null 단정 연산자라고 불려.  
의미는 다음과 같아:

## ❗ !!의 의미: "이 값은 절대 null이 아니야!"
- readLine() 같은 함수는 String?을 반환해 → 즉, null일 수도 있다는 뜻
- !!를 붙이면 Kotlin에게 **"이 값은 null이 아니라고 확신하니까 그냥 써!"** 라고 말하는 거야
- 만약 실제로 null이면? → NullPointerException 발생

### ✅ 예시
```kotlin
val input: String = readLine()!! // null이 아니라고 단정
val number: Int = readLine()!!.toInt() // 문자열 → 정수 변환
```


### ⚠️ 주의사항
- !!는 편리하지만 위험할 수 있어
- 실무에서는 ?.let {} 또는 ?: 같은 안전한 null 처리 방식이 더 권장돼
### 안전한 방식 예시
```kotlin
val input = readLine()
val number = input?.toIntOrNull()
if (number != null) {
    println("입력한 숫자: $number")
} else {
    println("올바른 숫자를 입력해주세요.")
}
```

## 📥 Kotlin Null-Safe 연산자 비교 요약
| 연산자 / 함수 | 역할 또는 기능 설명                          | 예시 코드                         | 안전성 |
|----------------|---------------------------------------------|-----------------------------------|--------|
| `!!`           | null 아님을 단정 (null이면 예외 발생)        | `val x = name!!`                  | ❌ 위험 |
| `?.`           | null이면 무시, 아니면 접근                    | `val len = name?.length`         | ✅ 안전 |
| `?:`           | null이면 기본값 반환 (엘비스 연산자)         | `val result = name ?: "기본값"`  | ✅ 안전 |
| `let`          | null이 아닐 때만 블록 실행 (`?.let {}`)      | `name?.let { println(it) }`      | ✅ 안전 |
| `run`          | 객체 컨텍스트에서 블록 실행 (`?.run {}`)     | `name?.run { length }`           | ✅ 안전 |
| `apply`        | 객체 설정 시 사용 (`?.apply {}`)             | `name?.apply { println(this) }`  | ✅ 안전 |


## 🔍 각 연산자 자세히 보기
### 1. !! — Non-null 단정 연산자
```kotlin
val name: String? = null
val length = name!!.length // ❌ NullPointerException 발생
```
- null이 아님을 확신할 때만 사용
- 실무에서는 지양하는 편

### 2. ?. — Safe Call 연산자
```kotlin
val name: String? = null
val length = name?.length // null 반환
```
- null이면 전체 표현식이 null
- 체이닝 가능: user?.profile?.email

### 3. ?: — 엘비스 연산자 (기본값 제공)
```kotlin
val name: String? = null
val result = name ?: "이름 없음" // "이름 없음"
```

- null일 경우 대체값 반환
- val len = name?.length ?: 0 처럼 조합 가능

### 4. let — null이 아닐 때만 실행
```kotlin
val name: String? = "JungHwan"
name?.let {
    println("이름 길이: ${it.length}")
}
```

- it으로 블록 내부에서 값 접근
- 주로 null 체크 후 안전한 작업에 사용

### 5. run — 객체 컨텍스트에서 블록 실행
```kotlin
val name: String? = "Copilot"
val len = name?.run {
    println("이름: $this")
    length
}
```

- this로 객체 참조
- 블록의 마지막 값이 반환됨

### 6. apply — 객체 설정 시 사용
```kotlin
val builder = StringBuilder().apply {
    append("Hello, ")
    append("JungHwan!")
}
println(builder.toString()) // Hello, JungHwan!
```

- 객체 초기화나 설정 시 유용
- this로 객체 참조, 반환값은 원래 객체

---


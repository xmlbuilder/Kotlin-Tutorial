# 📦 Kotlin Regular Expression 예제
## ✅ 1. 숫자만 포함된 문자열 검사
```kotlin
val pattern = Regex("^[0-9]+$")
val input = "123456"
println(pattern.matches(input)) // true
```


## ✅ 2. 이메일 주소 유효성 검사
```kotlin
val emailPattern = Regex("^[\\w.-]+@[\\w.-]+\\.[a-zA-Z]{2,}$")
val email = "test.email@domain.co.kr"
println(emailPattern.matches(email)) // true
```


## ✅ 3. 전화번호 형식 검사 (010-xxxx-xxxx)
```kotlin
val phonePattern = Regex("^010-\\d{4}-\\d{4}$")
val phone = "010-1234-5678"
println(phonePattern.matches(phone)) // true
```


## ✅ 4. 특정 단어 포함 여부
```kotlin
val logPattern = Regex(".*(error|fail|exception).*")
val log = "System error occurred at line 42"
println(logPattern.matches(log)) // true
```


## ✅ 5. 날짜 형식 검사 (YYYY-MM-DD)
```kotlin
val datePattern = Regex("^\\d{4}-\\d{2}-\\d{2}$")
val date = "2025-10-24"
println(datePattern.matches(date)) // true
```


## ✅ 6. 문자열에서 숫자 추출
```kotlin
val line = "Order ID: QT3000X"
val numberPattern = Regex("\\d+")
numberPattern.findAll(line).forEach {
    println("Found number: ${it.value}")
}
```


## ✅ 7. HTML 태그 제거
```kotlin
val html = "<p>Hello <b>World</b></p>"
val cleaned = html.replace(Regex("<[^>]*>"), "")
println(cleaned) // Hello World
```


## ✅ 8. 공백 기준으로 단어 분리
```kotlin
val sentence = "Java is powerful"
val words = sentence.split(Regex("\\s+"))
println(words) // [Java, is, powerful]
```


## ✅ 9. 문장에서 숫자 추출 (그룹 활용)
```kotlin
val line = "This order was placed for QT 3000! OK?"
val pattern = Regex("(.*?)(\\d+)(.*)")
val match = pattern.find(line)

if (match != null) {
    println("Group 0: ${match.groupValues[0]}")
    println("Group 1: ${match.groupValues[1]}")
    println("Group 2: ${match.groupValues[2]}")
    println("Group 3: ${match.groupValues[3]}")
} else {
    println("NO MATCH")
}
```
---





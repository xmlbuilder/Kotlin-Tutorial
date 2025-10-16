# apply / also / let / run / with
Kotlin의 스코프 함수인 apply, also, let, run, with는 객체를 다룰 때 코드의 가독성과 간결함을 높여주는 도구.  
아래에 각각의 특징과 간단한 샘플을 정리.

## 🧪 공통 예제용 클래스
```kotlin
class User {
    var name: String = ""
    var age: Int = 0
}
```


## 1️⃣ apply – 객체 구성에 사용 (this 사용, 객체 반환)
```kotlin
val user = User().apply {
    name = "JungHwan"
    age = 30
}
```

- 객체를 생성하고 바로 속성을 설정할 때 사용
- this가 생략되어 내부 프로퍼티 설정이 간결함
- 반환값: 객체 자체

## 2️⃣ also – 부가 작업에 사용 (it 사용, 객체 반환)
```kotlin
val user = User().also {
    it.name = "JungHwan"
    it.age = 30
    println("User 설정 완료: ${it.name}")
}
```

- 객체를 설정하면서 로깅, 디버깅, 부가 작업을 할 때 유용
- 반환값: 객체 자체

## 3️⃣ let – null-safe 처리, 체이닝 (it 사용, 결과 반환)
```kotlin
val name: String? = "JungHwan"

val length = name?.let {
    println("이름: $it")
    it.length
}
```

- null-safe 처리에 자주 사용 (?.let)
- 블록의 마지막 표현식이 반환값

## 4️⃣ run – 계산, 초기화, 컨텍스트 실행 (this 사용, 결과 반환)
```kotlin
val greeting = User().run {
    name = "JungHwan"
    age = 30
    "안녕하세요, $name님!"
}
```

- 객체 설정 후 결과값을 반환하고 싶을 때
- 반환값: 블록의 마지막 표현식

## 5️⃣ with – 이미 존재하는 객체에 여러 작업 수행 (this 사용, 결과 반환)
val user = User()
```kotlin
val intro = with(user) {
    name = "JungHwan"
    age = 30
    "이름: $name, 나이: $age"
}
```

- 객체를 인자로 받아 여러 작업을 수행할 때
- 반환값: 블록의 마지막 표현식

## 📌 요약 비교

| 함수   | 수신 객체 접근 방식 | 반환값         | 주 용도               | 특징                          |
|--------|----------------------|----------------|------------------------|-------------------------------|
| apply  | this                 | 객체 자체      | 객체 구성              | 초기화에 자주 사용             |
| also   | it                   | 객체 자체      | 부가 작업              | 로깅, 디버깅에 유용             |
| let    | it                   | 결과값         | null-safe, 체이닝      | ?.let 으로 안전하게 처리 가능   |
| run    | this                 | 결과값         | 계산, 컨텍스트 실행    | 블록 마지막 값 반환             |
| with   | this                 | 결과값         | 여러 작업 수행         | 인자로 객체를 넘겨서 처리       |

---

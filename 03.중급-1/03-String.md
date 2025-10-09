# 🧵 Kotlin String 클래스 - 핵심 정리
## 1️⃣ 문자 vs 문자열
| 타입        | 예시 또는 설명                                |
|-------------|-----------------------------------------------|
| `Char`      | `'a'` — 문자 하나를 표현하는 기본형            |
| `CharArray` | `charArrayOf('h','e','l','l','o')` — 문자 배열 |
| `String`    | `"hello"` — 문자열을 표현하는 클래스           |

```kotlin
val charArr = charArrayOf('h','e','l','l','o')
val str = "hello"
```
## 2️⃣ 문자열 생성 방식

| 방식               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| `"hello"`          | 문자열 리터럴. 동일한 문자열은 같은 참조값 사용. 문자열 풀에서 재사용됨 |
| `String("hello")`  | Kotlin에서는 직접 사용하지 않음. 대신 `String(charArrayOf(...))` 등으로 생성 가능 |

```kotlin
val str1 = "hello"
val str2 = String(charArrayOf('h','e','l','l','o')) // 드물게 사용
```


## 3️⃣ String 클래스 구조
```kotlin
class String {
    val length: Int
    fun substring(startIndex: Int, endIndex: Int): String
    fun indexOf(str: String): Int
    ...
}
```

- Kotlin에서는 length는 속성으로 사용 (str.length)
- 내부 구조는 JVM 기반으로 자바와 유사

## 4️⃣ 주요 메서드

| 메서드 또는 연산         | Kotlin 사용 방식                         |
|--------------------------|------------------------------------------|
| `length`                 | `str.length` — 속성으로 접근              |
| `get(index)` 또는 `[index]` | `str[0]` — 배열처럼 문자 접근           |
| `substring(start, end)`  | `str.substring(0, 5)` — 부분 문자열 추출  |
| `indexOf(String)`        | `str.indexOf("text")` — 위치 검색         |
| `lowercase()` / `uppercase()` | `str.lowercase()` / `str.uppercase()` |
| `trim()`                 | `str.trim()` — 양쪽 공백 제거             |
| `plus(String)` 또는 `+`  | `str + "추가"` 또는 `str.plus("추가")`    |



## 5️⃣ 문자열 연결
```kotlin
val a = "hello"
val b = " kotlin"
val result1 = a.plus(b)
val result2 = a + b
```

- + 연산자 또는 plus() 메서드로 연결 가능

## 6️⃣ 문자열 비교
| Kotlin 비교 방식 | 설명 또는 자바 대응 방식             |
|------------------|--------------------------------------|
| `==`             | 문자열 **내용 비교** (`equals()`와 동일) |
| `===`            | **참조값 비교** (자바의 `==`와 동일)     |

```kotlin
val str1 = String(charArrayOf('h','e','l','l','o'))
val str2 = String(charArrayOf('h','e','l','l','o'))
println(str1 == str2)    // true (내용 비교)
println(str1 === str2)   // false (참조 비교)
```


## 7️⃣ 문자열 풀 (String Pool)
- Kotlin도 JVM 기반이므로 문자열 리터럴은 풀에서 재사용됨
- "hello"는 동일한 참조값을 공유
```kotlin
val str3 = "hello"
val str4 = "hello"
println(str3 === str4) // true
```


## 8️⃣ 실무 팁 — 문자열 처리 시 주의사항

| 상황 또는 목적               | Kotlin에서의 권장 방식 또는 설명                         |
|-----------------------------|----------------------------------------------------------|
| 문자열 비교                 | `==` 사용 (내용 비교). 참조 비교는 `===` 사용             |
| 문자열 연결이 적을 때       | `+` 연산자 사용. 컴파일 시 자동 최적화됨                  |
| 문자열 연결이 많을 때       | `StringBuilder` 또는 `buildString {}` 사용                |
| 문자열 객체 생성            | 리터럴 `"..."` 사용 권장. 별도 생성은 거의 필요 없음       |
| 멀티쓰레드 환경에서 연결    | `StringBuffer` 사용 가능 (자바 API 활용 시)               |

----


# 🧵 String 클래스 - 불변 객체 핵심 정리
## 1️⃣ 불변 객체란?
- 한 번 생성되면 내부 상태가 변경되지 않는 객체
- Kotlin의 String도 불변이며, 수정 시 새로운 객체 생성

## 2️⃣ 문자열 연결 예시
```kotlin
val str1 = "hello"
val str2 = str1 + " kotlin"
println("str1 = $str1") // hello
println("str2 = $str2") // hello kotlin
```

- str1은 그대로 유지됨
- + 연산은 새로운 문자열 객체 생성

## 3️⃣ 불변 설계의 이유

| 항목               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| 사이드 이펙트 방지  | 공유된 문자열이 변경되면 다른 참조도 영향을 받음 → 불변으로 차단       |
| 문자열 풀 최적화   | 동일한 리터럴은 풀에서 재사용 → 값이 바뀌면 풀의 안정성 무너짐         |
| 멀티쓰레드 안전성  | 상태 변경이 없으므로 동기화 없이도 안전하게 공유 가능                  |
| 자료구조 호환성    | `equals()`와 `hashCode()`가 안정적 → 해시 기반 구조에서 유리함         |



## 4️⃣ 문자열 풀과 불변성
```kotlin
val str3 = "hello"
val str4 = "hello"
println(str3 === str4) // true
```

- "hello"는 풀에서 공유됨
- 불변이기 때문에 공유해도 안전함


---

# 🧵 String 클래스 - 주요 메서드 ① 정리
## 1️⃣ 문자열 정보 조회

| 메서드 또는 접근 방식     | Kotlin 사용 방식                         |
|---------------------------|------------------------------------------|
| `length`                  | `str.length` — 문자열 길이 속성           |
| `isEmpty()`               | `str.isEmpty()` — 비어 있는지 확인        |
| `isBlank()`               | `str.isBlank()` — 공백 포함 비어 있는지 확인 |
| `get(index)` 또는 `[index]` | `str[0]` — 인덱스로 문자 접근           |


### 예시
```kotlin
fun main() {
    val str = "Hello, Kotlin!"
    println("문자열의 길이: ${str.length}")
    println("문자열이 비어 있는지: ${str.isEmpty()}")
    println("문자열이 비어 있거나 공백인지1: ${str.isBlank()}")
    println("문자열이 비어 있거나 공백인지2: ${" ".isBlank()}")
    val c = str[7]
    println("7번 인덱스의 문자: $c")
}
```


## 2️⃣ 문자열 비교

| 비교 방식 또는 메서드        | Kotlin 사용 방식 또는 설명                                 |
|-----------------------------|--------------------------------------------------------------|
| `==`                        | 문자열 **내용 비교**. 자바의 `equals()`와 동일하게 동작       |
| `equals()`                  | `str1.equals(str2)` — 내용 비교. `==`와 동일한 결과            |
| `equalsIgnoreCase()`        | `str1.equals(str2, ignoreCase = true)` — 대소문자 무시 비교     |
| `compareTo()`               | `str1.compareTo(str2)` — 사전 순 비교                         |
| `startsWith()` / `endsWith()` | `str.startsWith("Hello")`, `str.endsWith("Kotlin!")` — 접두/접미 확인 |


### 예시
```kotlin
fun main() {
    val str1 = "Hello, Kotlin!"
    val str2 = "hello, kotlin!"
    val str3 = "Hello, World!"

    println("str1 == str2: ${str1 == str2}")
    println("str1 equalsIgnoreCase str2: ${str1.equals(str2, ignoreCase = true)}")
    println("'b' compareTo 'a': ${"b".compareTo("a")}")
    println("str1 compareTo str3: ${str1.compareTo(str3)}")
    println("str1 starts with 'Hello': ${str1.startsWith("Hello")}")
    println("str1 ends with 'Kotlin!': ${str1.endsWith("Kotlin!")}")
}
```

## 3️⃣ 문자열 검색
| 메서드 또는 기능           | Kotlin 사용 방식 또는 설명                                 |
|----------------------------|--------------------------------------------------------------|
| `contains()`               | `str.contains("Java")` — 특정 문자열 포함 여부 확인           |
| `indexOf()`                | `str.indexOf("Java")` — 처음 등장 위치 반환                   |
| `indexOf(String, Int)`     | `str.indexOf("Java", 10)` — 지정 위치부터 검색 시작           |
| `lastIndexOf()`            | `str.lastIndexOf("Java")` — 마지막 등장 위치 반환             |

### 예시
```kotlin
fun main() {
    val str = "Hello, Kotlin! Welcome to Kotlin world."
    println("문자열에 'Kotlin' 포함?: ${str.contains("Kotlin")}")
    println("'Kotlin' 첫 번째 인덱스: ${str.indexOf("Kotlin")}")
    println("인덱스 10부터 'Kotlin' 인덱스: ${str.indexOf("Kotlin", 10)}")
    println("'Kotlin' 마지막 인덱스: ${str.lastIndexOf("Kotlin")}")
}
```

---

# 🧵 Kotlin String 클래스 - 주요 메서드 ② 정리
## 1️⃣ 문자열 조작 및 변환
| 메서드                              | Kotlin 사용 방식 또는 설명                                         |
|-------------------------------------|--------------------------------------------------------------------|
| `substring(begin, end)`             | `str.substring(0, 5)` — 부분 문자열 추출                           |
| `plus(String)` 또는 `+`            | `str + "추가"` 또는 `str.plus("추가")` — 문자열 연결               |
| `replace(target, replacement)`      | `str.replace("Java", "Kotlin")` — 문자열 치환                      |
| `replace(Regex, replacement)`       | `str.replace(Regex("J\\w+"), "Kotlin")` — 정규식 치환              |
| `replaceFirst(Regex, replacement)`  | `str.replaceFirst(Regex("J\\w+"), "Kotlin")` — 첫 항목만 치환      |
| `lowercase()` / `uppercase()`       | `str.lowercase()` / `str.uppercase()` — 대소문자 변환              |
| `trim()`                            | `str.trim()` — 양쪽 공백 제거                                      |
| `trimStart()` / `trimEnd()`         | `str.trimStart()` / `str.trimEnd()` — 앞/뒤 공백 제거              |



## 2️⃣ 문자열 분할 및 조합
```kotlin
fun main() {
    val str = "Apple,Banana,Orange"

    // split()
    val splitStr = str.split(",")
    splitStr.forEach { println(it) }

    // joinToString()
    val joinedStr = listOf("A", "B", "C").joinToString("-")
    println("연결된 문자열: $joinedStr")

    val result = splitStr.joinToString("-")
    println("result = $result")
}
```

### 📌 예시 결과:
```
Apple
Banana
Orange
연결된 문자열: A-B-C
result = Apple-Banana-Orange
```

## 3️⃣ 기타 유틸리티
| 메서드                        | Kotlin 사용 방식 또는 설명                                     |
|-------------------------------|----------------------------------------------------------------|
| `valueOf(Object obj)`         | `obj.toString()` 또는 `"$obj"` — 문자열로 변환                  |
| `toCharArray()`               | `str.toCharArray()` — 문자 배열로 변환                          |
| `format(String, Object...)`   | `"숫자: %d".format(10)` 또는 `String.format(...)` 사용 가능     |
| `matches(String regex)`       | `"Hello123".matches(Regex("\\w+\\d+"))` — 정규식 일치 확인      |


### Kotlin 예시
```kotlin
fun main() {
    val num = 100
    val bool = true
    val obj = Any()
    val str = "Hello, Kotlin!"

    println("숫자의 문자열 값: ${num.toString()}")
    println("불리언의 문자열 값: ${bool.toString()}")
    println("객체의 문자열 값: ${obj.toString()}")
    println("빈문자열 + num: " + num)

    val strCharArray = str.toCharArray()
    println("문자열을 문자 배열로 변환: ${strCharArray.joinToString("")}")

    val formatted = "num: %d, bool: %b, str: %s".format(num, bool, str)
    println(formatted)

    val matchResult = "Hello123".matches(Regex("\\w+\\d+"))
    println("'str'이 패턴과 일치하는가? $matchResult")
}

```
### 📌 예시 결과:
```
숫자의 문자열 값: 100
불리언의 문자열 값: true
객체의 문자열 값: java.lang.Object@...
빈문자열 + num:100
Hello, Kotlin!
num: 100, bool: true, str: Hello, Kotlin!
'str'이 패턴과 일치하는가? true
```
---


# 🧵 Kotlin에서의 가변 문자열 처리
## 1️⃣ 불변 String 클래스의 단점
- String은 불변 → 수정 시마다 새로운 객체 생성
- 반복 연결 시 성능 저하 및 메모리 낭비 발생
val str = "A" + "B" + "C" + "D" // 중간 객체 생성됨



## 2️⃣ 해결책: StringBuilder 또는 buildString {}
- 내부적으로 CharArray를 사용해 직접 수정
- 객체 재사용으로 성능 향상

## 🧵 StringBuilder - 주요 메서드
| 메서드               | 설명                                      | 반환 타입     |
|----------------------|-------------------------------------------|---------------|
| `append(str)`        | 문자열 추가                               | `StringBuilder` |
| `insert(pos, str)`   | 특정 위치에 삽입                          | `StringBuilder` |
| `delete(start, end)` | 범위 삭제                                 | `StringBuilder` |
| `reverse()`          | 문자열 뒤집기                             | `StringBuilder` |
| `toString()`         | 최종 문자열 반환                          | `String`        |



## 3️⃣ 사용
```kotlin
fun main() {
    val sb = StringBuilder()
    sb.append("A").append("B").append("C").append("D")
    println("sb = $sb") // ABCD

    sb.insert(4, "Kotlin")
    println("insert = $sb") // ABCDKotlin

    sb.delete(4, 10)
    println("delete = $sb") // ABCD

    sb.reverse()
    println("reverse = $sb") // DCBA

    val result = sb.toString()
    println("string = $result") // DCBA
}
```


## 🔁 가변 vs 불변 비교
| 항목             | String (불변)               | StringBuilder (가변)           |
|------------------|-----------------------------|--------------------------------|
| 변경 가능 여부    | ❌ 불가능                    | ✅ 가능                         |
| 성능              | 느림                        | 빠름                            |
| 메모리 사용       | 많음                        | 적음                            |
| 사용 목적         | 변경이 적은 문자열 처리     | 자주 변경되는 문자열 처리       |
| 스레드 안전성     | ✅ 안전                     | ❌ 비동기 환경에서는 `StringBuffer` 사용 |
| 최종 결과 반환     | 그대로 사용 가능             | `toString()`으로 변환 필요       |



## 💡 실무 팁
- 반복 수정 시 StringBuilder 또는 buildString {} 사용
- 멀티스레드 환경에서는 StringBuffer 또는 동기화된 블록 사용

---


# 🚀 Kotlin의 문자열 최적화
## 1️⃣ 문자열 리터럴 최적화
```kotlin
val helloWorld = "Hello, " + "World!" // 컴파일 시점에 "Hello, World!"로 최적화됨
```

- 리터럴끼리의 연결은 컴파일 시점에 병합됨 → 성능 향상

## 2️⃣ 변수 기반 문자열 결합
```kotlin
val result = str1 + str2
```

- Kotlin도 내부적으로 StringBuilder를 사용해 최적화

## 3️⃣ 최적화가 어려운 경우
```kotlin
var result = ""
for (i in 0 until 100000) {
    result += "Hello Kotlin "
}
```

- 매 반복마다 새로운 객체 생성 → 성능 저하
## ✅ 해결책:
```kotlin
val result = buildString {
    repeat(100000) {
        append("Hello Kotlin ")
    }
}
```


## 🧵 StringBuilder vs StringBuffer
| 항목             | StringBuilder                          | StringBuffer                           |
|------------------|----------------------------------------|-----------------------------------------|
| 동기화 여부       | ❌ 비동기 (멀티스레드 안전하지 않음)     | ✅ 동기화 지원 (멀티스레드 안전)          |
| 성능              | 빠름 (동기화 없음)                     | 느림 (동기화 오버헤드 있음)              |
| 사용 환경         | 단일 스레드 환경                        | 멀티 스레드 환경                         |
| 기능              | append, insert, delete 등 동일         | append, insert, delete 등 동일           |
| 실무 사용 빈도     | 일반적으로 더 많이 사용됨               | 동기화가 필요한 경우에만 사용됨           |


---


# 🔗 메서드 체이닝 (Method Chaining)
## 1️⃣ 개념
- 메서드 체이닝이란 메서드 호출의 결과로 **자기 자신(this)**을 반환하여, 다음 메서드를 이어서 호출할 수 있는 방식
- 마치 메서드들이 체인처럼 연결되어 있는 구조
## 2️⃣ 기본 구조 예제 (Kotlin 버전)
```kotlin
class ValueAdder {
    private var value: Int = 0

    fun add(addValue: Int): ValueAdder {
        value += addValue
        return this // 자기 자신 반환
    }

    fun getValue(): Int {
        return value
    }
}
```

## 3️⃣ 사용 방식 비교
###  ❌ 일반 방식 (체이닝 없음)
```kotlin
val adder = ValueAdder()
adder.add(1)
adder.add(2)
adder.add(3)
val result = adder.getValue() // result = 6
```

### ✅ 체이닝 방식
```kotlin
val adder = ValueAdder()
val result = adder.add(1).add(2).add(3).getValue() // result = 6
```

## 4️⃣ 실행 흐름
```kotlin
adder.add(1).add(2).add(3).getValue()
```

- 각 add() 호출 후 this 반환 → 다음 메서드 호출 가능
## ✅ 메서드 체이닝의 장점
| 항목             | 설명                                      | 예시 또는 효과                                 |
|------------------|-------------------------------------------|------------------------------------------------|
| 코드 간결성       | 변수 없이 연속 호출 가능                  | `obj.method1().method2().method3()`            |
| 가독성 향상       | 흐름이 자연스럽고 직관적으로 읽힘         | 위에서 아래로 이어지는 호출 구조               |
| 유지보수 용이     | 중간 상태 추적 없이 한 줄로 표현 가능      | 수정 시 한 줄만 변경하면 됨                    |
| 객체 설계 유연성  | API 설계 시 유창한 인터페이스 제공 가능    | 빌더 패턴, 설정 객체, DSL 등에서 활용           |
| 실무 활용 빈도     | 다양한 라이브러리와 프레임워크에서 사용됨 | `StringBuilder`, `apply`, `also`, `run` 등     |


---

# 🔗 StringBuilder와 메서드 체이닝
## 1️⃣ 메서드 체이닝이란?
- 동일한 개념: 메서드가 자기 자신을 반환하면 연속 호출 가능
## 2️⃣ StringBuilder의 체이닝 구조 (Kotlin에서도 동일)
```kotlin
un main() {
    val sb = StringBuilder()
    val string = sb.append("A").append("B").append("C").append("D")
        .insert(4, "Kotlin")
        .delete(4, 10)
        .reverse()
        .toString()
    println("string = $string") // DCBA
}
```

- Kotlin의 StringBuilder도 자바처럼 체이닝 지원

---

## 🧪 Kotlin 문자열 문제 & 풀이 요약
### 🔹 문제 1: startsWith()
```kotlin
val url = "https://www.example.com"
val result = url.startsWith("https://")
```


### 🔹 문제 2: length()
```kotlin
val arr = arrayOf("hello", "kotlin", "jvm", "spring", "jpa")
var sum = 0
for (s in arr) {
    println("$s:${s.length}")
    sum += s.length
}
println("sum = $sum")
```


### 🔹 문제 3: indexOf()
```kotlin
val str = "hello.txt"
val index = str.indexOf(".txt")
```


### 🔹 문제 4: substring()\
```kotlin
val str = "hello.txt"
val filename = str.substring(0, 5)
val extName = str.substring(5)
```


### 🔹 문제 5: indexOf() + substring()
```kotlin
val str = "hello.txt"
val ext = ".txt"
val extIndex = str.indexOf(ext)
val filename = str.substring(0, extIndex)
val extName = str.substring(extIndex)
```


### 🔹 문제 6: 반복 검색 indexOf()
```kotlin
val str = "start hello kotlin, hello spring, hello jpa"
val key = "hello"
var count = 0
var index = str.indexOf(key)
while (index >= 0) {
    count++
    index = str.indexOf(key, index + 1)
}
println("count = $count")
```


### 🔹 문제 7: trim()
```kotlin
val original = " Hello Kotlin "
val trimmed = original.trim()



### 🔹 문제 8: replace()\
```kotlin
val input = "hello kotlin spring jpa kotlin"
val result = input.replace("kotlin", "jvm")
```


### 🔹 문제 9: split()
```kotlin
val email = "hello@example.com"
val parts = email.split("@")
val idPart = parts[0]
val domainPart = parts[1]



### 🔹 문제 10: split() + join()
```kotlin
val fruits = "apple,banana,mango"
val splitFruits = fruits.split(",")
splitFruits.forEach { println(it) }
val joinedString = splitFruits.joinToString("->")
```


### 🔹 문제 11: StringBuilder.reverse()
```kotlin
val str = "Hello Kotlin"
val reversed = StringBuilder(str).reverse().toString()
```

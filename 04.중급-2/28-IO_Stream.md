# IO Stream

이 코드는 Kotlin의 입출력 스트림(IO Stream) 구조를 잘 보여주는 예제.  
아래에 Writer/Reader의 상속 관계와 함께, 두 소스 파일을 단계적으로 정리.

## 📚 Kotlin IO 스트림 계층 구조
### [Character 기반]
```
Writer (추상 클래스)
├── FileWriter
├── BufferedWriter
└── PrintWriter
```
```
Reader (추상 클래스)
├── FileReader
├── BufferedReader
└── InputStreamReader
```


### [Byte 기반]
```
OutputStream (추상 클래스)
├── FileOutputStream
├── BufferedOutputStream
```
```
InputStream (추상 클래스)
├── FileInputStream
├── BufferedInputStream
```

- Writer / Reader: 문자(char) 기반 스트림
- OutputStream / InputStream: 바이트(byte) 기반 스트림
- PrintWriter: 문자 기반 출력에 특화된 고급 클래스

## 🧾 InputStreamTest.kt 단계별 설명

### 전체 소스
```kotlin
import java.io.*

fun main() {
    // [1] 파일에 문자열 쓰기
    try {
        FileWriter("Output.txt").use { fw ->
            BufferedWriter(fw).use { bw ->
                PrintWriter(bw).use { out ->
                    out.println("the text")
                    out.println("more text")
                }
            }
        }
    } catch (e: IOException) {
        e.printStackTrace()
    }
```
```kotlin
    // [2] 파일에서 문자열 읽기
    try {
        val inputStream: InputStream = FileInputStream("Output.txt")
        val strRet = inputStreamToString(inputStream)
        println(strRet)
    } catch (e: Exception) {
        e.printStackTrace()
    }
}
```
```kotlin
// [3] InputStream → String 변환
fun inputStreamToString(inputStream: InputStream): String {
    val writer = StringWriter()
    val buffer = CharArray(1024)

    InputStreamReader(inputStream, "UTF-8").use { isr ->
        BufferedReader(isr).use { reader ->
            var n: Int
            while (reader.read(buffer).also { n = it } != -1) {
                writer.write(buffer, 0, n)
            }
        }
    }
    return writer.toString()
}
```


## 🧾 OutputStreamTest.kt 단계별 설명
```java
import java.io.*

fun main() {
    val myFile = File("Output2.txt")
    try {
        PrintWriter(BufferedOutputStream(FileOutputStream(myFile))).use { writer ->
            writer.println("Sample")
        }
    } catch (e: IOException) {
        e.printStackTrace()
    }
}
```


flush / close
- flush() → 버퍼 비우기 (데이터 강제 전송)
- close() → flush + 스트림 닫기 (자원 해제)
- Kotlin에서는 use { ... } 블록을 쓰면 자동으로 close()가 호출됩니다.
- 
### 예시:
```kotlin
BufferedWriter(FileWriter("file.txt")).use { bw ->
    bw.write("Hello")
    bw.flush() // 중간에 강제 기록
} // 블록 끝나면 자동 close()
```


## 🧭 Kotlin IO 클래스 사용 흐름 예시
```kotlin
BufferedReader(
    InputStreamReader(
        BufferedInputStream(
            FileInputStream("file.txt")
        )
    )
).use { reader ->
    val line = reader.readLine()
    println(line)
}
```

- FileInputStream: 파일에서 바이트 읽기
- BufferedInputStream: 버퍼링으로 성능 향상
- InputStreamReader: 바이트 → 문자 변환
- BufferedReader: 줄 단위 읽기

## 🎯 데코레이터 기반 IO의 장단점 (Kotlin 관점)
### ✅ 장점
- 유연한 기능 조합: 필요한 기능만 래핑해서 사용 가능
- 성능 향상: 버퍼링으로 시스템 호출 감소
- 자동 자원 관리: use {} 블록으로 close 자동 처리
### ❌ 단점
- 중첩 구조 복잡: 여러 래퍼를 겹쳐야 함
- 기억하기 어려움: 어떤 클래스가 어떤 기능 제공하는지 헷갈림
- 코드 길어짐: 중첩 생성자 호출


## 📌 Kotlin IO 데코레이터 클래스 요약

| 클래스명              | 설명                                      | 주요 용도                  |
|-----------------------|-------------------------------------------|----------------------------|
| BufferedReader        | 문자 입력을 버퍼링하여 효율적으로 읽음     | 대량 텍스트 읽기 성능 향상 |
| BufferedWriter        | 문자 출력을 버퍼링하여 효율적으로 씀       | 대량 텍스트 쓰기 성능 향상 |
| PrintWriter           | 포맷팅된 텍스트 출력 지원                  | println, printf 스타일 출력|
| InputStreamReader     | 바이트 스트림을 문자 스트림으로 변환       | 인코딩 지정 가능           |
| OutputStreamWriter    | 문자 스트림을 바이트 스트림으로 변환       | 인코딩 지정 가능           |
| DataInputStream       | 기본형 데이터(int, double 등) 읽기 지원    | 바이너리 데이터 처리       |
| DataOutputStream      | 기본형 데이터(int, double 등) 쓰기 지원    | 바이너리 데이터 처리       |
| ObjectInputStream     | 객체 역직렬화 지원                        | 객체 읽기                  |
| ObjectOutputStream    | 객체 직렬화 지원                          | 객체 쓰기                  |
| PushbackReader        | 읽은 문자를 다시 되돌릴 수 있음            | 파서 구현 시 유용           |



## 🧪 Kotlin 샘플 코드
### 1️⃣ BufferedReader
```kotlin
BufferedReader(InputStreamReader(FileInputStream("input.txt"))).use { reader ->
    val line = reader.readLine()
    println(line)
}
```

### 2️⃣ BufferedWriter
```kotlin
BufferedWriter(OutputStreamWriter(FileOutputStream("output.txt"))).use { writer ->
    writer.write("Hello, JungHwan!")
    writer.newLine()
}
```

### 3️⃣ BufferedInputStream
```kotlin
BufferedInputStream(FileInputStream("image.jpg")).use { bis ->
    val data = bis.read()
    println(data)
}
```

### 4️⃣ BufferedOutputStream
```kotlin
BufferedOutputStream(FileOutputStream("copy.jpg")).use { bos ->
    bos.write(255)
    bos.flush()
}
```

### 5️⃣ PrintWriter
```kotlin
PrintWriter("log.txt").use { pw ->
    pw.println("로그 기록 시작")
}
```


## ✅ 요약:
- Kotlin은 Java IO 클래스를 그대로 사용하지만 use {} 블록으로 자원 관리가 더 간단합니다.
- 구조와 계층은 Java와 동일하며, 데코레이터 패턴으로 기능을 조합합니다.

---


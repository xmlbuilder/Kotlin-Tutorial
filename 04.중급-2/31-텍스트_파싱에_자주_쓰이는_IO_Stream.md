# 텍스트 파싱에 자주 쓰이는 IO Stream

아래는 Kotlin에서 텍스트 파싱 시 자주 사용되는 I/O 스트림 및 확장 함수들을 정리한 표입니다.  
Kotlin의 확장 함수를 활용하면 더욱 간결한 코드로 I/O 작업을 수행할 수 있습니다.


## 🧾 Kotlin 샘플 코드
```kotlin
import java.io.*

fun main() {
    val inputFileName = "Output.txt"
    val outputFileName = "Sample.txt"

    // BufferedReader로 줄 단위 읽기
    try {
        BufferedReader(FileReader(inputFileName)).use { br ->
            var line: String?
            while (br.readLine().also { line = it } != null) {
                println(line)
            }
        }
    } catch (e: IOException) {
        e.printStackTrace()
    }
```
```kotlin
    // BufferedWriter로 줄 단위 쓰기
    try {
        BufferedWriter(FileWriter(outputFileName)).use { wr ->
            wr.write("Sample1")
            wr.newLine()
            wr.write("Sample2")
            wr.newLine()
            wr.write("Sample3")
            wr.newLine()
            wr.write("Sample4")
            wr.newLine()
        }
    } catch (e: IOException) {
        e.printStackTrace()
    }
}
```

## 📦 Kotlin 텍스트 파싱용 I/O 스트림 요약
| 클래스               | 역할 및 특징                              |
|----------------------|--------------------------------------------|
| FileReader           | 텍스트 파일을 문자 단위로 읽음             |
| BufferedReader       | Reader 버퍼링, readLine() 지원             |
| InputStreamReader    | 바이트 → 문자 스트림 변환 (인코딩 지정 가능) |
| FileInputStream      | 파일을 바이트 단위로 읽음                  |
| FileWriter           | 텍스트 파일에 문자 단위로 씀               |
| BufferedWriter       | Writer 버퍼링, newLine() 지원              |
| OutputStreamWriter   | 문자 → 바이트 스트림 변환 (인코딩 지정 가능) |
| FileOutputStream     | 파일에 바이트 단위로 씀                    |

## 🧠 인코딩을 명시하고 싶을 때
```kotlin
val br = BufferedReader(
    InputStreamReader(FileInputStream("Output.txt"), Charsets.UTF_8)
)
```
```kotlin
val bw = BufferedWriter(
    OutputStreamWriter(FileOutputStream("Sample.txt"), Charsets.UTF_8)
)
```

- Charsets.UTF_8을 사용하면 명시적으로 UTF-8 인코딩 지정 가능
- 기본 FileReader/FileWriter는 시스템 인코딩 사용 → 다국어 처리 시 주의

## 📌 요약
- BufferedReader + FileReader: 줄 단위 읽기
- BufferedWriter + FileWriter: 줄 단위 쓰기
- InputStreamReader / OutputStreamWriter: 인코딩 지정 시 필수
- FileInputStream / FileOutputStream: 바이트 단위 처리

---




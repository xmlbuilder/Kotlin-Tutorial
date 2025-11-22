# DataOutputStream / DataInputStream
DataOutputStream과 DataInputStream은 자바에서 기본형 데이터 타입을 바이너리 형식으로  
읽고 쓰기 위한 스트림 클래스입니다.  
Kotlin은 JVM 위에서 동작하기 때문에 동일한 IO 클래스(DataOutputStream, DataInputStream,  
ObjectOutputStream, ObjectInputStream)를 그대로 사용할 수 있습니다.  
다만 try-with-resources 대신 use {} 블록을 활용하는 것이 Kotlin의 관용적 방식입니다.  

## 📦 Kotlin DataStream 클래스 요약
### DataOutputStream / DataInputStream 요약
| 클래스명           | 주요 메서드                          | 설명 |
|--------------------|--------------------------------------|------|
| DataOutputStream   | writeBoolean(), writeInt(), writeUTF() | 기본형 데이터를 바이너리로 출력 |
| DataInputStream    | readBoolean(), readInt(), readUTF()   | 바이너리로 저장된 기본형 데이터를 읽음 |


## 전체 코드 (Kotlin 버전)
```kotlin
import java.io.*

fun main() {
    // 쓰기
    DataOutputStream(FileOutputStream("data.bin")).use { dos ->
        dos.writeBoolean(true)
        dos.write(5)
        dos.writeByte(5)
        dos.writeInt(100)
        dos.writeDouble(200.5)
        dos.writeUTF("Hi., Hyangseon")
    }
```
```kotlin
    // 읽기
    DataInputStream(FileInputStream("data.bin")).use { dis ->
        val b = dis.readBoolean()
        val b2 = dis.read()
        val b3 = dis.readByte()
        val i = dis.readInt()
        val d = dis.readDouble()
        val s = dis.readUTF()

        println(b)
        println(b2)
        println(b3)
        println(i)
        println(d)
        println(s)
    }
}
```
```kotlin

## 🧪 주요 메서드 설명 및 예시 (Kotlin)
###  ✅ writeBoolean(boolean v) / readBoolean()
```kotlin
dos.writeBoolean(true)        // true 저장
val b = dis.readBoolean()     // true 읽기
```

### ✅ write(int v) / read()
```kotlin
dos.write(5)                  // 1바이트 저장
val b2 = dis.read()           // 1바이트 읽기 (0~255)

### ✅ writeByte(byte v) / readByte()
```kotlin
dos.writeByte(5)
val b3 = dis.readByte()
```
### ✅ writeInt(int v) / readInt()
```kotlin
dos.writeInt(100)
val i = dis.readInt()
```
### ✅ writeDouble(double v) / readDouble()
```kotlin
dos.writeDouble(200.5)
val d = dis.readDouble()
```
### ✅ writeUTF(String s) / readUTF()
```kotlin
dos.writeUTF("Hi., Hyangseon")
val s = dis.readUTF()
```


## 📌 장점 요약
- 기본형 타입을 정확하게 저장/복원 가능
- 플랫폼 독립적인 바이너리 포맷
- 네트워크 전송, 파일 저장에 적합
### 🧠 주의할 점
- 쓰기 순서와 읽기 순서가 반드시 일치해야 함
- write()와 writeByte()는 다르게 동작할 수 있음 → write()는 int지만 1바이트만 저장

---

# 객체 직렬화 & 네트워크 통신 (Kotlin)
## 🧱 1. 객체 직렬화 예제 (DataOutputStream 기반)
```kotlin
import java.io.*

data class Person(val name: String, val age: Int)
```
```kotlin
fun main() {
    // 저장
    DataOutputStream(FileOutputStream("person.bin")).use { dos ->
        val p = Person("JungHwan", 30)
        dos.writeUTF(p.name)
        dos.writeInt(p.age)
    }
```
```kotlin
    // 읽기
    DataInputStream(FileInputStream("person.bin")).use { dis ->
        val name = dis.readUTF()
        val age = dis.readInt()
        val p = Person(name, age)
        println("${p.name}, ${p.age}")
    }
}
```
---

## 🌐 2. 네트워크 통신 예제 (Socket + DataStream)
클라이언트와 서버가 DataInputStream / DataOutputStream을 통해 기본형 데이터를 주고받는 구조입니다.

## ✅ 서버 코드
```kotlin
import java.io.*
import java.net.*

fun main() {
    val server = ServerSocket(9999)
    val socket = server.accept()

    val dis = DataInputStream(socket.getInputStream())
    val msg = dis.readUTF()
    println("클라이언트로부터 받은 메시지: $msg")

    val dos = DataOutputStream(socket.getOutputStream())
    dos.writeUTF("서버 응답: 안녕 JungHwan!")

    dis.close()
    dos.close()
    socket.close()
    server.close()
}
```

## ✅ 클라이언트 코드
```kotlin
import java.io.*
import java.net.*

fun main() {
    val socket = Socket("localhost", 9999)

    val dos = DataOutputStream(socket.getOutputStream())
    dos.writeUTF("클라이언트 메시지: 안녕하세요!")

    val dis = DataInputStream(socket.getInputStream())
    val response = dis.readUTF()
    println("서버 응답: $response")

    dos.close()
    dis.close()
    socket.close()
}
```


## 📦 객체 직렬화 & 역직렬화 흐름 요약
| 단계 | 클래스              | 설명 |
|------|---------------------|------|
| 1. 객체 저장 | ObjectOutputStream | 객체를 바이너리 형태로 직렬화하여 저장 |
| 2. 버퍼링   | BufferedOutputStream | 성능 향상을 위해 버퍼링 처리 |
| 3. 파일 출력 | FileOutputStream | 파일에 바이트 단위로 저장 |
| 4. 객체 읽기 | ObjectInputStream | 저장된 객체를 역직렬화하여 복원 |
| 5. 버퍼링   | BufferedInputStream | 성능 향상을 위해 버퍼링 처리 |
| 6. 파일 입력 | FileInputStream | 파일에서 바이트 단위로 읽기 |



## 🧪 전체 예제 코드 (Kotlin 직렬화)
### ✅ 직렬화 (저장)
```kotlin
import java.io.*

data class Person(val name: String, val age: Int) : Serializable

fun main() {
    ObjectOutputStream(
        BufferedOutputStream(FileOutputStream("person.obj"))
    ).use { oos ->
        val p = Person("JungHwan", 30)
        oos.writeObject(p)
    }
}
```

### ✅ 역직렬화 (읽기)
```kotlin
import java.io.*

fun main() {
    ObjectInputStream(
        BufferedInputStream(FileInputStream("person.obj"))
    ).use { ois ->
        val p = ois.readObject() as Person
        println("${p.name}, ${p.age}")
    }
}
```


## 📌 Kotlin 설명 요약
- Kotlin에서도 Java IO 클래스(DataOutputStream, DataInputStream, ObjectOutputStream, ObjectInputStream)를 그대로 사용 가능
- try-with-resources 대신 use {} 블록을 활용하는 것이 관용적
- data class와 Serializable을 함께 쓰면 객체 직렬화/역직렬화가 간단해짐
- 네트워크 통신, 파일 저장 모두 동일한 방식으로 적용 가능

---


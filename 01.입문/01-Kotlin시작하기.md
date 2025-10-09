# Kotlin 시작하기
## 🧑‍💻 1. 개발 환경 설정 (Kotlin 기준)
### IDE 선택: IntelliJ vs Eclipse
- IntelliJ: Kotlin 공식 지원. 대부분의 기업에서 사용.
- Eclipse: Kotlin 플러그인 설치 필요. 비추천.
- 추천: IntelliJ Community Edition (무료)
### OS 선택
- Mac / Windows / Linux 모두 사용 가능
### Kotlin 설치
- IntelliJ에서 Kotlin 플러그인 자동 설치됨
- 별도 JDK 설치 필요 (JDK 17 이상 권장)

## 💻 2. IntelliJ 설치 및 Kotlin 프로젝트 생성
### 설치 링크
- IntelliJ 다운로드
### 새 프로젝트 생성
- 이름: kotlin-start
- 언어: Kotlin (JVM)
- JDK: 17 이상 (권장: OpenJDK 21)
- 샘플 코드 추가: 체크
### JDK 설정
- Vendor: Oracle OpenJDK 또는 Eclipse Temurin
- Mac M1/M2/M3: aarch64 선택
- 설치 위치: 기본값 사용

## 📂 3. 기존 Kotlin 프로젝트 열기
### 압축 푼 프로젝트 열기
- 메뉴: File → New → Project from Existing Sources
- JDK 없을 경우: + 버튼으로 OpenJDK 21 다운로드

## ▶️ 4. Kotlin 프로그램 실행
### HelloKotlin 예제
```kotlin
fun main() {
    println("hello kotlin")
}
```

- main() 함수가 시작점
- println()으로 콘솔 출력
- 세미콜론 ; 불필요
### 실행 결과
```
hello kotlin
```


## ✍️ 5. 주석 (Comment)
주석 종류
- 한 줄: //
- 여러 줄: /* ... */
### 예제
```kotlin
fun main() {
    println("hello kotlin1") // 출력 설명
    // println("hello kotlin2") // 실행 안됨
    /*
    println("hello kotlin3")
    println("hello kotlin4")
    */
}
```

## 📘 6. Kotlin이란?
### Kotlin의 구조
- JVM 기반 언어
- 자바와 100% 호환
- 주요 구현체: Kotlin/JVM, Kotlin/Native, Kotlin/JS
### 운영체제 독립성
- Kotlin도 JVM 위에서 실행됨
- OS에 상관없이 작동 (Mac에서 개발, Linux에서 운영 가능)
### 컴파일과 실행
- .kt 파일 작성
- IntelliJ에서 자동 컴파일 및 실행
- 명령어: kotlinc, kotlin (CLI 환경)
### IntelliJ의 역할
- Kotlin 플러그인 내장
- 컴파일과 실행을 버튼 클릭으로 처리

---


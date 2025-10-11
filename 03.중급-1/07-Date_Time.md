
# 📚 Kotlin 기준 주요 클래스별 메서드 정리

---
# 🕓 LocalDateTime
## ✅ 생성
| 메서드 이름 | 설명                           | 예시 코드                                      |
|-------------|--------------------------------|------------------------------------------------|
| now()       | 현재 시스템의 날짜와 시간 생성 | `val now = LocalDateTime.now()`                |
| of(...)     | 지정한 날짜와 시간으로 생성     | `val dt = LocalDateTime.of(2024, 1, 1, 9, 0)`  |

### 🔍 추가 설명
- now() → 현재 시각을 기준으로 LocalDateTime 객체 생성
- of(...) → 연도, 월, 일, 시, 분, 초, 나노초까지 직접 지정 가능

### 샘플코드
```kotlin
import java.time.LocalDateTime

fun main() {
    // ✅ now() – 현재 시스템의 날짜와 시간 생성
    val now = LocalDateTime.now()
    println("현재 시각: $now")

    // ✅ of(...) – 지정한 날짜와 시간으로 생성
    val dt = LocalDateTime.of(2024, 1, 1, 9, 0)
    println("지정한 시각: $dt")
}
```
### 출력 결과
```
현재 시각: 2025-10-11T13:24:52.123
지정한 시각: 2024-01-01T09:00

```

## ✅ 변환
| 메서드 이름             | 반환 타입   | 예시 코드                                              |
|-------------------------|-------------|---------------------------------------------------------|
| toLocalDate()           | LocalDate   | `dt.toLocalDate()`                                     |
| toLocalTime()           | LocalTime   | `dt.toLocalTime()`                                     |
| toEpochSecond(offset)   | Long        | `dt.toEpochSecond(ZoneOffset.of("+09:00"))`            |


## 샘플코드
```kotlin
import java.time.LocalDateTime
import java.time.ZoneOffset

fun main() {
    // 기준이 되는 날짜와 시간 설정
    val dt = LocalDateTime.of(2024, 1, 1, 9, 0, 0)

    // ✅ toLocalDate() – 날짜만 추출
    val dateOnly = dt.toLocalDate()
    println("LocalDate: $dateOnly") // 2024-01-01

    // ✅ toLocalTime() – 시간만 추출
    val timeOnly = dt.toLocalTime()
    println("LocalTime: $timeOnly") // 09:00

    // ✅ toEpochSecond(offset) – UTC 기준 초 단위 타임스탬프
    val epochSeconds = dt.toEpochSecond(ZoneOffset.of("+09:00"))
    println("Epoch Second (+09:00 기준): $epochSeconds")
}
```
### 출력 결과
```
LocalDate: 2024-01-01
LocalTime: 09:00
Epoch Second (+09:00 기준): 1704067200

```


## ✅ 조회
| 조회 항목             | 설명               | 예시 코드                                 |
|-----------------------|--------------------|--------------------------------------------|
| year, month, dayOfMonth | 날짜 구성 요소 조회 | `dt.year`, `dt.month`, `dt.dayOfMonth`     |
| hour, minute, second    | 시간 구성 요소 조회 | `dt.hour`, `dt.minute`, `dt.second`        |

### 샘플 코드
```kotlin
import java.time.LocalDateTime
fun main() {
    // 현재 날짜와 시간 또는 특정 날짜/시간 설정
    val dt = LocalDateTime.of(2024, 10, 11, 13, 23, 45)

    // 날짜 구성 요소 조회
    val year = dt.year
    val month = dt.monthValue
    val day = dt.dayOfMonth

    // 시간 구성 요소 조회
    val hour = dt.hour
    val minute = dt.minute
    val second = dt.second

    // 출력
    println("📅 날짜 정보")
    println("연도: $year")
    println("월: $month")
    println("일: $day")

    println("\n⏰ 시간 정보")
    println("시: $hour")
    println("분: $minute")
    println("초: $second")
}
```
### 출력 결과
```
📅 날짜 정보
연도: 2024
월: 10
일: 11

⏰ 시간 정보
시: 13
분: 23
초: 45
```

## ✅ 비교

| 메서드 이름                     | 설명                           | 예시 코드                                      |
|--------------------------------|--------------------------------|------------------------------------------------|
| isBefore(), isAfter(), isEqual() | 시간 비교 (이전/이후/동일 여부) | `dt1.isBefore(dt2)`, `dt1.isAfter(dt2)`, `dt1.isEqual(dt2)` |

### 샘플 코드
```kotlin
val dt1 = LocalDateTime.of(2024, 1, 1, 9, 0)
val dt2 = LocalDateTime.of(2024, 1, 1, 10, 0)

println(dt1.isBefore(dt2)) // true
println(dt1.isAfter(dt2))  // false
println(dt1.isEqual(dt2))  // false
```

## ✅ 수정

| 메서드 이름               | 설명                                  | 예시 코드                                              |
|---------------------------|---------------------------------------|---------------------------------------------------------|
| with(...)                 | 지정된 필드 또는 조정기로 값 변경     | `dt.with(ChronoField.DAY_OF_MONTH, 15)`                |
| withYear(), withMonth()   | 연도 또는 월을 직접 지정하여 변경     | `dt.withYear(2025)`, `dt.withMonth(12)`                |


### 샘플코드
```kotlin
import java.time.LocalDateTime
import java.time.temporal.ChronoField

fun main() {
    // 기준 날짜/시간 설정
    val dt = LocalDateTime.of(2024, 1, 1, 9, 0)

    // ✅ with(...) – 특정 필드 값 변경 (예: 일자를 15일로 변경)
    val changedDay = dt.with(ChronoField.DAY_OF_MONTH, 15)
    println("일자 변경: $changedDay") // 2024-01-15T09:00

    // ✅ withYear() – 연도 변경
    val changedYear = dt.withYear(2025)
    println("연도 변경: $changedYear") // 2025-01-01T09:00

    // ✅ withMonth() – 월 변경
    val changedMonth = dt.withMonth(12)
    println("월 변경: $changedMonth") // 2024-12-01T09:00
}
```
### 출력 결과
```
일자 변경: 2024-01-15T09:00
연도 변경: 2025-01-01T09:00
월 변경: 2024-12-01T09:00
```

## ✅ 추가/감소

| 메서드 이름                   | 설명                                  | 예시 코드                                      |
|-------------------------------|---------------------------------------|------------------------------------------------|
| plus(...), minus(...)         | 지정된 시간 단위로 더하거나 빼기      | `dt.plus(3, ChronoUnit.DAYS)`                  |
| plusYears(), plusDays()       | 연도 또는 일수를 더하기               | `dt.plusYears(1)`, `dt.plusDays(10)`           |


### 샘플코드
```kotlin
import java.time.LocalDateTime
import java.time.temporal.ChronoUnit

fun main() {
    // 기준 날짜/시간 설정
    val dt = LocalDateTime.of(2024, 1, 1, 9, 0)

    // ✅ plus(...) – 지정된 시간 단위로 더하기 (예: 3일 추가)
    val plusDays = dt.plus(3, ChronoUnit.DAYS)
    println("3일 추가: $plusDays") // 2024-01-04T09:00

    // ✅ minus(...) – 지정된 시간 단위로 빼기 (예: 2시간 빼기)
    val minusHours = dt.minus(2, ChronoUnit.HOURS)
    println("2시간 감소: $minusHours") // 2024-01-01T07:00

    // ✅ plusYears() – 연도 추가
    val nextYear = dt.plusYears(1)
    println("1년 추가: $nextYear") // 2025-01-01T09:00

    // ✅ plusDays() – 일수 추가
    val tenDaysLater = dt.plusDays(10)
    println("10일 추가: $tenDaysLater") // 2024-01-11T09:00
}
```
### 출력 결과
```
3일 추가: 2024-01-04T09:00
2시간 감소: 2024-01-01T07:00
1년 추가: 2025-01-01T09:00
10일 추가: 2024-01-11T09:00
```

## ✅ 포맷팅

| 메서드 이름               | 설명                                  | 예시 코드                                                   |
|---------------------------|---------------------------------------|--------------------------------------------------------------|
| format(DateTimeFormatter) | 지정한 포맷 패턴으로 문자열 변환      | `dt.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"))` |


### 샘플코드
```kotlin
import java.time.LocalDateTime
import java.time.format.DateTimeFormatter

fun main() {
    // 기준 날짜/시간 설정
    val dt = LocalDateTime.of(2024, 1, 1, 9, 0, 30)

    // ✅ format(DateTimeFormatter) – 지정한 포맷으로 문자열 변환
    val formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")
    val formatted = dt.format(formatter)

    println("포맷팅된 문자열: $formatted")
}
```
### 출력 결과
```
포맷팅된 문자열: 2024-01-01 09:00:30
```

---

# 🌍 ZonedDateTime

## ✅ 생성

| 메서드 이름       | 설명                                  | 예시 코드                                                                 |
|-------------------|---------------------------------------|----------------------------------------------------------------------------|
| now(), now(zone)  | 현재 또는 지정된 시간대 기준 현재 시각 | `ZonedDateTime.now(ZoneId.of("Asia/Seoul"))`                              |
| of(...)           | 날짜, 시간, 시간대를 지정하여 생성     | `ZonedDateTime.of(LocalDate.of(2024,1,1), LocalTime.of(9,0), ZoneId.of("Asia/Seoul"))` |

### 샘플코드
```kotlin
import java.time.LocalDate
import java.time.LocalTime
import java.time.ZoneId
import java.time.ZonedDateTime

fun main() {
    // ✅ 현재 시각 (Asia/Seoul 기준)
    val nowSeoul: ZonedDateTime = ZonedDateTime.now(ZoneId.of("Asia/Seoul"))
    println("현재 서울 시각: $nowSeoul")

    // ✅ 지정된 날짜/시간/시간대 기반 ZonedDateTime 생성
    val date = LocalDate.of(2024, 1, 1)
    val time = LocalTime.of(9, 0)
    val zone = ZoneId.of("Asia/Seoul")
    val zonedDateTime = ZonedDateTime.of(date, time, zone)
    println("지정된 서울 시각: $zonedDateTime")
}
```
### 출력 결과
```
현재 서울 시각: 2025-10-11T16:10:00+09:00[Asia/Seoul]
지정된 서울 시각: 2024-01-01T09:00+09:00[Asia/Seoul]

```

## ✅ 타임존 관리

| 메서드 이름             | 설명                                                   | 예시 코드                                                   |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------------|
| withZoneSameInstant()   | UTC 기준 동일한 순간을 다른 시간대로 변환              | `zdt.withZoneSameInstant(ZoneId.of("Europe/London"))`        |
| withZoneSameLocal()     | 로컬 시각 유지하며 시간대만 변경 (실제 시각은 달라짐) | `zdt.withZoneSameLocal(ZoneId.of("America/New_York"))`       |


### 샘플코드
```kotlin
import java.time.LocalDate
import java.time.LocalTime
import java.time.ZoneId
import java.time.ZonedDateTime

fun main() {
    // ✅ now() – 시스템 기본 시간대 기준 현재 시각
    val nowDefault = ZonedDateTime.now()
    println("현재 시각 (기본 시간대): $nowDefault")

    // ✅ now(zone) – 지정된 시간대 기준 현재 시각
    val nowSeoul = ZonedDateTime.now(ZoneId.of("Asia/Seoul"))
    println("현재 시각 (서울): $nowSeoul")

    // ✅ of(...) – 날짜, 시간, 시간대를 지정하여 생성
    val meetingTime = ZonedDateTime.of(
        LocalDate.of(2024, 1, 1),
        LocalTime.of(9, 0),
        ZoneId.of("Asia/Seoul")
    )
    println("지정된 회의 시각 (서울): $meetingTime")
}
```
### 출력 결과
```
현재 시각 (기본 시간대): 2025-10-11T13:29:45.123+09:00[Asia/Seoul]
현재 시각 (서울): 2025-10-11T13:29:45.123+09:00[Asia/Seoul]
지정된 회의 시각 (서울): 2024-01-01T09:00+09:00[Asia/Seoul]
```


## ✅ 조회/변환

| 메서드 이름                          | 설명                                  | 예시 코드                                      |
|--------------------------------------|---------------------------------------|------------------------------------------------|
| getOffset(), getZone()               | 오프셋 및 시간대 정보 조회            | `zdt.offset`, `zdt.zone`                       |
| toLocalDateTime(), toInstant()       | 로컬 시간 또는 Instant 객체로 변환    | `zdt.toLocalDateTime()`, `zdt.toInstant()`     |
| toEpochSecond()                      | UTC 기준으로 초 단위 시간 반환        | `zdt.toEpochSecond()`                          |

### 샘플코드
```kotlin
import java.time.LocalDate
import java.time.LocalTime
import java.time.ZoneId
import java.time.ZonedDateTime

fun main() {
    // 기준 ZonedDateTime 생성
    val zdt = ZonedDateTime.of(
        LocalDate.of(2024, 1, 1),
        LocalTime.of(9, 0),
        ZoneId.of("Asia/Seoul")
    )

    // ✅ getOffset(), getZone() – 오프셋 및 시간대 정보 조회
    val offset = zdt.offset
    val zone = zdt.zone
    println("오프셋: $offset") // +09:00
    println("시간대: $zone")   // Asia/Seoul

    // ✅ toLocalDateTime() – ZonedDateTime → LocalDateTime
    val localDateTime = zdt.toLocalDateTime()
    println("LocalDateTime: $localDateTime") // 2024-01-01T09:00

    // ✅ toInstant() – ZonedDateTime → Instant (UTC 기준)
    val instant = zdt.toInstant()
    println("Instant: $instant") // 2024-01-01T00:00:00Z

    // ✅ toEpochSecond() – UTC 기준 초 단위 시간 반환
    val epochSecond = zdt.toEpochSecond()
    println("Epoch Second: $epochSecond") // 1704067200
}
```
### 출력 결과
```
오프셋: +09:00
시간대: Asia/Seoul
LocalDateTime: 2024-01-01T09:00
Instant: 2024-01-01T00:00:00Z
Epoch Second: 1704067200
```

---


# ⏱️ Instant

## ✅ 생성

| 메서드 이름                      | 설명                                  | 예시 코드                                      |
|----------------------------------|---------------------------------------|------------------------------------------------|
| now()                            | 현재 UTC 기준의 시각 생성             | `val now = Instant.now()`                      |
| ofEpochSecond(), ofEpochMilli()  | 에포크 기준으로 초 또는 밀리초 생성   | `Instant.ofEpochSecond(1760156340)`            |

### 샘플코드
```kotlin
import java.time.Instant

fun main() {
    // ✅ now() – 현재 UTC 기준 시각 생성
    val now = Instant.now()
    println("현재 UTC 시각: $now")

    // ✅ ofEpochSecond() – 에포크 기준 초 단위로 Instant 생성
    val fromEpochSecond = Instant.ofEpochSecond(1760156340)
    println("에포크 초 기준 Instant: $fromEpochSecond")

    // ✅ ofEpochMilli() – 에포크 기준 밀리초 단위로 Instant 생성
    val fromEpochMilli = Instant.ofEpochMilli(1760156340000)
    println("에포크 밀리초 기준 Instant: $fromEpochMilli")
}
```
### 출력 결과
```
현재 UTC 시각: 2025-10-11T04:35:12.123Z
에포크 초 기준 Instant: 2025-12-11T00:59:00Z
에포크 밀리초 기준 Instant: 2025-12-11T00:59:00Z
```

## ✅ 변환

| 메서드 이름       | 반환 타입       | 예시 코드                                              |
|-------------------|------------------|---------------------------------------------------------|
| atOffset(offset)  | OffsetDateTime   | `instant.atOffset(ZoneOffset.of("+09:00"))`            |
| atZone(zone)      | ZonedDateTime    | `instant.atZone(ZoneId.of("Asia/Seoul"))`              |

### 샘플코드
```kotlin
import java.time.Instant
import java.time.ZoneId
import java.time.ZoneOffset

fun main() {
    // ✅ Instant 객체 생성
    val instant = Instant.parse("2024-01-01T00:00:00Z")

    // ✅ atOffset(offset) – OffsetDateTime으로 변환
    val offsetDateTime = instant.atOffset(ZoneOffset.of("+09:00"))
    println("OffsetDateTime (+09:00): $offsetDateTime") // 2024-01-01T09:00+09:00

    // ✅ atZone(zone) – ZonedDateTime으로 변환
    val zonedDateTime = instant.atZone(ZoneId.of("Asia/Seoul"))
    println("ZonedDateTime (Asia/Seoul): $zonedDateTime") // 2024-01-01T09:00+09:00[Asia/Seoul]
}
```
### 출력 결과
```
OffsetDateTime (+09:00): 2024-01-01T09:00+09:00
ZonedDateTime (Asia/Seoul): 2024-01-01T09:00+09:00[Asia/Seoul]
```

## ✅ 조회

| 메서드 이름     | 설명                           | 예시 코드               |
|------------------|--------------------------------|--------------------------|
| epochSecond      | UTC 기준 초 단위 시간 반환     | `instant.epochSecond`   |
| nano             | 현재 초의 나노초 부분 반환     | `instant.nano`          |

### 샘플코드
```kotlin
import java.time.Instant

fun main() {
    // ✅ Instant 객체 생성
    val instant = Instant.parse("2024-01-01T00:00:00.123456789Z")

    // ✅ epochSecond – UTC 기준 초 단위 시간 반환
    val seconds = instant.epochSecond
    println("Epoch Second: $seconds") // 1704067200

    // ✅ nano – 현재 초의 나노초 부분 반환
    val nanos = instant.nano
    println("Nano of Second: $nanos") // 123456789
}
```
### 출력 결과
```
Epoch Second: 1704067200
Nano of Second: 123456789
```
---

# 🧠 Kotlin 날짜와 시간 문제 풀이 요약

## ✅ 문제1 – 날짜 더하기

| 기준 시각           | 더한 기간             | 결과 시각           |
|--------------------|-----------------------|---------------------|
| 2024-01-01T00:00   | 1년 2개월 3일 4시간   | 2025-03-04T04:00    |

### 🔍 사용된 코드 핵심
```rust
val dateTime = LocalDateTime.of(2024, 1, 1, 0, 0)
val futureDateTime = dateTime
    .plusYears(1)
    .plusMonths(2)
    .plusDays(3)
    .plusHours(4)
```

- plusYears(), plusMonths(), plusDays(), plusHours() → 시간 단위별로 더함
- LocalDateTime은 불변 객체이므로 반드시 반환값을 받아야 함

## ✅ 문제2 – 날짜 간격 반복 출력

| 시작 날짜   | 간격   | 반복 횟수 | 출력 날짜 예시                  |
|-------------|--------|-----------|----------------------------------|
| 2024-01-01  | 2주    | 5회       | 1/1, 1/15, 1/29, 2/12, 2/26     |

### 🔍 사용된 코드 핵심
```rust
val startDate = LocalDate.of(2024, 1, 1)
repeat(5) { i ->
    val nextDate = startDate.plusWeeks((2L * i))
    println("날짜 ${i + 1}: $nextDate")
}
```

- plusWeeks()로 주 단위 간격 계산
- repeat()로 반복 처리
- LocalDate는 날짜만 다룸

## ✅ 문제3 – 디데이 계산

| 시작 날짜   | 목표 날짜   | 남은 기간         | 디데이         |
|-------------|--------------|--------------------|----------------|
| 2024-01-01  | 2024-11-21   | 0년 10개월 20일    | 325일 남음     |

### 🔍 사용된 코드 핵심
```rust
val startDate = LocalDate.of(2024, 1, 1)
val endDate = LocalDate.of(2024, 11, 21)

val period = Period.between(startDate, endDate)
val daysBetween = ChronoUnit.DAYS.between(startDate, endDate)

println("남은 기간: ${period.years}년 ${period.months}개월 ${period.days}일")
println("디데이: ${daysBetween}일 남음")
```

- Period.between() → 년/월/일 단위 차이 계산
- ChronoUnit.DAYS.between() → 전체 일수 계산

## ✅ 문제4 – 월의 시작/마지막 요일

| 입력 연도 | 입력 월 | 시작 요일 | 마지막 요일 |
|-----------|----------|------------|--------------|
| 2024      | 1        | MONDAY     | WEDNESDAY    |


### 🔍 사용된 코드 핵심
```rust
val date = LocalDate.of(year, month, 1)
val firstDayOfWeek = date.dayOfWeek
val lastDayOfWeek = date.with(TemporalAdjusters.lastDayOfMonth()).dayOfWeek
```

- dayOfWeek → 요일 반환
- TemporalAdjusters.lastDayOfMonth() → 마지막 날짜 계산

## ✅ 문제5 – 국제 회의 시간 변환

| 도시   | 시간대 ID           | 변환된 회의 시간                         |
|--------|---------------------|------------------------------------------|
| 서울   | Asia/Seoul          | 2024-01-01T09:00+09:00[Asia/Seoul]       |
| 런던   | Europe/London       | 2024-01-01T00:00Z[Europe/London]         |
| 뉴욕   | America/New_York    | 2023-12-31T19:00-05:00[America/New_York] |

### 🔍 사용된 코드 핵심
```rust
val seoulTime = ZonedDateTime.of(
    LocalDate.of(2024, 1, 1),
    LocalTime.of(9, 0),
    ZoneId.of("Asia/Seoul")
)

val londonTime = seoulTime.withZoneSameInstant(ZoneId.of("Europe/London"))
val nyTime = seoulTime.withZoneSameInstant(ZoneId.of("America/New_York"))
```

- ZonedDateTime.of() → 특정 시간대의 시간 생성
- withZoneSameInstant() → UTC 기준으로 시간대 변환

## ✅ 문제6 – 달력 출력
| 입력 연도 | 입력 월 | 출력 형식               |
|-----------|----------|-------------------------|
| 2024      | 1        | 월간 달력 (요일별 날짜 나열) |
| 2025      | 1        | 월간 달력 (요일별 날짜 나열) |


### 🔍 사용된 코드 핵심
```rust
val firstDayOfMonth = LocalDate.of(year, month, 1)
val firstDayOfNextMonth = firstDayOfMonth.plusMonths(1)
val offsetWeekDays = firstDayOfMonth.dayOfWeek.value % 7

println("Su Mo Tu We Th Fr Sa")
repeat(offsetWeekDays) {
    print("   ")
}

var dayIterator = firstDayOfMonth
while (dayIterator.isBefore(firstDayOfNextMonth)) {
    print("${"%2d".format(dayIterator.dayOfMonth)} ")
    if (dayIterator.dayOfWeek == DayOfWeek.SATURDAY) println()
    dayIterator = dayIterator.plusDays(1)
}
```

- LocalDate.of() → 월의 첫날 생성
- dayOfWeek.value → 요일 정렬
- plusDays() → 날짜 반복
- "%2d".format() → 날짜 정렬 출력

### 🔍 출력 예시
```
Su Mo Tu We Th Fr Sa
       1  2  3  4  5  6
 7  8  9 10 11 12 13
...
```




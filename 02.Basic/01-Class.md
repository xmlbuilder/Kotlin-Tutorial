# 클래스 (Kotlin 버전)
## 1️⃣ 클래스가 필요한 이유
### 문제: 학생 정보 출력
```kotlin
val student1Name = "학생1"
val student1Age = 15
val student1Grade = 90
```

- 학생 수가 늘어날수록 변수 선언과 출력 코드가 증가
- 유지보수 어려움
### 개선: 배열 사용
```kotlin
val names = arrayOf("학생1", "학생2")
val ages = arrayOf(15, 16)
val grades = arrayOf(90, 80)
```

- 반복문으로 출력 가능
- 하지만 데이터가 분산되어 관리가 어려움

## 2️⃣ 클래스 정의와 객체 생성
### 클래스 정의
```kotlin
class Student {
    var name: String = ""
    var age: Int = 0
    var grade: Int = 0
}
```

- 클래스는 객체를 생성하기 위한 설계도
- 멤버 변수(프로퍼티): 객체가 가지는 속성
### 객체 생성
```kotlin
val student1 = Student()
student1.name = "학생1"
student1.age = 15
student1.grade = 90
```

- Student()로 객체 생성
- . 연산자로 멤버 변수 접근

## 3️⃣ 객체와 참조값
- 객체는 메모리에 생성되고, 변수에는 참조값이 저장됨
- 참조값을 통해 객체에 접근
```kotlin
val student1 = Student() // x001
val student2 = Student() // x002
```

- student1은 x001 객체를 참조
- student2는 x002 객체를 참조
→ 변수에는 인스턴스 자체가 아니라 참조값이 들어있다

## 4️⃣ 배열과 객체
### 객체 배열 선언
```kotlin
val students = arrayOf(student1, student2)
```

- 배열 요소도 참조값을 저장
- students[0] → x001 → student1 객체
객체 배열 출력
```kotlin
println(students[0].name)
println(students[1].name)
```


## 5️⃣ 객체 출력
### for문 사용
```kotlin
for (i in students.indices) {
    println("이름:${students[i].name} 나이:${students[i].age} 성적:${students[i].grade}")
}
```


### 향상된 for문
```kotlin
for (s in students) {
    println("이름:${s.name} 나이:${s.age} 성적:${s.grade}")
}
```

- 코드가 간결하고 가독성이 좋음

## 6️⃣ 문제와 풀이
### 🎬 영화 리뷰 관리하기 1
```kotlin
class MovieReview {
    var title: String = ""
    var review: String = ""
}

val inception = MovieReview().apply {
    title = "인셉션"
    review = "인생은 무한 루프"
}
```

### 🎬 영화 리뷰 관리하기 2 (배열 도입)
```kotlin
val aboutTime = MovieReview().apply {
    title = "어바웃 타임"
    review = "시간을 되돌릴 수 있다면"
}

val reviews = arrayOf(inception, aboutTime)

for (review in reviews) {
    println("영화 제목: ${review.title}, 리뷰: ${review.review}")
}
```


### 🛒 상품 주문 시스템 개발
```kotlin
class ProductOrder {
    var productName: String = ""
    var price: Int = 0
    var quantity: Int = 0
}

val order1 = ProductOrder().apply {
    productName = "노트북"
    price = 1000000
    quantity = 1
}
val order2 = ProductOrder().apply {
    productName = "마우스"
    price = 30000
    quantity = 2
}
val order3 = ProductOrder().apply {
    productName = "키보드"
    price = 50000
    quantity = 1
}

val orders = arrayOf(order1, order2, order3)

var totalAmount = 0
for (order in orders) {
    println("상품명: ${order.productName}, 가격: ${order.price}, 수량: ${order.quantity}")
    totalAmount += order.price * order.quantity
}
println("총 결제 금액: $totalAmount")

```


## 7️⃣ 📥 정리
| 개념         | 설명                                      |
|--------------|-------------------------------------------|
| 클래스       | 객체를 생성하기 위한 설계도                |
| 객체         | 클래스 기반으로 생성된 실체                |
| 인스턴스     | 특정 클래스에서 생성된 객체                |
| 참조값       | 객체의 메모리 주소                        |
| 배열         | 객체들을 하나의 구조로 묶어 관리           |
| 향상된 for문 | 배열 요소를 간결하게 순회하는 반복문       |

---

## ✅ apply의 동작 방식
```kotlin
val order = ProductOrder().apply {
    productName = "노트북"
    price = 1_000_000
    quantity = 1
}
```

- ProductOrder()로 객체를 생성하고,
- apply { ... } 블록 안에서 this를 생략한 채 프로퍼티를 설정하고,
- 마지막에 그 객체 자체를 반환합니다.
즉, 위 코드는 다음과 동일한 의미:
```kotlin
val order = ProductOrder()
order.productName = "노트북"
order.price = 1_000_000
order.quantity = 1
```

---







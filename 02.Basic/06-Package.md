# 📦 Kotlin에서의 패키지란?
- Kotlin에서도 관련된 클래스들을 기능별로 그룹화하기 위해 패키지를 사용합니다.
- 자바와 마찬가지로 디렉토리 구조와 패키지 이름이 일치해야 하며, 유지보수와 확장에 매우 유리합니다.

## 🧩 예시: 쇼핑몰 시스템 클래스 분류
### 🔹 작은 프로그램
```
Order.kt  
User.kt  
Product.kt
```

### 🔹 큰 프로그램
```
User.kt, UserManager.kt, UserHistory.kt  
Product.kt, ProductCatalog.kt, ProductImage.kt  
Order.kt, OrderService.kt, OrderHistory.kt  
ShoppingCart.kt, CartItem.kt  
Payment.kt, PaymentHistory.kt  
Shipment.kt, ShipmentTracker.kt
```

### 🔹 패키지로 분류
```
user     → User, UserManager, UserHistory  
product  → Product, ProductCatalog, ProductImage  
order    → Order, OrderService, OrderHistory  
cart     → ShoppingCart, CartItem  
payment  → Payment, PaymentHistory  
shipping → Shipment, ShipmentTracker

```

## 📌 Kotlin 패키지 사용법
### 1️⃣ 패키지 선언
```kotlin
package pack
```

- 파일의 첫 줄에 선언
- 디렉토리 구조와 반드시 일치해야 함 (pack/Data.kt)

### 2️⃣ 클래스 정의
```kotlin
package pack

class Data {
    init {
        println("패키지 pack Data 생성")
    }
}
```


### 3️⃣ 하위 패키지 사용
```kotlin
package pack.a

class User {
    init {
        println("패키지 pack.a 회원 생성")
    }
}
```


### 4️⃣ 다른 클래스에서 사용
```kotlin
package pack

import pack.a.User

fun main() {
    val data = Data()       // 같은 패키지 → import 없이 사용 가능
    val user = User()       // 다른 패키지 → import 필요
}
```


### ✅ 실행 결과
```
패키지 pack Data 생성  
패키지 pack.a 회원 생성
```

## 🎯 Kotlin 패키지 핵심 개념
| 항목               | 문법/예시                  | 설명                                               |
|--------------------|----------------------------|----------------------------------------------------|
| 패키지 선언         | `package 패키지명`          | 파일의 첫 줄에 작성, 디렉토리 구조와 일치해야 함     |
| 클래스 사용         | `import pack.a.User`        | 다른 패키지의 클래스를 사용할 때 전체 경로 생략 가능 |
| 와일드카드 import   | `import pack.a.*`           | 해당 패키지의 모든 클래스 사용 가능 (권장되지 않음) |
| 클래스 이름 중복    | `pack.a.User`, `pack.b.User`| 중복 시 하나만 import, 나머지는 전체 경로로 사용     |
| 디렉토리 구조        | `pack/a/User.kt`            | 패키지 이름과 실제 폴더 위치는 반드시 일치해야 함     |



## 📘 Kotlin 프로젝트 구조 예시: helloshop
### 📦 디렉토리 구조 요약
```
com.helloshop
  ├── user
  │   ├── User.kt
  │   └── UserService.kt
  ├── product
  │   ├── Product.kt
  │   └── ProductService.kt
  └── order
      ├── Order.kt
      ├── OrderService.kt
      └── OrderHistory.kt
```


## 🧩 샘플 코드: OrderService 사용 예시
```kotlin
// 파일 위치: com/helloshop/order/OrderService.kt
package com.helloshop.order

import com.helloshop.user.User
import com.helloshop.product.Product

class OrderService {
    fun order() {
        val user = User("user123", "JungHwan")
        val product = Product("prod456", 30000)
        val order = Order(user, product)

        println("주문 생성 완료: ${user.name} → ${product.productId}")
    }
}
```


## 📘 실행 클래스 예시
```kotlin
// 파일 위치: com/helloshop/OrderApp.kt
package com.helloshop

import com.helloshop.order.OrderService

fun main() {
    val orderService = OrderService()
    orderService.order()
}
```


## ✅ 실행 결과 예시
```
주문 생성 완료: JungHwan → prod456
```


## 📌 설명 요약: helloshop 패키지 구조
| 패키지 이름             | 포함 클래스 목록                          | 주요 역할 및 설명                          |
|-------------------------|-------------------------------------------|--------------------------------------------|
| com.helloshop.user      | User, UserService                         | 사용자 정보 관리 및 사용자 관련 기능 처리   |
| com.helloshop.product   | Product, ProductService                   | 상품 정보 관리 및 상품 관련 기능 처리       |
| com.helloshop.order     | Order, OrderService, OrderHistory         | 주문 생성, 주문 처리, 주문 이력 관리        |



## 🔄 패키지 간 관계
- OrderService는 User, Product, Order를 사용 → import 필요
- 각 클래스는 public으로 선언되어야 외부 패키지에서 접근 가능
- 기능별로 패키지를 나누면 유지보수와 확장성이 높아짐

---

# 멀티 모듈 프로젝트

Kotlin과 Java 모두 Gradle 기반 멀티 모듈 프로젝트를 지원하지만,  
Kotlin은 Kotlin DSL과 더 강력한 구조적 표현 덕분에 설정이 더 명확하고 간결합니다.

## 🧩 Kotlin의 모듈 분리와 멀티 모듈 프로젝트
### 1️⃣ 모듈이란?
- **모듈(Module)** 은 기능별로 코드를 분리한 독립적인 단위
- 각 모듈은 자체 build.gradle.kts 파일을 가지며, 독립적으로 컴파일됨
- 모듈 간 의존성을 설정해 하나의 애플리케이션으로 통합 가능

### 2️⃣ 디렉토리 구조 예시
```
my-app/
├── build.gradle.kts          // 루트 설정
├── settings.gradle.kts       // 모듈 포함 선언
├── kotlin-base/              // 공통 로직
│   └── build.gradle.kts
├── kotlin-web/               // 웹 관련 로직
│   └── build.gradle.kts
```

### 3️⃣ settings.gradle.kts
```
rootProject.name = "my-app"
include("kotlin-base", "kotlin-web")
```

###  4️⃣ 각 모듈의 build.gradle.kts
```
plugins {
    kotlin("jvm") version "1.9.10"
}

dependencies {
    implementation(project(":kotlin-base"))
}
```


## ⚙️ Kotlin DSL vs Java (Groovy DSL)
| 항목               | .kts (Kotlin DSL)                          | .gradle (Groovy DSL)                      |
|--------------------|--------------------------------------------|-------------------------------------------|
| 파일 확장자         | `.gradle.kts`                              | `.gradle`                                  |
| 플러그인 선언       | `plugins { kotlin("jvm") version "1.9.10" }` | `plugins { id 'org.jetbrains.kotlin.jvm' }` |
| 의존성 선언         | `implementation(project(":module"))`       | `implementation project(':module')`       |
| 타입 안정성         | 정적 타입 → IDE 자동완성, 오류 사전 방지     | 동적 타입 → IDE 지원 제한, 런타임 오류 가능 |
| 문법 스타일         | 함수형, 구조적 표현                         | 문자열 기반, 유연하지만 가독성 낮음         |
| 유지보수 및 리팩토링 | 쉬움 (IDE 지원 강력)                       | 어려움 (구조 파악 어려움)                   |



## ✅ Gradle 설정 팁
- Kotlin DSL은 .gradle.kts 확장자를 사용
- buildSrc 또는 convention plugins로 공통 설정을 모듈화 가능
- allprojects 또는 subprojects 블록으로 공통 설정 적용 가능
```
subprojects {
    repositories {
        mavenCentral()
    }
}
```

## 🔄 Kotlin vs Java: 멀티 모듈 차이점 요약
| 항목               | .kts (Kotlin DSL)                  | .gradle (Groovy DSL)               |
|--------------------|------------------------------------|------------------------------------|
| 파일 확장자         | `.gradle.kts`                      | `.gradle`                          |
| 문법 스타일         | 정적 타입, 함수형 표현              | 동적 타입, 문자열 기반 표현         |
| IDE 지원            | 자동완성, 타입 체크 강력             | 제한적 자동완성, 오류 탐지 어려움    |
| 의존성 선언         | `implementation(project(":module"))` | `implementation project(':module')` |
| 유지보수            | 구조적 표현으로 리팩토링 쉬움        | 유연하지만 오류 발생 가능성 높음     |
| 학습 곡선           | Kotlin 개발자에게 친숙               | Java 개발자에게 익숙                |


## 💡 실전 팁
- Kotlin 프로젝트에서는 Kotlin DSL을 사용하는 것이 권장됨
- 모듈은 기능별로 나누되, 공통 로직은 core, base, common 같은 이름으로 별도 분리
- settings.gradle.kts에서 모든 모듈을 명시적으로 include() 해야 함
- 멀티 모듈은 모놀리식 구조를 기능 단위로 나누는 데 효과적이며, 마이크로서비스로 확장하기 전 단계로도 활용 가능

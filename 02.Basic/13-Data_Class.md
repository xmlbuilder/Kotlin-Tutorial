# data class

Kotlin에서 data class는 일반 클래스와 달리 데이터를 담기 위한 목적으로 특별히 설계된 클래스.  
data 키워드를 붙이면 코틀린이 자동으로 여러 유용한 기능을 생성.

## ✅ data class의 의미와 기능
data 키워드를 붙이면 코틀린이 다음 메서드들을 자동으로 생성합니다:  
| 메서드         | 설명                                                              |
|----------------|-------------------------------------------------------------------|
| `equals()`     | 객체의 내용이 같은지 비교. 참조값이 아닌 필드 값 기준으로 비교       |
| `hashCode()`   | 객체의 해시코드 반환. 컬렉션에서 키로 사용할 때 유용                 |
| `toString()`   | 객체 정보를 문자열로 반환. 예: `User(id=100, name="JungHwan")`     |
| `copy()`       | 기존 객체를 기반으로 일부 값만 바꾼 새 객체 생성                     |
| `componentN()` | 구조 분해 선언에서 사용. 예: `val (id, name) = user`               |


## 📦 예제 비교
### 일반 클래스
```kotlin
class User(val id: String, val name: String)
```
- `equals()`, `toString()` 등은 자동 생성되지 않음
- 비교나 출력 시 직접 오버라이딩 필요

## 데이터 클래스
```kotlin
data class User(val id: String, val name: String)
```
- `equals()`, `hashCode()`, `toString()`, `copy()` 자동 생성
- 데이터 중심 로직에 매우 유용
  

### ✨ 사용 예시
```kotlin
val user1 = User("id-100", "JungHwan")
val user2 = user1.copy(name = "Minji")

println(user1 == user2) // false (name이 다름)
println(user2)          // User(id=id-100, name=Minji)
```

- 요약하자면, data class는 값 중심의 객체를 다룰 때 코드를 간결하고 안전하게 만들어주는 코틀린의 강력한 기능.

---



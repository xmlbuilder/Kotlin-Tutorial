# Copy

## 🔍 copy()는 얕은 복사 (Shallow Copy)
### 예제: 참조형 필드가 있는 data class
```kotlin
data class Address(val city: String)
data class User(val name: String, val address: Address)

val original = User("JungHwan", Address("Seoul"))
val copied = original.copy()

copied.address.city = "Busan"
println(original.address.city) // Busan
```

- User 객체는 복사되었지만 address는 같은 객체를 참조함
- 따라서 copied.address.city를 변경하면 original.address.city도 함께 바뀜

## ✅ 깊은 복사(Deep Copy)를 원할 때
직접 구현하거나 확장 함수로 처리해야 합니다:
```kotlin
fun User.deepCopy(): User {
    return User(name, Address(address.city))
}
```
- 이제 deepCopy()를 사용하면 내부 객체까지 새로 생성되어 완전히 독립된 객체가 됩니다.

## 📚 요약 — Kotlin의 copy() vs deepCopy()
| 메서드       | 설명                                                                 |
|--------------|----------------------------------------------------------------------|
| `copy()`     | 얕은 복사. 참조형 필드는 주소만 복사되므로 원본 객체와 내부 객체를 공유함 |
| `deepCopy()` | 깊은 복사. 내부 객체까지 새로 생성하여 원본과 완전히 독립된 객체를 반환함 |

---




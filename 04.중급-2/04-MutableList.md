
# MutableList

Kotlin의 컬렉션은 MutableList입니다.  
Kotlin은 Java 컬렉션을 그대로 사용할 수도 있지만, 자체적으로 더 간결하고 안전한 방식으로 컬렉션을 다룹니다.  
아래에 Java의 ArrayList 주요 메서드에 대응되는 Kotlin 방식과 예제를 정리:

🧰 Kotlin MutableList 주요 메서드 정리
| Java ArrayList 메서드     | Kotlin MutableList 대응 메서드 / 표현     |
|---------------------------|-------------------------------------------|
| `add(E e)`                | `list.add(e)`                             |
| `add(int index, E e)`     | `list.add(index, e)`                      |
| `get(int index)`          | `list[index]`                             |
| `set(int index, E e)`     | `list[index] = e`                         |
| `remove(int index)`       | `list.removeAt(index)`                   |
| `remove(Object o)`        | `list.remove(e)`                          |
| `size()`                  | `list.size`                               |
| `isEmpty()`               | `list.isEmpty()`                          |
| `contains(Object o)`      | `list.contains(e)`                        |
| `indexOf(Object o)`       | `list.indexOf(e)`                         |
| `clear()`                 | `list.clear()`                            |
| `subList(from, to)`       | `list.subList(from, to)`                  |
| `sort()`                  | `list.sort()`                             |
| `equals(Object o)`        | `list == otherList`                       |
| `clone()`                 | `list.toMutableList()`                    |

## 🧪 Kotlin 샘플 예제: 기본 사용법
```kotlin
fun main() {
    val fruits = mutableListOf("Apple", "Banana", "Cherry")

    println(fruits[1]) // Banana
    fruits[1] = "Blueberry"
    fruits.remove("Apple")

    println(fruits) // [Blueberry, Cherry]
    println("Contains Cherry? ${fruits.contains("Cherry")}")
    println("Size: ${fruits.size}")
}
```

## 🧑‍💻 실전 예제: 정렬, 필터링, 복제
```kotlin
fun main() {
    val scores = mutableListOf(85, 92, 78, 90, 88)

    scores.sort()
    println("Sorted: $scores")

    val topScores = scores.subList(2, 5)
    println("Top Scores: $topScores")

    scores.removeAll { it < 80 }
    println("Filtered: $scores")

    val clone = scores.toMutableList()
    println("Cloned: $clone")
}
```
###  출력 결과
```
Sorted: [78, 85, 88, 90, 92]
Top Scores: [88, 90, 92]
Filtered: [85, 88, 90, 92]
Cloned: [85, 88, 90, 92]

```

## 📌 Kotlin 컬렉션 특징
- List는 읽기 전용, MutableList는 수정 가능
- list[index] 문법으로 접근/수정 가능
- == 비교는 내용 기반 (Java의 .equals()와 동일)
- toMutableList()는 얕은 복사

---




# Map

코틀린 스타일에 맞게 val, mutableMapOf, mapOf, forEach, entries, keys, values 등을 활용.

## 📦 Map 인터페이스 개요
### 🔑 Map 구현체 특징 요약
| 항목               | HashMap                  | LinkedHashMap              | TreeMap                    |
|--------------------|--------------------------|-----------------------------|----------------------------|
| 순서 유지 여부     | ❌ 유지 안 됨             | ✅ 입력 순서 유지           | ❌ 입력 순서 무시, 값 기준 정렬 |
| 정렬 여부          | ❌ 없음                  | ❌ 없음                     | ✅ 자동 정렬됨             |
| 성능               | `O(1)` 평균              | `O(1)` 평균                 | `O(log n)`                 |
| 내부 구조          | 해시 테이블              | 해시 테이블 + 링크드 리스트 | 레드-블랙 트리             |
| 키 중복 여부       | ❌ 중복 불가             | ❌ 중복 불가                | ❌ 중복 불가               |
| 값 중복 여부       | ✅ 중복 가능             | ✅ 중복 가능                | ✅ 중복 가능               |
| 주요 용도          | 빠른 검색, 기본 맵       | 순서 유지가 필요한 경우     | 정렬된 맵, 범위 검색 필요 시 |


## 🧪 주요 메서드별 샘플과 설명
### 1. put(key, value)
```kotlin
val map = mutableMapOf<String, Int>()
map["A"] = 100
map["A"] = 200 // 기존 값 덮어쓰기
println(map) // {A=200}
```

### 2. putIfAbsent(key, value)
```kotlin
map.putIfAbsent("A", 300) // 이미 있으므로 무시
map.putIfAbsent("B", 400) // 없으므로 저장
println(map) // {A=200, B=400}
```

### 3. get(key)
```kotlin
val score = map["A"]
println(score) // 200
```

### 4. getOrDefault(key, defaultValue)
```kotlin
val score = map.getOrDefault("C", 0)
println(score) // 0
```

### 5. remove(key)
```kotlin
map.remove("A")
println(map) // {B=400}
```

### 6. clear()
```kotlin
map.clear()
println(map) // {}
```

### 7. containsKey(key)
```kotlin
val exists = map.containsKey("B")
println(exists) // true
```

### 8. containsValue(value)
```kotlin
val hasValue = map.containsValue(400)
println(hasValue) // true
```

### 9. keySet()
```kotlin
for (key in map.keys) {
    println("key = $key")
}
```

### 10. values()
```kotlin
for (value in map.values) {
    println("value = $value")
}
```

### 11. entrySet()
```kotlin
for ((key, value) in map.entries) {
    println("key = $key, value = $value")
}
```

### 12. size / isEmpty
```kotlin
println(map.size)     // 2
println(map.isEmpty()) // false
```


### 🧠 Map 구현체 비교 요약
| 구현체           | 순서 유지 여부       | 정렬 여부         | 성능 (검색/삽입/삭제) | 내부 구조             | 특징 및 용도                          |
|------------------|----------------------|--------------------|------------------------|------------------------|---------------------------------------|
| `HashMap`        | ❌ (순서 없음)        | ❌ (정렬 안 됨)     | `O(1)` 평균            | 해시 테이블            | 가장 많이 사용됨, 빠른 성능           |
| `LinkedHashMap`  | ✅ (입력 순서 유지)   | ❌ (정렬 안 됨)     | `O(1)` 평균            | 해시 테이블 + 링크드 리스트 | 순서 유지 필요 시 사용               |
| `TreeMap`        | ❌ (입력 순서 무시)   | ✅ (자동 정렬됨)    | `O(log n)`             | 레드-블랙 트리         | 정렬된 키-값 저장, 범위 검색 가능     |


### ✅ 실무 팁 
- 중복 없는 키-값 저장: putIfAbsent()
- 기본값 처리: getOrDefault()
- 전체 순회: entries 사용
- 정렬 필요: TreeMap 사용
- 입력 순서 유지: LinkedHashMap 사용

---


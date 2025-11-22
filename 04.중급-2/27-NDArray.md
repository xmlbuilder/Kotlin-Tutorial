# NDArray
이 코드는 Kotlin에서 DJL (Deep Java Library)를 활용해 NumPy처럼 NDArray 연산을 수행하는 예제입니다.
아래에 단계별로 기능을 설명하고, 전체 흐름을 정리합니다.

## 🧠 핵심 개념: DJL의 NDArray는 Kotlin에서도 NumPy처럼 사용 가능
- `NDArray`: 다차원 배열 객체 (NumPy의 ndarray와 유사)
- `NDManager`: NDArray를 생성하고 관리하는 객체 (메모리 관리 포함)
- `NDIndex`: 배열 인덱싱 및 슬라이싱 표현
- `Shape`: 배열의 차원 정보

## 전체 코드
```kotlin
package com.thj.thread

import ai.djl.ndarray.NDArray
import ai.djl.ndarray.NDManager
import ai.djl.ndarray.index.NDIndex
import ai.djl.ndarray.types.Shape

fun main() {
    NDManager.newBaseManager().use { manager ->

        var nd = manager.ones(Shape(2, 3))
        nd = nd.add(10)
        println(nd)
        println(nd.get(1, 1))
        nd.set(NDIndex(1, 1), 10)
        println(nd)

        val nd1 = manager.randomUniform(0f, 1f, Shape(1, 1, 4))
        println(nd1)

        var nd2 = manager.arange(1, 10).reshape(3, 3)
        nd2 = nd2.transpose()
        nd2 = nd2.add(10)
        println(nd2)

        var nd4 = manager.arange(5, 14)
        nd4 = nd4.get(nd4.gte(10))
        val arData = nd4.toIntArray()
        for (val_ in arData) {
            println(val_)
        }

        var nd5 = manager.arange(1, 10).reshape(3, 3)
        nd5.set(NDIndex(":,1")) { array -> array.mul(2) }
        println(nd5)

        val array = intArrayOf(1, 6, 5, 1, 3, 2, 8, 1, 12)
        var ndArray = manager.create(array).reshape(Shape(array.size.toLong(), 1))
        ndArray = ndArray.div(10)
        println(ndArray)
    }
}
```


## 🔍 단계별 설명
### [1] NDManager 생성 및 NDArray 초기화
```kotlin
val manager = NDManager.newBaseManager()
var nd = manager.ones(Shape(2, 3))   // 2x3 배열, 모든 값 1
nd = nd.add(10)                      // 모든 요소에 10 더하기
println(nd)                          // 전체 출력
println(nd.get(1, 1))                 // 특정 요소 접근
nd.set(NDIndex(1, 1), 10)            // 특정 요소 수정
println(nd)                          // 수정 후 출력
```

### [2] 랜덤 배열 생성
```kotlin
val nd1 = manager.randomUniform(0f, 1f, Shape(1, 1, 4))
println(nd1)                         // 0~1 사이 랜덤 값 출력
```

### [3] arange + reshape + transpose + add
```kotlin
var nd2 = manager.arange(1, 10)      // 1~9
nd2 = nd2.reshape(3, 3)              // 3x3 배열
nd2 = nd2.transpose()                // 전치
nd2 = nd2.add(10)                    // 모든 요소에 10 더하기
println(nd2)
```

### [4] 조건 필터링 및 배열 변환
```kotlin
var nd4 = manager.arange(5, 14)      // 5~13
nd4 = nd4.get(nd4.gte(10))           // 10 이상 필터링
val arData = nd4.toIntArray()        // Kotlin 배열로 변환
for (val_ in arData) {
    println(val_)                    // 값 출력
}
```

### [5] 슬라이싱 후 연산 적용
```kotlin
var nd5 = manager.arange(1, 10).reshape(3, 3) // 3x3 배열
nd5.set(NDIndex(":,1")) { array -> array.mul(2) } // 1번 열 * 2
println(nd5)
```

### [6] Kotlin 배열 → NDArray → 정규화
```kotlin
val array = intArrayOf(1, 6, 5, 1, 3, 2, 8, 1, 12)
var ndArray = manager.create(array).reshape(Shape(array.size.toLong(), 1))
ndArray = ndArray.div(10)            // 모든 값 10으로 나누기
println(ndArray)
```

## 📦 전체 흐름 요약

| 단계 | 기능                      | 메서드 또는 연산                 |
|------|---------------------------|----------------------------------|
| 1    | 기본 배열 생성 및 수정    | ones, add, get, set              |
| 2    | 랜덤 배열 생성            | randomUniform                    |
| 3    | arange + reshape + 전치   | arange, reshape, transpose, add  |
| 4    | 조건 필터링 및 배열 변환  | gte, get, toIntArray             |
| 5    | 슬라이싱 후 연산 적용     | NDIndex, set                     |
| 6    | 배열 생성 및 정규화       | create, reshape, div             |


## 🧠 참고: DJL은 어떤 라이브러리인가요?
- DJL (Deep Java Library): Java/Kotlin에서 딥러닝과 수치 연산을 할 수 있게 해주는 라이브러리
- NumPy처럼 NDArray를 지원하며, PyTorch, TensorFlow, MXNet 백엔드도 연결 가능
- 공식 사이트: https://djl.ai

---




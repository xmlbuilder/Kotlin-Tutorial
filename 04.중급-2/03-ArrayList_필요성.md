# 🧠 Kotlin 배열 정리
## 📌 배열이란?
- Kotlin 배열은 동일한 타입의 데이터를 연속된 메모리 공간에 저장하는 자료 구조입니다.  
- IntArray, Array<String> 등 다양한 타입의 배열을 사용할 수 있습니다.  

### ⚙️ 배열의 특징

| 연산 종류       | 예시 코드              | 설명                                      | 시간 복잡도 |
|----------------|------------------------|-------------------------------------------|--------------|
| 인덱스 입력     | `arr[2] = 10`           | 2번 위치에 값 10 저장                      | O(1)         |
| 인덱스 조회     | `val value = arr[2]`    | 2번 위치의 값 읽기                         | O(1)         |
| 인덱스 변경     | `arr[2] = 20`           | 2번 위치의 값을 20으로 수정                | O(1)         |
| 순차 검색       | `for (i in arr)`        | 배열 전체를 순회하며 값 찾기               | O(n)         |
| 첫 위치 삽입    | `addFirst(arr, x)`      | 모든 요소를 뒤로 밀고 첫 위치에 삽입       | O(n)         |
| 중간 삽입       | `addAtIndex(arr, i, x)` | 해당 위치 이후 요소를 뒤로 밀고 삽입       | O(n)         |
| 마지막 삽입     | `addLast(arr, x)`       | 마지막 위치에 직접 삽입                    | O(1)         |


## 🧮 배열 인덱스 계산 공식
배열은 메모리상에 연속적으로 배치되며, 각 요소의 주소는 다음과 같이 계산됩니다:  
주소 = 배열 시작 주소 + (자료 크기 * 인덱스)  

### 예: Int는 4바이트
```
arr[0] → x100 + (4 * 0) = x100  
arr[1] → x100 + (4 * 1) = x104  
arr[2] → x100 + (4 * 2) = x108  
```

## 💻 Kotlin 배열 삽입 예제 코드
```kotlin
fun main() {
    val arr = IntArray(5)
    arr[0] = 1
    arr[1] = 2
    println(arr.joinToString())

    println("배열의 첫번째 위치에 3 추가 O(n)")
    addFirst(arr, 3)
    println(arr.joinToString())

    println("배열의 index(2) 위치에 4 추가 O(n)")
    addAtIndex(arr, 2, 4)
    println(arr.joinToString())

    println("배열의 마지막 위치에 5 추가 O(1)")
    addLast(arr, 5)
    println(arr.joinToString())
}

fun addLast(arr: IntArray, newValue: Int) {
    arr[arr.size - 1] = newValue
}

fun addFirst(arr: IntArray, newValue: Int) {
    for (i in arr.size - 1 downTo 1) {
        arr[i] = arr[i - 1]
    }
    arr[0] = newValue
}

fun addAtIndex(arr: IntArray, index: Int, newValue: Int) {
    for (i in arr.size - 1 downTo index + 1) {
        arr[i] = arr[i - 1]
    }
    arr[index] = newValue
}
```

### ⚠️ 배열의 한계
- 크기 고정: 생성 시 크기를 정해야 하며 이후 변경 불가
- 동적 확장 불가: 실시간 데이터 추가에 유연하지 않음
- 메모리 낭비 가능성: 너무 크면 낭비, 너무 작으면 부족

## ✅ 대안: Kotlin 컬렉션
Kotlin은 배열의 한계를 극복하기 위해 다양한 컬렉션 클래스를 제공합니다:
- mutableListOf(): 동적으로 크기 조절 가능
- LinkedList: 삽입/삭제에 유리 (Java interop)
- HashSet, HashMap: 검색/저장에 특화

---

# 📦 왜 ArrayList가 필요한가?

## 🔧 배열의 한계
| 문제점                     | 설명                                                                 |
|----------------------------|----------------------------------------------------------------------|
| 크기 고정                  | 배열은 생성 시 크기를 정해야 하며, 이후 변경할 수 없음                     |
| 삽입/삭제 불편             | 중간에 데이터를 넣거나 빼려면 직접 요소를 이동시켜야 함                    |
| 동적 확장 불가             | 실시간으로 데이터가 늘어날 경우 대응이 어려움                              |
| 타입 안전성 부족           | 모든 타입을 허용하므로 실수로 잘못된 타입을 넣을 수 있음                    |
| 예외 발생 가능             | 크기를 초과해 데이터를 추가하면 `ArrayIndexOutOfBoundsException` 발생 |

- ➡️ 이런 불편함을 해결하기 위해 **동적으로 크기 조절 가능한 리스트(List)** 가 필요합니다.

## 📚 리스트란?
- 순서가 있고 중복을 허용하는 자료 구조
- 배열과 유사하지만 크기가 동적으로 변함
- Kotlin에서는 ArrayList, MutableList, LinkedList 등 다양한 리스트 구현체 제공
  
| 자료 구조   | 순서 유지 | 중복 허용 | 크기 조절 | 삽입/삭제 성능 | 대표 구현체         |
|-------------|-----------|------------|------------|------------------|----------------------|
| 배열 (Array) | ✅        | ✅         | ❌ (고정)  | 불편함 (O(n))     | `IntArray`, `Array<String>` |
| 리스트 (List)| ✅        | ✅         | ✅ (동적)  | 유연함 (O(1)~O(n))| `MutableList`, `ArrayList` |



## 🛠️ 직접 구현
### 🎯 목표
- 배열 기반 리스트를 직접 구현해보고, 배열의 한계를 체험
- 고정 크기 배열을 사용하여 add, get, set, indexOf 기능 제공

```kotlin
class MyArrayListV1 {
    private val defaultCapacity = 5
    private var elementData = arrayOfNulls<Any>(defaultCapacity)
    private var size = 0

    fun size(): Int = size

    fun add(e: Any) {
        elementData[size] = e
        size++
    }

    fun get(index: Int): Any? = elementData[index]

    fun set(index: Int, element: Any): Any? {
        val oldValue = get(index)
        elementData[index] = element
        return oldValue
    }

    fun indexOf(o: Any): Int {
        for (i in 0 until size) {
            if (elementData[i] == o) return i
        }
        return -1
    }

    override fun toString(): String {
        return elementData.sliceArray(0 until size).contentToString() +
                " size=$size, capacity=${elementData.size}"
    }
}
```


### 🧪 실행 예제
```kotlin
fun main() {
    val list = MyArrayListV1()
    println("==데이터 추가==")
    println(list)

    list.add("a")
    println(list)

    list.add("b")
    println(list)

    list.add("c")
    println(list)

    println("==기능 사용==")
    println("list.size(): ${list.size()}")
    println("list.get(1): ${list.get(1)}")
    println("list.indexOf(\"c\"): ${list.indexOf("c")}")
    println("list.set(2, \"z\"), oldValue: ${list.set(2, "z")}")
    println(list)

    println("==범위 초과==")
    list.add("d")
    list.add("e")
    list.add("f") // 예외 발생 가능
    println(list)
}
```

## 🛠️ 직접 구현 정리
### ✅ V1: 고정 크기 배열 기반 리스트
```kotlin
fun add(e: Any) {
    elementData[size] = e
    size++
}
```

- 배열 크기 초과 시 예외 발생
- 삽입/삭제는 단순하지만 확장 불가

### ✅ V2: 동적 배열 확장
```kotlin
private fun grow() {
    val newCapacity = elementData.size * 2
    elementData = elementData.copyOf(newCapacity)
}
```

- add() 시 배열이 꽉 차면 grow() 호출
- 배열을 2배로 확장하고 기존 데이터 복사

### ✅ V3: 삽입/삭제 기능 추가
```kotlin
fun add(index: Int, e: Any) {
    shiftRightFrom(index)
    elementData[index] = e
    size++
}

fun remove(index: Int): Any? {
    val oldValue = get(index)
    shiftLeftFrom(index)
    size--
    return oldValue
}
```

- 중간 삽입/삭제 시 요소 이동 필요 → O(n)
- 마지막 삽입/삭제는 빠름 → O(1)
  
| 연산 종류         | 설명                                      | 시간 복잡도 |
|------------------|-------------------------------------------|--------------|
| `add(e)`         | 리스트의 마지막에 데이터 추가               | O(1)         |
| `add(index, e)`  | 중간 또는 앞에 삽입 → 요소 이동 필요        | O(n)         |
| `remove(size-1)` | 마지막 요소 삭제 → 이동 없음                | O(1)         |
| `remove(index)`  | 중간 또는 앞에 삭제 → 요소 왼쪽으로 이동 필요| O(n)         |



### 🚨 문제 발생: 타입 안전성 부족
```kotlin
val numberList = MyArrayListV3()
numberList.add(1)
numberList.add("문자3") // 실수로 문자 입력
val num3 = numberList.get(2) as Int // ClassCastException 발생
```

- ➡️ Any 기반 리스트는 모든 타입을 허용하므로 타입 오류 발생 가능성 있음

### ✅ 해결: 제네릭 도입
```kotlin
class MyArrayListV4<E>(initialCapacity: Int = DEFAULT_CAPACITY) {
    companion object {
        private const val DEFAULT_CAPACITY = 5
    }

    private var elementData: Array<Any?> = arrayOfNulls(initialCapacity)
    private var size = 0

    fun size(): Int = size

    fun add(e: E) {
        if (size == elementData.size) grow()
        elementData[size] = e
        size++
    }

    fun add(index: Int, e: E) {
        if (size == elementData.size) grow()
        shiftRightFrom(index)
        elementData[index] = e
        size++
    }

    private fun shiftRightFrom(index: Int) {
        for (i in size downTo index + 1) {
            elementData[i] = elementData[i - 1]
        }
    }

    fun get(index: Int): E {
        @Suppress("UNCHECKED_CAST")
        return elementData[index] as E
    }

    fun set(index: Int, element: E): E {
        val oldValue = get(index)
        elementData[index] = element
        return oldValue
    }

    fun remove(index: Int): E {
        val oldValue = get(index)
        shiftLeftFrom(index)
        size--
        elementData[size] = null
        return oldValue
    }

    private fun shiftLeftFrom(index: Int) {
        for (i in index until size - 1) {
            elementData[i] = elementData[i + 1]
        }
    }

    fun indexOf(o: E): Int {
        for (i in 0 until size) {
            if (elementData[i] == o) return i
        }
        return -1
    }

    private fun grow() {
        val newCapacity = elementData.size * 2
        elementData = elementData.copyOf(newCapacity)
    }

    override fun toString(): String {
        return elementData.slice(0 until size).toString() +
                " size=$size, capacity=${elementData.size}"
    }
}
```

### ✅ 사용 예제
```kotlin
fun main() {
    val stringList = MyArrayListV4<String>()
    stringList.add("a")
    stringList.add("b")
    stringList.add("c")
    val str = stringList.get(0)
    println("string = $str")

    val intList = MyArrayListV4<Int>()
    intList.add(1)
    intList.add(2)
    intList.add(3)
    val num = intList.get(0)
    println("integer = $num")
}
```

### 🧪 실행 결과
```
string = a
integer = 1
```

## 🧠 제네릭과 Object 배열의 관계
```kotlin
private var elementData: Array<Any?> = arrayOfNulls(DEFAULT_CAPACITY)

fun add(e: String) {
    elementData[size] = e
}

fun get(index: Int): String {
    return elementData[index] as String
}
```

- Any[]에 저장하지만, E 타입으로만 접근
- 컴파일 시 타입 체크, 런타임에 안전한 다운캐스팅

## ⚠️ MyArrayList의 단점
| 단점                          | 설명                                                                 |
|-------------------------------|----------------------------------------------------------------------|
| 메모리 낭비 가능성            | 배열 크기를 넉넉히 잡으면 사용되지 않는 공간이 생김                     |
| 중간 삽입/삭제 성능 저하      | 요소를 한 칸씩 이동해야 하므로 O(n)                                   |
| 제네릭 배열 생성 불가         | `new E[]`는 불가능 → `Array<Any?>`로 대체                              |


## 📊 ArrayList의 빅오 정리
| 연산 종류         | 시간 복잡도 |
|------------------|--------------|
| 마지막에 추가     | O(1)         |
| 앞/중간에 추가    | O(n)         |
| 마지막에 삭제     | O(1)         |
| 앞/중간에 삭제    | O(n)         |
| 인덱스 조회       | O(1)         |
| 데이터 검색       | O(n)         |


- ➡️ 순차적 추가/조회에 최적화된 구조
- ➡️ 중간 삽입/삭제가 많다면 LinkedList 고려 필요


---

## 샘플 코드
```kotlin
class MyArrayListV4<E>(initialCapacity: Int = DEFAULT_CAPACITY) {
    companion object {
        private const val DEFAULT_CAPACITY = 5
    }

    private var elementData: Array<Any?> = arrayOfNulls(initialCapacity)
    private var size = 0

    fun size(): Int = size

    fun add(e: E) {
        if (size == elementData.size) grow()
        elementData[size] = e
        size++
    }

    fun add(index: Int, e: E) {
        if (size == elementData.size) grow()
        shiftRightFrom(index)
        elementData[index] = e
        size++
    }

    private fun shiftRightFrom(index: Int) {
        for (i in size downTo index + 1) {
            elementData[i] = elementData[i - 1]
        }
    }

    @Suppress("UNCHECKED_CAST")
    fun get(index: Int): E {
        return elementData[index] as E
    }

    fun set(index: Int, element: E): E {
        val oldValue = get(index)
        elementData[index] = element
        return oldValue
    }

    fun remove(index: Int): E {
        val oldValue = get(index)
        shiftLeftFrom(index)
        size--
        elementData[size] = null
        return oldValue
    }

    private fun shiftLeftFrom(index: Int) {
        for (i in index until size - 1) {
            elementData[i] = elementData[i + 1]
        }
    }

    fun indexOf(o: E): Int {
        for (i in 0 until size) {
            if (elementData[i] == o) return i
        }
        return -1
    }

    private fun grow() {
        val newCapacity = elementData.size * 2
        elementData = elementData.copyOf(newCapacity)
    }

    override fun toString(): String {
        return elementData.slice(0 until size).toString() +
                " size=$size, capacity=${elementData.size}"
    }
}
```


## ✅ 사용 예제
```kotlin
fun main() {
    val stringList = MyArrayListV4<String>()
    stringList.add("a")
    stringList.add("b")
    stringList.add("c")
    val string = stringList.get(0)
    println("string = $string")

    val intList = MyArrayListV4<Int>()
    intList.add(1)
    intList.add(2)
    intList.add(3)
    val integer = intList.get(0)
    println("integer = $integer")
}
```


## 🧪 실행 결과
```
string = a
integer = 1
```

---

# 제네릭 타입의 배열
Kotlin에서는 제네릭 타입의 배열을 직접 생성할 수 없음.  
그 이유는 JVM의 타입 소거(type erasure) 때문인데, 런타임에는 E 같은 제네릭 타입 정보가 사라지기 때문에 Array<E>를 바로 만들 수 없습니다.  
하지만 안전하게 제네릭 배열을 생성하는 방법은 있습니다:

## ✅ 방법 1: arrayOfNulls<Any?>() 사용 후 캐스팅
```kotlin
@Suppress("UNCHECKED_CAST")
val array: Array<E?> = arrayOfNulls<Any?>(size) as Array<E?>
```
- Any?로 만든 배열을 E?로 캐스팅
- 자바의 Object[]와 같은 방식
- 가장 일반적인 제네릭 배열 생성 패턴

## ✅ 방법 2: Array(size) { initializer } 사용
```kotlin
val array: Array<E?> = Array(size) { null }
```

- 초기값을 null로 설정
- 타입 추론이 어려운 경우 Array<E?>(size) { null }처럼 명시적으로 타입 지정

## ✅ 방법 3: reified 타입과 inline 함수 활용 (고급)
```kotlin
inline fun <reified T> createArray(size: Int): Array<T?> {
    return arrayOfNulls<T>(size)
}
```

- reified를 사용하면 런타임에도 타입 정보를 유지할 수 있음
- 단, 이건 inline 함수 안에서만 사용 가능

## 🔍 예제: 제네릭 리스트 구현에서 사용
```kotlin
class MyArrayListV4<E>(initialCapacity: Int = 5) {
    private var elementData: Array<E?> = arrayOfNulls<Any?>(initialCapacity) as Array<E?>
}
```

이렇게 하면 E 타입의 데이터를 안전하게 저장하고 꺼낼 수 있어요.  
Kotlin에서는 ArrayList<E>를 직접 쓰는 게 더 편하지만, 자료 구조를 직접 구현할 때는 위 방식이 핵심입니다.

---


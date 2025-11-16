# 🧠 LLVM이란?
LLVM은 **Low Level Virtual Machine** 의 약자지만, 지금은 가상 머신이 아니라 컴파일러 인프라로 사용됩니다.

## 🔧 핵심 개념
- 컴파일러 백엔드 플랫폼: 다양한 프로그래밍 언어를 기계어로 변환할 수 있도록 도와주는 도구 모음
- 중간 표현(IR): LLVM은 소스 코드를 중간 단계인 IR로 바꾼 뒤, 이를 다양한 플랫폼의 기계어로 컴파일함
- 다양한 타겟 지원: macOS, iOS, Linux, Windows, WebAssembly 등 거의 모든 플랫폼에 대응 가능

## 🚀 Kotlin/Native와 LLVM
Kotlin/Native는 JVM 없이 Kotlin 코드를 실행할 수 있도록 만든 기술입니다.  
이때 Kotlin/Native는 LLVM을 활용해 Kotlin 코드를 네이티브 바이너리로 컴파일합니다.  

## ✅ 특징

| 항목                         | Kotlin/JVM                              | Kotlin/Native (LLVM 기반)              |
|------------------------------|------------------------------------------|----------------------------------------|
| 실행 환경                   | JVM 위에서 실행                         | JVM 없이 네이티브 바이너리로 실행     |
| 타입 이레이저               | 있음                                     | 없음 (타입 정보 유지 가능)            |
| 리플렉션                    | Java 리플렉션 사용 가능                  | 제한적, 대신 `reified`로 대체 가능     |
| 제네릭 배열 생성            | 불가능 (`new T[]` 불가)                  | 가능 (`Array<T>` 생성 가능)           |
| 런타임 타입 정보 접근       | 제한적 (`T::class` 사용 불가)            | `T::class` 사용 가능                   |
| `reified` 지원              | `inline` 함수에서만 사용 가능            | 동일하게 지원, 더 유용하게 활용 가능  |


## 📦 예시: Kotlin/Native가 LLVM으로 컴파일되는 과정
- Kotlin 코드 작성
- Kotlin 컴파일러가 LLVM IR로 변환
- LLVM이 IR을 타겟 플랫폼의 기계어로 컴파일
- 결과: macOS 앱, iOS 앱, Linux CLI 등으로 실행 가능

## ✨ 요약
- LLVM은 다양한 언어를 다양한 플랫폼의 기계어로 변환해주는 컴파일러 인프라
- Kotlin/Native는 LLVM을 활용해 JVM 없이 Kotlin 코드를 실행할 수 있게 해줌
- 이 덕분에 타입 이레이저 없이 더 강력한 제네릭 기능을 구현할 수 있고, iOS나 임베디드 환경에서도 Kotlin을 사용할 수 있음
---



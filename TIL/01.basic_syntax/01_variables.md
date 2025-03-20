# 01_1. 변수와 자료형

# 변수(Variables)

Dart에서는 변수를 선언하고 `var`, `final`, `const` , 그리고 특정 타입을 사용하여 변수를 정의 할 수 있다.

# 변수 선언 (Declaring variables)

## var 키워드 사용

```dart
var name = 'Dart';
```

- `var` 키워드를 사용하면 Dart가 자동으로 변수의 타입을 추론
- 위 코드에서 `name`변수는 문자열(String)로 인식됨
- 명시적으로 변수를 선언하지 않는 방법은 많은 버그와 오류를 일으키는 원인이 된다.

## 명시적 타입 지정

- 변수의 타입을 명확하게 지정할 수 있음
- 타입을 한번 지정하는 것으로 다른 값이 할당이 쉽지 않게 하여 오류가 쉽게 발생할 수 없게 방지 할 수 있다.

| 자료형(Type) | 설명 | 예제 코드 |
| --- | --- | --- |
| `String` | 문자열 데이터를 저장 | `String name = 'Dart';` |
| `int` | 정수 값을 저장 | `int age = 30;` |
| `double` | 실수(소수점 포함)값을 저장 | `double pi = 3.14;` |
| `bool` | true 또는 false 값을 저장 | `bool isDartFun = true;` |
| `List<T>` | 특정 타입(T)의 리스트 | `List<int> numbers = [1, 2, 3];` |
| `Map<K,V>` | 키(K)와 값(V) 쌍을 저장 | `Map<String, int> scores = {'Alice': 90, 'Bob': 85};` |
| `dynamic` | 타입이 실행 중 변경될 수 있음(쓰지 말기) | `dynamic anything = 'Hello'; anything = 123;` |

# final과 const

## final과 const

- 변수의 값이 변경되지 않도록 하려면 `final` 또는  `const`를 사용할 수 있다.
- 상수를 선언 하기 위해 쓴다

### final 키워드

```dart
final city = 'Seoul';
```

- `final` 변수는 한 번만 설정 할 수 있으며, 이후 변경 할 수 없음
- 런타임에 결정되어서 런타임이 시작 되기 전 값을 배정 할 수 있음

### const 키워드

```dart
const pi = 3.14;
```

- `const` 변수는 컴파일 타임에 상수로 설정이 됨
- 때문에 `const DateTime now = DateTime.now();` 이와 같은 코드는 컴파일 타임에 값이 결정 될 수 없기에 오류가 발생 한다.

# Nuallable과 Non-nullable 변수

- Dart에서는 null-safety를 지원하므로 변수는 기본적으로 null 값을 가질 수 없다.
- 만약 null 값을 허용하려면 `?` 을 추가 해야 한다
    
    ```dart
    String name = null; // 오류 발생
    
    String? name;
    name = null; // 허용됨
    ```
    

# Late 변수 (지연 초기화)

```dart
String description; // 오류 발생 (초기값 없음)

String? description;  // nullable로 변경 (null 가능)

String description = 'Dart is awesome!';  // 즉시 초기화
```

- 기본적으로 Dart의 **non-nullable** 변수는 반드시 선언과 동시에 초기값을 할당해야 함
- 하지만 **nullable변수(`?` 사용)** 하거나 **즉시 초기화** 하는 법으로 이 문제를 해결할 수 있음
- 하지만 변수를 선언 할때 즉시 값을 할당할 수 없는 경우도 있기에 이럴 때 **`late` 키워드를 사용**하면 변수를 **나중에 초기화할 수 있으면서도 null-safety를 유지**할 수 있다.

## late 키워드 사용 방법

```dart
late String description;

void main() {
  description = 'Dart is awesome!';
  print(description); // 정상 출력: Dart is awesome!
}
```

- late 키워드를 사용하면 초기화 없이 변수를 선언할 수 있지만, 사용하기 전에 반드시 값을 할당해야 한다.
- late 변수는 실제로 처음 사용될 때 값을 할당 받게 된다.

## late를 사용하는 이유

1. **값이 반드시 필요하지만, 초기화 시점을 늦춰야 하는 경우**
    
    ```dart
    class User {
      late String name;
    
      void setName(String newName) {
        name = newName;
      }
    
      void printName() {
        print(name);
      }
    }
    
    void main() {
      User user = User();
      user.setName('Alice'); // 나중에 할당
      user.printName(); // 출력: Alice
    }
    
    ```
    
2. **리소스를 나중에 로드해야 할 때 (예: API 요청, 데이터베이스 연결 등)**
    
    ```dart
    class DataFetcher {
      late String data;
    
      void fetchData() {
        // 가정: 서버에서 데이터를 가져옴
        data = '서버에서 받은 데이터';
      }
    
      void printData() {
        print(data);
      }
    }
    
    void main() {
      DataFetcher fetcher = DataFetcher();
      fetcher.fetchData(); // 데이터 할당
      fetcher.printData(); // 정상 출력: 서버에서 받은 데이터
    }
    ```
    

## late 변수의 주의점

1. 값을 할당하지 않고 사용하면 런타임 오류 발생
    
    ```dart
    late String title;
    
    void main() {
      print(title); // 오류 발생: LateInitializationError
    }
    ```
    
    - `late`변수는 선언만 했다고 해서 기본적으로 `null`이 되는 것이 아니다.
    - **값이 할당되기 전에 사용하면 `LateInitializationError`가 발생 한다**.
2. late 변수는 값이 한 번만 할당되는 것이 아님
    
    ```dart
    late String status;
    
    void main() {
      status = '준비 중';
      print(status); // 출력: 준비 중
    
      status = '완료';
      print(status); // 출력: 완료
    }
    ```
    
    - `late`변수는 `final`이나 `const`처럼 불변 값이 아니므로, **필요할 때 변경할 수 있다**
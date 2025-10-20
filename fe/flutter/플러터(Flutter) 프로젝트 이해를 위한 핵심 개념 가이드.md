## 플러터(Flutter) 프로젝트 이해를 위한 핵심 개념 가이드

이 문서는 플러터 프로젝트를 진행하며 코드를 더 깊이 있게 이해하는 데 필요한 다트(Dart)와 플러터(Flutter)의 핵심 개념을 정리한 문서입니다.

-----

### 1부: 다트(Dart) - 플러터의 프로그래밍 언어

플러터는 UI 프레임워크이며, 다트는 이 프레임워크를 작동시키는 프로그래밍 언어입니다. 다음은 다트의 핵심 문법입니다.

#### 1\. 변수, 타입, 그리고 Null 안정성

다트는 타입-안전(Type-safe) 언어로, 모든 변수는 명확한 타입을 가집니다. 특히 'Null 안정성(Null Safety)'은 다트의 중요한 특징입니다.

  * `String productId;`: `productId` 변수는 문자열(`String`) 타입만 가질 수 있습니다.
  * `Map<String, dynamic>? _product;`:
      * **`Map<String, dynamic>`**: Key는 `String` 타입이고, Value는 `dynamic` (모든 타입 가능) 타입인 데이터 \<em\>(JSON 객체와 유사)\</em\>입니다.
      * **`?` (Nullable)**: 이 변수가 `Map` 데이터를 가지거나, 혹은 **`null` (값이 없음)** 상태일 수 있음을 명시합니다. API 응답을 받기 전까지 데이터가 `null`일 수 있기 때문에 필수적입니다.

#### 2\. 널(Null) 인식 연산자 (`?.` / `??`)

변수가 `null`일 수 있다고(`?`) 선언했다면, 해당 변수를 안전하게 사용하기 위한 연산자가 필요합니다. 이는 `NullPointerException` (앱 충돌)을 방지합니다.

  * **`?.` (Safe Navigation Operator)**: "만약 객체가 `null`이 아니면 뒤의 속성에 접근하고, `null`이면 전체 표현식을 `null`로 반환하라."

      * **예시**: `_seller?['name']`
      * **의미**: `_seller`가 `null`이 아닐 때만 `['name']` 속성에 접근합니다. `_seller`가 `null`이면 오류 대신 `null`을 반환합니다.

  * **`??` (Null Coalescing Operator)**: "만약 왼쪽의 표현식이 `null`이면, 오른쪽의 기본값을 사용하라."

      * **예시**: `_seller?['name']?.toString() ?? ''`
      * **의미**: `_seller`의 `name`을 `toString()`으로 변환하되, 그 과정 중 어느 하나라도 `null`이어서 최종 결과가 `null`이 되면, 대신 빈 문자열(`''`)을 사용합니다.

#### 3\. 비동기 프로그래밍 (`Future`, `async`, `await`)

API 통신이나 파일 읽기처럼 시간이 걸리는 작업을 처리할 때 사용합니다. 앱이 멈추지 않고(Non-blocking) 다른 작업을 계속할 수 있게 해줍니다.

  * **`Future`**: '미래에 완료될 작업'을 의미하는 객체입니다. 작업이 완료되면 '값' 또는 '오류'를 반환합니다. `ApiService.getProduct()`는 `Future<Map<String, dynamic>?>` (미래에 `Map` 또는 `null`을 반환) 타입을 반환할 것입니다.
  * **`async`**: 함수 선언부에 붙어, "이 함수는 비동기 작업을 포함하며, `await` 키워드를 사용할 수 있다"고 표시합니다.
  * **`await`**: `Future` 작업이 완료될 때까지 **기다리게** 만듭니다. `await`를 사용하면 비동기 코드를 동기식 코드처럼 순차적으로 작성할 수 있습니다.
      * **예시**: `final product = await ApiService.getProduct(widget.productId);`
      * **의미**: `ApiService.getProduct`가 완료되어 `product`에 실제 데이터가 담길 때까지 코드의 다음 줄 실행을 '일시 중단'합니다. (이때 앱 UI는 멈추지 않습니다.)

-----

### 2부: 플러터(Flutter) - UI 프레임워크

플러터의 핵심 철학은 \*\*"모든 것은 위젯(Widget)이다"\*\*입니다. UI 요소(`Text`, `Icon`), 배치(`Column`, `Row`), 스타일(`Padding`, `Container`) 모두 위젯의 조합으로 구성됩니다.

#### 1\. `StatelessWidget` vs. `StatefulWidget`

플러터 위젯은 크게 두 가지로 나뉩니다.

  * **`StatelessWidget` (상태가 없는 위젯)**

      * 내부 상태에 의존하지 않는 정적인 위젯입니다.
      * 한번 그려진 후에는 스스로 변하지 않습니다. (예: `AppBottomNav`)
      * 오직 부모 위젯에서 전달받은 값(예: `final String productId`)에 의해서만 모습이 결정됩니다.

  * **`StatefulWidget` (상태가 있는 위젯)**

      * **`DetailPage`가 여기에 해당합니다.**
      * 위젯의 생명주기 동안 \*\*변경될 수 있는 내부 데이터(`State`)\*\*를 가집니다.
      * `_isLoading`, `_isFavorite`, `_product`처럼 데이터가 변경되면 UI를 스스로 다시 그릴 수 있습니다.
      * `StatefulWidget`은 `Widget` 자체와 `State` 객체, 두 개의 클래스로 구성됩니다.

#### 2\. `State`와 `setState()` (StatefulWidget의 핵심)

`State` 객체(`_DetailPageState`)는 `StatefulWidget`의 모든 상태 변수와 로직을 관리합니다.

  * **`State` 변수**: `_isFavorite`, `_isLoading` 등 `_DetailPageState` 클래스 내에 선언된 변수들입니다.

  * **`setState()`**: 플러터 프레임워크에 "**내부 상태가 변경되었으니 UI를 다시 그려라\!**"고 알리는 유일한 방법입니다.

      * **중요**: 단순히 `_isFavorite = true;`처럼 변수 값만 바꾼다고 해서 화면은 절대 새로고침되지 않습니다.
      * **올바른 사용법**:
        ```dart
        // 찜 버튼 클릭 시
        setState(() {
          _isFavorite = !_isFavorite;
        });
        ```
      * `setState()` 콜백 함수 내에서 상태 변수를 변경하면, 플러터는 해당 위젯의 `build()` 메소드를 다시 호출하여 변경된 상태를 UI에 반영합니다.

#### 3\. `build()` 메소드 (UI 설계도)

`build()` 메소드는 현재 `State`를 기반으로 위젯 트리를 구성(설계)하는 역할을 합니다.

  * **호출 시점**:

    1.  위젯이 처음 생성될 때
    2.  `setState()`가 호출될 때
    3.  부모 위젯이 재빌드될 때 등

  * `build()` 메소드는 매우 자주 호출될 수 있으므로, 항상 현재 상태(`_isLoading`, `_product`)를 기반으로 UI를 반환하도록 작성해야 합니다.

      * **예시**:
        ```dart
        @override
        Widget build(BuildContext context) {
          // 1. 현재 State(_isLoading)에 따라 로딩 화면 반환
          if (_isLoading) {
            return Scaffold(body: Center(child: CircularProgressIndicator()));
          }

          // 2. 현재 State(_product)에 따라 에러 화면 반환
          if (_product == null) {
            return Scaffold(body: Center(child: Text('상품을 찾을 수 없습니다')));
          }

          // 3. 모든 State가 준비되면 실제 상품 화면 반환
          return Scaffold(body: SingleChildScrollView(...));
        }
        ```

#### 4\. `initState()` (위젯 생명주기)

`StatefulWidget`은 '생명주기(Lifecycle)'를 가지며, `initState()`는 그중 가장 중요합니다.

  * **`initState()`**: `State` 객체가 생성된 후 **단 한 번만 호출**됩니다.

  * `build()` 메소드가 실행되기 전에 호출됩니다.

  * 주로 API 호출, 리스너 등록 등 위젯이 생성될 때 딱 한 번만 수행해야 하는 초기화 작업을 이곳에서 수행합니다.

      * **예시**:
        ```dart
        @override
        void initState() {
          super.initState();
          _loadProductData(); // 화면 진입 시 데이터 로딩 시작
        }
        ```

-----

### 3부: `DetailPage` 코드 실행 흐름 요약

1.  **(진입)** 사용자가 `DetailPage`로 진입합니다.
2.  **(생성)** `_DetailPageState` 객체가 생성됩니다. (이때 `_isLoading = true`)
3.  **(초기화)** `initState()`가 **단 한 번** 호출되고, `_loadProductData()`를 실행시킵니다.
4.  **(첫 빌드)** `build()` 메소드가 처음 호출됩니다. `_isLoading`이 `true`이므로, 화면에 **로딩 스피너**가 나타납니다.
5.  **(데이터 로딩)** `_loadProductData` 함수는 `await ApiService.getProduct()`를 만나 API 응답을 \*\*'대기'\*\*합니다. (UI는 멈추지 않고 로딩 스피너를 계속 표시합니다.)
6.  **(데이터 도착)** API 응답이 도착하고 `product`, `initialFavorite` 변수에 값이 할당됩니다.
7.  **(상태 변경)** `setState()`가 호출되면서 `_isLoading = false`, `_product = product` 등으로 **상태 변수가 갱신**됩니다.
8.  **(재(Re)-빌드)** `setState()`가 호출되었으므로, `build()` 메소드가 **다시 실행**됩니다.
      * `if (_isLoading)` 조건은 `false` (통과).
      * `if (_product == null)` 조건도 `false` (통과).
      * `Scaffold(body: SingleChildScrollView(...))`가 반환되어 **실제 상품 정보 UI**가 화면에 그려집니다.
9.  **(상호작용)** 사용자가 찜(Like) 버튼을 탭합니다.
10. **(상태 변경 2)** `onTap` 내부의 `setState(() { _isFavorite = ...; })`가 호출됩니다.
11. **(재(Re)-빌드 2)** `build()` 메소드가 **다시 실행**되어, `_isFavorite` 값에 따라 꽉 찬 하트 아이콘(❤️) 또는 빈 하트 아이콘(🤍)으로 UI가 갱신됩니다.
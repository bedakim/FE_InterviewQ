# LIST

### `.forEach` 와 `.map()` 루프 사이의 주요 차이점

이 두 함수의 차이점을 이해하기 위해 각 함수가 무엇을 하는지 살펴보겠습니다.

**`forEach`**

- 배열의 요소를 반복합니다.
- 각 요소에 대한 콜백을 실행합니다.
- 값을 반환하지 않습니다.

```js
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
  // num 및 / 또는 index로 무엇이든 해보세요.
});

// doubled = undefined
```

**`map`**

- 배열의 요소를 반복합니다.
- 각 요소에서 함수를 호출하여 결과로 새 배열을 작성하여 각 요소를 새 요소에 매핑합니다.

```js
const a = [1, 2, 3];
const doubled = a.map(num => {
  return num * 3
});

// doubled = [3, 6, 9]
```

`.forEach` 와 `.map()` 의 주된 차이점은 `.map()` 이 새로운 배열을 반환한다는 것입니다. 결과가 필요하지만 원본 배열을 변경하고 싶지 않으면 `.map()` 이 확실한 선택입니다. 단순히 배열을 반복할 필요가 있다면, forEach 가 좋은 선택입니다.

### `null`, `undefined`, `선언되지 않은 변수` 의 차이점은 무엇입니까? 당신은 어떻게 이 상태들에 대한 점검을 할 것입니까?

**선언되지 않은 변수** 변수는 이전에 `var`, `let`, `const` 를 사용하여 생성되지 않은 식별자에 값을 할당할 때 생성됩니다. `선언되지 않은 변수` 는 현재 범위 외부에서 전역으로 정의됩니다. strict 모드에서는 `선언되지 않은 변수` 에 할당하려고 할 때 `ReferenceError` 가 throw 됩니다. `선언되지 않은 변수` 는 전역 변수처럼 좋지 않은 것입니다. 그것들은 모두 피하세요! 이들을 검사하기 위해 사용할 때 `try` / `catch` 블록에 감싸십시오.

```js
function foo() {
  x = 1; // strict 모드에서 ReferenceError를 발생시킵니다.
}

foo();
console.log(x); // 1
```

`undefined` 변수는 선언되었지만 값이 할당되지 않은 변수입니다. 이것은 `undefined` 타입입니다. 함수가 실행 결과에 따라 값을 반환하지 않으면 변수에 할당되며, 변수가 `undefined` 값을 갖습니다. 이것을 검사하기 위해, 엄격한 (`===`) 연산자 또는 `typeof` 에 `undefined` 문자열을 사용하여 비교하십시오. 확인을 위해 추상 평등 연산자(`==`)를 사용해서는 안되며, 이는 값이 `null` 이면 `true` 를 반환합니다.

```js
var foo;
console.log(foo); // undefined
console.log(foo === undefined); // true
console.log(typeof foo === "undefined"); // true

console.log(foo == null); // true. 옳지않습니다. 확인하는 데 사용하지 마세요.

function bar() {}
var baz = bar();
console.log(baz); // undefined
```

`null` 인 변수는 `null` 값에 명시적으로 할당될 것입니다. 그것은 값을 나타내지 않으며 명시적으로 할당된다는 점에서 `undefined`와 다릅니다. `null`을 체크하기 위해서 단순히 완전 항등 연산자(`===`)를 사용하여 비교하면 됩니다. 위와 같이, 추상 평등 연산자 (`==`)를 사용해서는 안되며, 값이 `undefined`이면 `true`를 반환합니다.

```js
var foo = null;
console.log(foo === null); // true

console.log(foo == undefined); // true. 옳지않습니다. 확인하는 데 사용하지 마세요.
```

### `==`와 `===`의 차이점은 무엇입니까?

`==`는 추상 동등 연산자이고 `===`는 완전 동등 연산자입니다. `==`연산자는 타입 변환이 필요한 경우 타입 변환을 한 후에 동등한지 비교할 것입니다. `===`연산자는 타입 변환을 하지 않으므로 두 값이 같은 타입이 아닌 경우 `===`는 단순히 `false`를 반환합니다. `==`를 사용하면 다음과 같은 무서운 일이 발생할 수 있습니다.

```js
1 == "1"; // true
1 == [1]; // true
1 == true; // true
0 == ""; // true
0 == "0"; // true
0 == false; // true
```

`null`과 `undefined`를 비교할 때를 제외하고, `==`연산자를 절대 사용하지 않는 것입니다. `a == null`은 `a`가 `null` 또는 `undefined`이면 `true`를 반환합니다.

```js
var a = null;
console.log(a == null); // true
console.log(a == undefined); // true
```

### 내장 JavaScript 객체를 확장하는 것이 좋은 생각이 아닌 이유는 무엇입니까?

빌트인/네이티브 JavaScript 객체를 확장한다는 것은 prototype에 속성/함수를 추가한다는 것을 의미합니다. 이것은 처음에는 좋은 생각처럼 보일 수 있지만 실제로는 위험합니다. 여러분의 코드가 동일한 `contains` 메소드를 추가함으로써 `Array.prototype`을 확장하는 여러가지 라이브러리를 사용한다고 상상해보십시오. 이러한 구현은 메소드를 서로 덮어쓰게 되며 이 두 메소드의 동작이 동일하지 않으면 코드가 망가질 것입니다.

네이티브 객체를 확장할 수 있는 유일한 경우는 polyfill을 만들려고 할 때입니다. JavaScript 사양의 일부이지만 오래된 브라우저이기 때문에 사용자 브라우저에 없을 수도 있는 메서드에 대한 고유한 구현을 제공해야 할 경우입니다.

### single page app이 무엇인지 설명하고 SEO-friendly한 앱을 만드는 방법을 설명하십시오.

아래는 [Grab Front End Guide](https://github.com/grab/front-end-guide)에서 가져온 것이며, 우연히도 저를 위해 쓰였습니다!

웹 개발자는 요즘 웹 사이트가 아닌 웹 앱으로 제작한 제품을 언급합니다. 두 가지 용어 사이에는 엄격한 차이는 없지만 웹 앱은 대화형 및 동적인 경향이 있어 사용자가 작업을 수행하고 작업에 대한 응답을 받을 수 있습니다. 전통적으로, 브라우저는 서버에서 HTML을 받아 렌더링합니다. 사용자가 다른 URL로 이동하면 전체페이지 새로고침이 필요하며 서버는 새페이지에 대해 새 HTML을 보냅니다. 이를 server-side rendering이라고합니다.

그러나 현대 SPA에서는 대신 클라이언트 측 렌더링이 사용됩니다. 브라우저는 전체 애플리케이션에 필요한 스크립트(프레임워크, 라이브러리, 앱 코드) 및 스타일시트와 함께 서버의 초기 페이지를 로드합니다. 사용자가 다른 페이지로 이동하면 페이지 새로고침이 발생하지 않습니다. 페이지의 URL은 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)를 통해 업데이트됩니다. 일반적으로 JSON 형식의 새 페이지에 필요한 새 데이터는 브라우저에서 [AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) 요청을 통해 서버로 전송됩니다. SPA는 초기 페이지 로딩에서 미리 다운로드된 JavaScript를 통해 페이지를 동적으로 업데이트합니다. 이 모델은 네이티브 모바일 앱의 작동 방식과 유사합니다.

장점들:

- 전체 페이지 새로고침으로 인해 페이지 탐색 사이에 하얀 화면이 보이지 않아 앱이 더 반응적으로 느껴지게 됩니다.

- 동일한 애셋을 페이지 로드마다 다시 다운로드할 필요가 없으므로 서버에 대한 HTTP 요청이 줄어듭니다.

- 클라이언트와 서버 사이의 고려해야 할 부분을 명확하게 구분합니다. 서버 코드를 수정하지 않고도 다양한 플랫폼(예: 모바일, 채팅 봇, 스마트워치)에 맞는 새로운 클라이언트를 쉽게 구축할 수 있습니다. 또한 API 계약이 깨지지 않는 한 내에서 클라이언트와 서버에서 기술 스택을 독립적으로 수정할 수 있습니다.

단점들:

- 여러 페이지에 필요한 프레임워크, 앱 코드, 애셋로드로 인해 초기 페이지로드가 무거워집니다.

- 모든 요청을 단일 진입점으로 라우트하고 클라이언트 측 라우팅이 그 한곳에서 인계받을 수 있도록 서버를 구성하는 추가 단계가 수행되어야합니다.

- SPA는 콘텐츠를 렌더링하기 위해 JavaScript에 의존하지만 모든 검색 엔진이 크롤링 중에 JavaScript를 실행하지는 않으며 페이지에 빈 콘텐츠가 표시될 수 있습니다. 이로 인해 의도치 않게 앱의 검색 엔진 최적화 (SEO)가 어려워집니다.

그러나 대부분의 경우 앱을 제작할 때 검색 엔진에서 모든 콘텐츠 색인할 필요는 없으므로 SEO가 가장 중요한 요소는 아닙니다. 이를 극복하기 위해, 앱을 서버 측 렌더링하거나 [Prerender](https://prerender.io/)와 같은 서비스를 사용하여 "브라우저에서 JavaScript를 렌더링하고, 정적 HTML을 저장한 다음, 크롤러에게 반환합니다".

### 동기 및 비동기 함수의 차이점을 설명하십시오.

동기 함수는 차단되는 반면 비동기 함수는 차단되지 않습니다. 동기 함수에서는 다음 명령문이 실행되기 전에 명령문이 완료됩니다. 이 경우 프로그램은 명령문의 순서대로 정확하게 평가되고 명령문 중 하나가 매우 오랜 시간이 걸리면 프로그램 실행이 일시 중지됩니다.

비동기 함수는 일반적으로 파라미터를 통해서 콜백을 받아들이고 비동기 기능은 일반적으로 매개 변수로 콜백을 허용하며 비동기 기능이 호출된 후 즉시 다음 줄에서 실행이 계속됩니다. 콜백은 비동기 작업이 완료되고 호출 스택이 비어 있을 때만 호출됩니다. 웹 서버에서 데이터를 로드하거나 데이터베이스를 쿼리 하는 등의 엄격한 작업을 비동기식으로 수행하여 주 스레드가 긴 작업을 완료할 때까지 차단하지 않고 다른 작업을 계속해야 합니다(브라우저의 경우 UI가 중지됨).

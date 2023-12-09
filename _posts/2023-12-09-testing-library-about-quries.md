---
title: About Queries
author: sam
date: 2022-12-01 16:32:00 -0500
categories: [Testing Library, Core API, Queries]
tags: [Testing Library]
imgp: /assets/img/git/
---

# About Queries

## Overview

쿼리는 Testing Library에서 페이지에서 요소를 찾을 수 있도록 제공하는 메서드다. 쿼리에는 여러 타입("get", "find", "query")이 있다. 요소를 찾을 수 없는 경우 쿼리가 오류를 던질지, 아니면 Promise를 반환하고 다시 시도할지가 이들 간의 차이점이다. 어떤 타입의 쿼리가 적절할지는 페이지 컨텐츠에 따라 다르다.  이에 대한 자세한 내용은 아래 Priority 섹션에서 다룬다.

요소를 선택한 후 이벤트 API 또는 사용자 이벤트를 사용하여 이벤트를 실행하고 페이지와 사용자 상호 작용을 시뮬레이션하거나 Jest 및 jest-dom을 사용하여 요소에 대한 assertion(조건 검증)을 수행할 수 있다.

쿼리와 함께 작동하는 Testing Library helper 메서드가 있다. 요소가 액션의 결과로 나타났다 사라지는 경우, `waitFor` 또는 `findBy` 쿼리와 같은 비동기 API를 사용하여 DOM의 변경 사항을 대기할 수 있다. 특정 요소의 자식 요소만 찾는 경우엔 `within` 을 사용할 수 있다. 필요에 따라 재시도를 트리거 하는 시간제한 및 기본 testID 속성과 같은 몇 가지 옵션을 설정할 수도 있다.

## Example

```javascript
import {render, screen} from '@testing-library/react' // (or /dom, /vue, ...)

test('should show login form', () => {
  render(<Login />)
  const input = screen.getByLabelText('Username')
  // Events and assertions...
})
```

## Types of Queries

- Single Elements
    - `getBy…`:  쿼리에 매칭된 노드를 반환하고 일치하는 요소가 없거나 둘 이상 일치하는 요소가 발견되면 이를 설명하는 오류를 던진다(두 개 이상의 요소가 필요한 경우엔 `getAllBy` 사용).
    - `queryBy...`: 쿼리에 매칭된 노드를 반환하고 일치하는 요소가 없으면 `null` 을 반환한다. 이는 존재하지 않는 요소를 assertion하는 데 유용하다. 일치하는 요소가 두 개 이상 발견되면 오류를 던진다(매칭되는 요소가 두 개 이상이어도 괜찮다면 `queryAllBy` 사용).
    - `findBy...`: 지정된 쿼리와 일치하는 요소가 발견되면 resolve되는 Promise를 반환한다. 요소가 발견되지 않거나 1000ms인 기본 시간 제한 이후에 발견된 요소가 2개 이상이면 Promise를 reject한다. 두 개 이상의 요소를 찾아야 하는 경우 `findAllBy` 를 사용하자.
- Multiple Elements
    - `getAllBy...`: 쿼리에 매칭된 모든 노드가 담긴 배열을 반환하고 일치하는 요소가 없으면 오류를 던진다.
    - `queryAllBy...`: 쿼리에 매칭된 모든 노드가 담긴 배열을 반환하고 일치하는 요소가 없으면 빈 배열(`[]`)을 반환한다.
    - `findAllBy...`: 지정된 쿼리와 일치하는 요소가 발견되면 요소가 담긴 배열로 resolve되는 Promise를 반환한다. 기본 제한 시간인 1000ms 후에 요소가 발견되지 않으면 Promise를 reject한다.
        - `findBy` 메서드는 `getBy*` 쿼리와 `waitFor` 의 조합이다. 이 메서드는 `waitFor` 옵션을 마지막 인수로 받는다 (예:  `await screen.findByText('text', queryOptions, waitForOptions)`).
- Summary Table
    
    
    | 쿼리 타입 | 0 매치 | 1 매치 | >1 먀차 | 재시도 (Async/Await) |
    | --- | --- | --- | --- | --- |
    | Single Element |  |  |  |  |
    | getBy... | Throw error | Return element | Throw error | No |
    | queryBy... | Return null | Return element | Throw error | No |
    | findBy... | Throw error | Return element | Throw error | Yes |
    | Multiple Elements |  |  |  |  |
    | getAllBy... | Throw error | Return array | Return array | No |
    | queryAllBy... | Return [] | Return array | Return array | No |
    | findAllBy... | Throw error | Return array | Return array | Yes |

## Priority

테스트는 최대한 유저가 코드와 상호작용 하는 방법과 닮아야 한다는 원칙 아래 다음 순서대로 사용 우선순위를 둘 것을 권장한다.

1. **모든 사용자 사용자가 액세스할 수 있는 쿼리** :  비주얼/마우스 사용자 및 보조 기술을 사용하는 유저의 경험을 반영하는 쿼리
    1. `getByRole` : 접근성 트리에 노출된 모든 요소를 쿼리하는 데 사용할 수 있다. `name` 옵션을 사용하여 반환되는 요소를 액세스 가능한 이름으로 필터링할 수 있다. 아마 이것이 거의 경우에 가장 선호하는 방식이 될 것이다. 이 메서드로 얻을 수 없는건 많지 않다. 이 메서드가 name 옵션과 함께 자주 사용되는 방식은 다음과 같다 : `getByRole('button', {name: /submit/i})`
    2. `getByLabelText` : 이 방법은 폼 필드에 매우 적합하다. 웹 사이트 폼을 탐색할 때 유저는 레이블 텍스트를 사용하여 요소를 찾는다. 이 메서드는 해당 동작을 모방한다. 역시 가장 자주 사용될 메서드 중 하나다.
    3. `getByPlaceholderText` : placeholder는 레이블의 대체제는 아니지만, 레이블이 없다면 사용할 수 있는 가장 나은 대안이다.
    4. `getByText` : 폼을 제외하면 텍스트 컨텐츠는 유저가 요소를 찾는 가장 주된 방법이다. 이 메서드는 div, span, p 와 같은 비상호작용적 요소를 찾는데 사용된다.
    5. `getByDisplayValue` : 폼 요소의 현재 값은 채워진 값으로 페이지를 탐색할 때 유용하다.
2. **시멘틱 쿼리** : HTML과 ARIA 프로토콜을 준수하는 셀렉터. 이러한 속성과 상호작용하는 유저 경험은 브러우저와 보조 기술에 따라 크게 다르다는 것을 유념하자.
    1. `getByAltText` : 요소가 `alt` 속성을 지원한다면 이 메서드를 사용해서 해당 요소를 찾을 수 있다.
    2. `getByTitle` : `title` 속성은 스크린리더(시각장애인이 사용하는 보조 기구)로 일관성 있게 읽히지 않으며 기본적으로 눈에 보이지 않는다.
3. **Test IDs**
    1. `getByTestId` : TestId는 정상인이 시각으로 확인할 수도, 시각 장애인의 보조 기구가 읽을 수도 없다. 따라서 이 메서드는 role, text 로 매치할 수 없거나 동적 텍스트 처럼 그 의미를 미리 알 수 없는 경우에만 사용한다.

## Using Queries

DOM Testing Library의 기본 쿼리는 첫 번째 인수로 컨테이너를 전달해야 한다. Testing Library의 프레임워크 대응 구현은 대부분이 컴포넌트를 렌더링할 때 이러한 쿼리의 사전 바인딩(pre-bound) 버전을 제공하므로 컨테이너를 제공할 필요가 없다. 또한 `document.body`를 쿼리하고 싶다면 아래 설명할 내용과 같이 `screen` `export`를 사용할 수 있다(`screen` 사용을 권장한다).

쿼리의 기본 인수는 문자열, 정규 표현식 또는 함수다. 노드 텍스트의 파싱 방식을 조정하는 옵션도 있다. 

다음과 같은 (React, Vue, Angular 또는 기본 HTML 코드로 렌더링 가능한) DOM 요소가 있다고 생각해보자.

```html
<body>
  <div id="app">
    <label for="username-input">Username</label>
    <input id="username-input" />
  </div>
</body>
```

요소를 찾기 위해 아래와 같이 쿼리를 사용할 수 있다(여기선 `byLabelText` 를 사용했다).

```javascript
import {screen, getByLabelText} from '@testing-library/dom'

// screen = document.body
const inputNode1 = screen.getByLabelText('Username')

// screen을 사용하는게 아니면, 컨테이너를 인수에 넘겨야 한다.
const container = document.querySelector('#app')
const inputNode2 = getByLabelText(container, 'Username')
```

### `queryOptions`

`queryOptions` 객체를 쿼리 타입과 함께 전달할 수 있다.

### `screen`

DOM Testing Library에서 export된 모든 쿼리는 컨테이너를 첫 번째 인수로 받는다. 전체 `document.body`를 조회하는 것은 매우 일반적이기 때문에, DOM Testing Library는 document.body에 미리 바인딩된 모든 쿼리를 내장한 `screen` 객체를 제공(export)한다. React Testing Library와 같은 래퍼는 `screen` re-export 하므로 동일한 방식으로 screen을 사용할 수 있다.

```javascript
import {render, screen} from '@testing-library/react'

render(
  <div>
    <label htmlFor="example">Example</label>
    <input id="example" />
  </div>,
)

const exampleInput = screen.getByLabelText('Example')
```

<aside>
📌 **Note**

`screen`을 사용하려면 글로벌 DOM 환경이 필요하다. `jest`를 사용하는 경우 `testEnvironment` 를 `jsdom`으로 설정하면 글로벌 DOM 환경을 사용할 수 있다.

`script` 태그로 테스트를 로드하는 경우엔 테스트가 반드시 `body` 다음에 와야 한다. 

</aside>

## `TextMatch`

대부분의 쿼리 API는 `TextMatch`를 인수로 사용한다. 해당 인수는 문자열, 정규 표현식 또는 매치되면 true 매치 안되면 false를 반환하는 `(content?: string, element?: Element | null) => boolean` 구조의 함수를 값으로 받는다.

### TextMatch Examples

다음과 같은 HTML 문서가 있다 생각해보자.

```html
<div>Hello World</div>
```

다음 방법으로 위 HTML에서 div를 찾을 수 있다.

```javascript
// 문자열 매칭:
screen.getByText('Hello World') // 전체 문자열 매칭
screen.getByText('llo Worl', {exact: false}) // 하위 문자열 매칭
screen.getByText('hello world', {exact: false}) // 대소문자 무시

// 정규표현식 매칭:
// 하위 문자열 매칭
screen.getByText(/World/)
// 하위 문자열 매칭, 대소문자 무시
screen.getByText(/world/i) 
// 전체 문자열 매칭, 대소문자 무시
screen.getByText(/^hello world$/i) 
// 하위 문자열 매칭, 대소문자 무시, "hello world" 또는 "hello orld" 검색
screen.getByText(/Hello W?oRlD/i) 

// 함수로 매칭:
screen.getByText((content, element) => content.startsWith('Hello'))
```

다음은 매칭되지 않는 경우에 대한 예시다.

```javascript
// 전체 문자열이 매칭되지 않는다.
screen.getByText('Goodbye World')

// 대소문자 구분 정규 표현식과 매칭되지 않음
screen.getByText(/hello world/)

// span을 찾는 함수이므로 매칭되지 않음
screen.getByText((content, element) => {
  return element.tagName.toLowerCase() === 'span' && content.startsWith('Hello')
})
```

### Precision

또한 TextMatch를 인수로 사용하는 쿼리는 문자열 일치의 정밀도에 영향을 미치는 옵션을 포함할 수 있는 객체를 마지막 인수로 받는다.

- `exact` : 기본값은 `true` 이고, 전체 문자열과 일치하며 대소문자를 구분합니다. `false` 인 경우, 하위 문자열과 일치하며 대소문자를 구분하지 않는다.
    - `exact`는 `regex` 또는 `function` 인수에 영향을 주지 않는다.
    - `exact`는 `*byRole` 쿼리의 `name` 옵션으로 지정된 접근 가능한 이름에는 영향을 미치지 않는다. 접근 가능한 이름에 퍼지 매칭(완전히 동일한가가 아니라 비슷한가의 여부를 판단)을 위해 regex를 사용할 수 있다.
    - 대부분의 경우 문자열 대신 regex를 사용하는 것이 퍼지 매칭를 더 잘 제어할 수 있으므로 regex 사용을 `{ exact: false }` 보다 우선해야 한다.
- `normalizer` : 정규화 동작을 재정의하는 선택 함수다.

### Normalization

DOM Testing Library는 DOM의 텍스트와 일치하는 로직을 실행하기 전에 해당 텍스트를 자동으로 정규화한다. 기본적으로 정규화는 텍스트 시작과 끝 공백 제거와 문자열 내에 인접한 여러 공백 문자를 단일 공백으로 병합하는 것으로 구성된다.

해당 정규화를 방지하거나 대체 정규화(예: 유니코드 제어 문자 제거)를 제공하려면 option 객체에서 `normalizer` 함수를 제공하면 된다. 이 함수는 문자열을 받아 해당 문자열의 정규화 버전을 반환해야 한다.

<aside>
📌 Note

`normalizer`의 값을 지정하면 내장 정규화가 대체되지만 `getDefaultNormalizer`를 호출하여 내장 기본 `normalizer`를 얻을 수 있다. 이를 활용해 기본 내장 normalizer를 조정하거나 직접 작성하는 커스텀 normalizer 내부에서 호출하여 기본 기능을 커스텀 normalizer에서 활용할 수 있다.

</aside>

`getDefaultNormalizer`는 동작을 선택할 수 있는 옵션 객체를 인수로 받는다.

- `trim` : 기본값은 `true`. 문자열 앞과 뒤 공백 제거.
- `collapseWhitespace` : 기본값은 `true`. 문자열 내에 인접한 여러 공백 문자를 단일 공백으로 병합

**Normalization Examples**
기본 정규화 작업중 문자열 앞뒤 공백 제거(trim) 없이 텍스트 매치 수행

```javascript
screen.getByText('text', {
  normalizer: getDefaultNormalizer({trim: false}),
})
```

아래는 기본 내장 정규화 작업중 일부를 유지하면서 유니코드 문자열 몇 가지를 제거하도록 정규화를 재정의하는 예시다.

```javascript
screen.getByText('text', {
  normalizer: str =>
    getDefaultNormalizer({trim: false})(str).replace(/[\u200E-\u200F]*/g, ''),
})
```

## **Manual Queries**

Testing Library에서 제공하는 쿼리 위에 일반 `querySelector`[](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) DOM API를 사용하여 요소를 쿼리할 수 있다. 사용자가 볼 수 없기 때문에 클래스 또는 id로 쿼리하기 위한 탈출 해치(escape hatch)로 이것을 사용하는 건 권장하지 않는다. 필요한 경우 testid를 사용하여 비의미적 쿼리를 대안으로 사용할 것임을 명확히 하고 HTML에서 안정적인 API를 확립하자.

```javascript
// @testing-library/react
const {container} = render(<MyComponent />)
const foo = container.querySelector('[data-foo="bar"]')
```

## **Browser extension[](https://testing-library.com/docs/queries/about/#browser-extension)**

[Testing Playground](https://chrome.google.com/webstore/detail/testing-playground/hejbmebodbijjdhflfknehhcgaklhano) 라는 크롬  확장 기능이 사용하면 요소를 선택할 수 있는 최적의 쿼리를 찾을 수 있도록 도와준다. 브라우저 개발자 도구에서 요소 계층을 검사하고 이를 선택하는 방법을 제안한다.

## **Playground**

이러한 쿼리에 더 익숙해지고 싶다면 [testing-playground.com](http://testing-playground.com/) 에서 사용해 볼 수 있다. Testing Playground는 자신의 html에 대해 다양한 쿼리를 실행하고 위에서 언급한 규칙과 일치하는 시각적 피드백을 얻을 수 있는 대화형 샌드박스다.
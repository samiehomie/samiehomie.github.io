---
title: ByRole
author: sam
date: 2022-12-02 16:32:00 -0500
categories: [Testing Library, Core API, Queries]
tags: [Testing Library]
imgp: /assets/img/git/
---

# ByRole

> getByRole, queryByRole, getAllByRole, queryAllByRole, findByRole, findAllByRole
> 

## API

```javascript
getByRole(
  // screen을 사용하면 container 인수는 건너뛴다.
  container: HTMLElement,
  role: string,
  options?: {
    hidden?: boolean = false,
    name?: TextMatch,
    description?: TextMatch,
    selected?: boolean,
    busy?: boolean,
    checked?: boolean,
    pressed?: boolean,
    suggest?: boolean,
    current?: boolean | string,
    expanded?: boolean,
    queryFallbacks?: boolean,
    level?: number,
    value?: {
      min?: number,
      max?: number,
      now?: number,
      text?: TextMatch,
    }
  }): HTMLElement
```

부여한 역할에 매칭된 요소에 대한 쿼리다(또한 `TextMatch`도 허용한다). 역할이란 기본적으로 ‘기본 역할’을 의미한다. 예를 들어 `<button />`은 역할 속성을 명시적으로 설정하는 것 없이 button 역할을 기본적으로 갖는다. [여기](https://www.w3.org/TR/html-aria/#docconformance)에서 기본 및 원하는 역할이 있는 HTML 요소 표를 볼 수 있다.

암묵적 ARIA 시맨틱스와 매칭되는 `role` 또는 `aria-*` 속성을 설정하는 것은 불필요하며 권장하지 않는다. 이러한 속성은 브라우저에서 이미 설정되어 있다. 더불어 요소가 가지는 의미와 충돌하는  `role` 및 `aria-*` 속성을 부여해서도 안된다. 예를 들어, button 요소는 heading 역할과 충돌하는 기본 특성을 가지고 있기 때문에 heading의 role 속성을 가질 수 없다.


> 📌 ARIA란?
> 
> ARIA(Accessible Rich Internet Applications)는 웹 콘텐츠와 웹 애플리케이션(특히 JavaScript를 사용하여 개발된 것)이 장애를 가진 사람들에게 더 접근 가능하도록 만드는 방법을 정의하는 W3C(Web Content Accessibility Guidelines) 사양이다.
> 
> ARIA는 웹 페이지의 요소가 어떤 역할을 하는지, 그 상태는 무엇인지, 그리고 그것들이 서로 어떤 관계를 가지고 있는지를 보조 기술에게 알려주는 역할을 한다. 이는 스크린 리더와 같은 보조 기술을 사용하는 사용자가 웹 콘텐츠를 더 잘 이해하고 사용할 수 있게 돕는다.

> role은 ARIA 역할 계층 구조를 상속과 무관하게 오로지 문자열 동등성에 의해 매치된다. 따라서 `checkbox` 와 같은 슈퍼클래스 role을 쿼리 한다고 `switch`와 같은 서브클래스 role을 가진 요소가 포함되지 않는다.
> 

그들의 액세스 가능한 이름 또는 설명으로 해당 요소(들)을 쿼리할 수 있다. 액세스 가능한 이름은 폼 요소의 레이블, 버튼의 텍스트 내용 또는 `aria-label`속성의 값과 같은 단순한 경우에 사용된다. 예를 들어, 렌더링된 컨텐츠에 동일한 역할을 하는 여러 요소가 있는 경우 특정 요소를 쿼리하는 데 사용할 수 있다. 자세한 내용은 TPGi의 ["액세스 가능한 이름이란?"](https://www.tpgi.com/what-is-an-accessible-name/)을 확인하자. 

단일 요소만 쿼리하는 경우엔 `getByText('The name')` 보다 `getByRole(expectedRole, { name: 'The name' })` 을 사용하는 것이 대개 더 좋다. 액세스 가능한 이름 쿼리는 `*ByAlt` 또는 `*ByTitle`과 같은 다른 쿼리를 대체하지 않는다. 액세스 가능한 이름이 이러한 속성의 값과 동일할 순 있지만 이러한 속성의 기능을 대체하지는 않는다. 예를 들어 `<img aria-label="fancy image" src="fancy.jpg" />`은 `getByRole('img', { name: 'fancy image' })` 의 결과로 반환된다. 그러나 fancy.jpg가 로드될 수 없는 경우엔 이미지는 자신의 설명을 표시할 수 없다.

## Option

### `hidden`

`hidden` 에 `true` 를 설정하면, 일반적으로 접근성 트리에서 제외되는 요소들도 쿼리 대상으로 고려된다. 기본 동작은 어떤 경우든 쿼리에서 고려되는 role="none" 및 role="presentation"을 제외하고  [링크](https://www.w3.org/TR/wai-aria-1.2/#tree_exclusion) 를 따른다.

```html
<body>
  <main aria-hidden="true">
    <button>Open dialog</button>
  </main>
  <div role="dialog">
    <button>Close dialog</button>
  </div>
</body>
```

위 예제어서 `getByRole('button')` 는 오직 “Close dialog” button을 반환한다. “Open dialog” button까지 반환하게 하려면 `getAllByRole('button', { hidden: true })` 을 사용해야 한다.

### `selected`

`selected` 옵션에 `true` 또는 `false` 를 설정해서 요소의 선택 상태를 통해 반환된 요소를 필터할 수 있다.

```html
<body>
  <div role="tablist">
    <button role="tab" aria-selected="true">Native</button>
    <button role="tab" aria-selected="false">React</button>
    <button role="tab" aria-selected="false">Cypress</button>
  </div>
</body>
```

`getByRole('tab', { selected: true })` 를 호출하여 “Native” 탭을 얻을 수 있다. 선택된 상태와 어떤 요소가 이 선택 상태를 가질 수 있는지 더 확인 하려면 [해당 페이지](https://www.w3.org/TR/wai-aria-1.2/#aria-selected)를 확인하자.

### **`busy`**

`busy` 옵션에 `true` 또는 `false` 를 설정해서 요소의 busy 상태를 통해 반환된 요소를 필터할 수 있다.

```html
<body>
  <section>
    <div role="alert" aria-busy="false">Login failed</div>
    <div role="alert" aria-busy="true">Error: Loading message...</div>
  </section>
</body>
```

`getByRole('alert', { busy: false })` 를 호출하여 "Login failed” alert를 얻을 수 있다.

### **`checked`**

`checked` 옵션에 `true` 또는 `false` 를 설정해서 요소의 checked 상태를 통해 반환된 요소를 필터할 수 있다.

> 📌 Note
> 
> Checkbox는 checked도 unchecked 아닌 이 두가지 사이의 상태인 불확정(indeterminate) 상태를 갖는다.
> 
> 이 상태는 주로 트리 구조에서 부모 노드가 자식 노드의 선택 상태를 나타낼 때 사용된다. 예를 들어, 부모 노드에 연결된 모든 자식 노드가 체크되지 않았거나 일부만 체크된 경우, 부모 노드의 checkbox는 'indeterminate' 상태를 나타낼 수 있다.
> 
> HTML에서는 'indeterminate' 상태를 직접 설정할 수 없으며, 이를 설정하려면 JavaScript를 사용해야 한다. 예를 들어, 다음과 같이 설정할 수 있다.
>
> ```javascript
> document.getElementById("myCheckbox").indeterminate = true;
> ```
>

### **`current`**

`current: boolean | string` 옵션을 사용해 current 상태로 반환된 요소를 필터할 수 있다. 유의할 것은 `aria-current` 속성의 기본값이 `false` 이므로, `current: false` 옵션으로는 어떤 `aria-current` 속성과도 매치되지 않는다(aria-current 속성이 없는 요소가 매칭될 수 있다).

```javascript
<body>
  <nav>
    <a href="current/page" aria-current="page">👍</a>
    <a href="another/page">👎</a>
  </nav>
</body>
```

`getByRole('link', { current: 'page' })` 는 "👍" link 를 반환하고, `getByRole('link', { current: false })`는 "👎” link를 반환한다.

### **`pressed`**

button은 pressed 상태를 갖을 수 있다. `pressed` 옵션에 `true` 또는 `false` 를 설정해서 요소의 pressed 상태를 통해 반환된 요소를 필터할 수 있다.

### **`suggest`**

`suggest` 옵션에 `false` 를 설정하여 특정 쿼리에 대한 [throw suggestions](https://testing-library.com/docs/dom-testing-library/api-configuration#throwsuggestions-experimental) 기능을 비활성화 할 수 있다.

### **`expanded`**

 `expanded` 옵션에 `true` 또는 `false` 를 설정해서 요소의 expanded 상태를 통해 반환된 요소를 필터할 수 있다.

### **`queryFallbacks`**

요소의 첫 번째 role이 아닌 폴백(fall back) role을 사용해 요소를 쿼리해야 하는 경우 `queryFallbacks: true` 옵션을 사용해야 한다. 예를 들어,  `<div role="switch checkbox" />` 는 첫 번째 role이 매칭되는 `getByRole('switch')`와 항상 매칭되는 반면,  `getByRole('checkbox')` 와는 매칭되지 않는다. 그러나 `getByRole('checkbox', { queryFallbacks: true })` 는 모든 폴백 role을 활성화하므로 `<div role="switch checkbox" />` 와 매칭된다.

> 하나의 요소는 주어진 환경에서 여러 역할을 가지지 않는다. 하나의 요소는 하나의 역할을 가진다. 환경은 처음 이해하는 역할을 찾을 때까지 속성에 부여된 여러 역할을 왼쪽에서 오른쪽으로 평가한다. 이는 새로운 도입된 역할을 지원하면서 함께 그 역할을 이해하지 못하는 오래된 환경도 지원하고자 할 때 유용하다.
> 

### **`level`**

`getByRole('heading')` 는 heading 역할을 갖는 모든 요소가 매칭 대상인 반면, `getByRole('heading', { level: 2 })` 와 같이 `level` 옵션을 사용하여 특정 heading level로 대상을 한정할 수 있다.

level 옵션은 시맨틱 HTML 헤딩 요소 <h1>-<h6>에 의해 결정된 지시된 레벨에 매칭되거나 `aria-level` 속성에 매칭되어 요소(들)을 쿼리한다.

```html
<body>
  <section>
    <h1>Heading Level One</h1>
    <h2>First Heading Level Two</h2>
    <h3>Heading Level Three</h3>
    <div role="heading" aria-level="2">Second Heading Level Two</div>
  </section>
</body>
```

`getByRole('heading', { level: 3 })` 을 사용하여 “Heading Level Three” heading 요소를 쿼리할 수 있다.

```javascript
getByRole('heading', {level: 1})
// <h1>Heading Level One</h1>

getAllByRole('heading', {level: 2})
// [
//   <h2>First Heading Level Two</h2>,
//   <div role="heading" aria-level="2">Second Heading Level Two</div>
// ]
```

요소의 `aria-level` 과 `role="heading"` 을 명시적으로 설정하는 것이 위 예제와 같이 가능하지만, HTML 시멘틱 heading인 `<h1>-<h6>` 사용을 강력히 권장한다.

> `level` 옵션은 오직 `heading` 역할에만 사용할 수 있다. 다른 역할에 사용하면 에러가 발생한다.
> 

### **`value`**

range 위젯은 `getByRole('spinbutton')` 으로 쿼리 할 수 있다. 쿼리 대상을 특정 값으로 한정 하려면 `getByRole('spinbutton', { value: { now: 5, min: 0, max: 10, text: 'medium' } })` 와 같이 `value` 옵션을 사용한다.

이때 모든 value를 지정하지 않아도 된다. `getByRole('spinbutton', { value: { now: 5, text: 'medium' } })` 와 같이 매칭하기에 충분한 서브셋이면 된다.

```html
<body>
  <section>
    <button
      role="spinbutton"
      aria-valuenow="5"
      aria-valuemin="0"
      aria-valuemax="10"
      aria-valuetext="medium"
    >
      Volume
    </button>
    <button
      role="spinbutton"
      aria-valuenow="3"
      aria-valuemin="0"
      aria-valuemax="10"
      aria-valuetext="medium"
    >
      Pitch
    </button>
  </section>
</body>
```

다음과 같은 쿼리로 특정한 spinbutton(들)을 쿼리할 수 있다.

```javascript
getByRole('spinbutton', {value: {now: 5}})
// <button>Volume</button>

getAllByRole('spinbutton', {value: {min: 0}})
// [
//   <button>Volume</button>,
//   <button>Pitch</button>
// ]
```

> `value` 옵션에 지정한 모든 프로퍼티는 매칭의 필수조건이다. 예를 들어 `{value: {min: 0, now: 3}}` 를 쿼리 했다면, `aria-valuemin` 는 반드시 0 이고 `aria-valuenow` 는 반드시 3이어야 매칭된다.
> 

> `value` 옵션은 일부 특정한 역할에서만 사용할 수 있다. 사용할 수 없는 역할에서 사용하면 에러가 발생한다.
> 

### **`description`**

동일한 역할을 하는 여러 요소가 있고 액세스 가능한 이름이 없지만 설명이 있는 경우에 대해 액세스 가능한 설명으로 반환된 요소를 필터링할 수 있다.
이는 ****`alertdialog`** 역할을 갖는 요소에서 사용할 수 있고,  `aria-describedby` 속성이 요소의 컨텐츠를 설명하는데 사용된다.

```html
<body>
  <ul>
    <li role="alertdialog" aria-describedby="notification-id-1">
      <div><button>Close</button></div>
      <div id="notification-id-1">You have unread emails</div>
    </li>
    <li role="alertdialog" aria-describedby="notification-id-2">
      <div><button>Close</button></div>
      <div id="notification-id-2">Your session is about to expire</div>
    </li>
  </ul>
</body>
```

아래와 같이 특정 요소를 쿼리할 수 있다.

```javascript
getByRole('alertdialog', {description: 'Your session is about to expire'})
```

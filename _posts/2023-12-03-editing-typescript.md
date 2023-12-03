---
title: Editing
author: sam
date: 2022-12-03 17:32:00 -0500
categories: [VS Code, TYPESCRIPT]
tags: [VS Code, TypeScript]
imgp: /assets/img/basic/
---

# TypeScript 에디팅

## IntelliSense

IntelliSense는 빠르고 정확한 코드 작성을 위해 코드 완성, 호버 정보창, 시그니처 헬프를 제공한다.

![ts-editing.png]({{ page.imgp }}ts-editing.png)

VS Code는 개별 TypeScript 파일 및 TypeScript tsconfig.json 프로젝트에 대한 IntelliSense를 제공한다.

## Hover inoformation

TypeScript 심볼 위에 마우스를 올리면 타입 정보와 관련 문서를 보여준다.

![ts-info.png]({{ page.imgp }}ts-info.png)

키보드 단축키를 사용해서(**Ctrl + K 심볼 선택 -> Ctrl + I**) 호버 정보를 확인할 수 있다.

## Signature help

TypeScript 함수 호출을 작성하면, VS Code는 해당 함수 시그니처에 대한 정보를 보여주고 현재 완료하고 있는 매개 변수를 강조 표시한다.

![ts-editing2.png]({{ page.imgp }}ts-editing2.png)

시그니처 헬프는 `(` 또는 `,`를 함수 호출부 안에서 작성할 때 자동으로 표시된다. **Ctrl + Shift + Space** 단축키를 사용해 수동으로 시그니처헬프를 트리거할 수도 있다.

## Snippets

VS Code는 기본 TypeScript 스닙펫을 포함하고 있으며 타이핑 중에 적절한 스닙펫을 제안한다.

![ts-snippet.png]({{ page.imgp }}ts-snip.png)

확장기능을 통해 추가로 스닙펫을 설치하거나 직접 TypeScript 스닙펫을 정의할 수도 있다.

> Tip : settings 파일의 `editor.snippetSuggestions` 속성값에 `“none”`을 할당해서 스닙펫 기능을 비활성화 할 수 있다. 그 외 할당 가능한 값 “top”, “bottom”, “inline(기본값)” 을 할당해 스닙펫 제안의 상대적 순서를 지정할 수도 있다. 

## Inlay 힌트

Inlay 힌트는 해당 코드가 무엇을 수행하는지 이해를 돕기 위해 인라인으로 추가적인 정보를 표시한다.
매개변수 이름 inlay 힌트는 함수 호출자에서 매개변수 이름을 보여준다.

![ts-hint.png]({{ page.imgp }}ts-hint.png)

이는 각 인수가 어떤 의미인지 한 눈에 이해할 수 도록 한다. 이 기능은 특히 Boolean 플래그를 취하거나 혼합하기 쉬운 매개변수를 갖는 함수에서 유용하다.

매개변수 이름 inlay 힌트를 활성화 하려면, **typescript.inlayHints.parameterNames.enabled** 를 설정한다. 이 옵션에는 아래의 3가지 값을 할당할 수 있다.

- `none` - 매개변수 inlay 힌트 비활성화
- `literals` - 오직 리터럴(`string`, `number`, `Boolean`)에 대한 inlay 힌트만을 표시한다. 다른 타입의 매개변수는 힌트 표시 안함.
- `all` - 모든 인수에 대한 inlay 힌트를 표시한다.

**변수 타입 inlay** 힌트는 명시적으로 타입 주석이 붙지 않은 변수에 대해 타입을 표시한다. 
세팅은 `typescript.inlayHints.variableTypes.enabled` 옵션을 사용한다.

![ts-in1.png]({{ page.imgp }}ts-in1.png)

**프로퍼티 타입 inlay 힌트**는 명시적 타입 주석이 없는 클래스 프로퍼티에 대한 타입을 표시한다.
세팅은 `typescript.inlayHints.propertyDeclarationTypes.enabled` 옵션을 사용한다.

![ts-in2.png]({{ page.imgp }}ts-in2.png)

**매개변수 타입 inlay 힌트**는 암묵적으로 타입이 붙은 매개변수의 타입을 보여준다.
세팅은 `typescript.inlayHints.parameterTypes.enabled` 옵션을 사용한다.

![ts-in3.png]({{ page.imgp }}ts-in3.png)

**반환 타입 inlay 힌트**는 명시적 타입 주석이 붙지않은 함수의 반환 타입을 보여준다.
세팅은 `typescript.inlayHints.functionLikeReturnTypes.enabled` 옵션을 사용한다.

![ts-in4.png]({{ page.imgp }}ts-in4.png)

## 참조 코드렌즈(References CodeLens)

TypeScript 참조 코드렌즈는 export된 객체, 프로퍼티, 메서드, 인터페이스, 클래스의 참조 총계를 인라인으로 표시한다.

![ts-code.png]({{ page.imgp }}ts-code.png)

이 기능을 활성화 하려면 settings 파일에서 `“typescript.referencesCodeLens.enabled” : true` 를 설정해야한다. 
참조 총계를 클릭하면 곧바로 어디서 참조되고 있는지 참조 목록을 보여준다.

![ts-code2.png]({{ page.imgp }}ts-code2.png)

## 구현 코드렌즈(Implementations CodeLens)

TypeScript 구현 코드렌즈는 인터페이스가 총 몇번 구현되었는지를 표시한다.

![ts-lense.png]({{ page.imgp }}ts-lense.png)

이 기능은 `“typescript.implementationsCodeLens.enabled” : true` 로 활성화 한다.
참조 코드렌즈와 마찬가지로, 구현 총계를 클릭하면 곧바로 어디서 구현이 되었는지 구현 목록을 보여준다.

## 자동 import
자동 import 기능은 적절한 심볼을 찾도록 돕고 그 심볼을 import 하는 구문을 자동으로 추가함으로써 코딩 작성 효율을 높인다.
일단 타이핑만 시작해도 이용가능한 TypeScript 심볼 제안이 뜨는것을 볼 수 있을 것이다.

![ts-import.png]({{ page.imgp }}ts-import.png)

제안중 모듈 또는 별개 파일의 것을 선택하면 VS Code는 자동으로 그 심볼을 import 하는 구문을 추가한다.

![ts-import2.png]({{ page.imgp }}ts-import2.png)

자동 import 기능을 비활성화 하려면 `“typescript.suggest.autoImports”: false`를 설정한다.

## JSX 와 자동 클로징 태그(JSX and auto closing tags)

VS Code의 TypeScript 기능에는 JSX 관련 기능도 포함되어 있다. TypeScript 프로젝트에서 JSX을 사용하려면 *.ts 대신 `*.tsx` 파일 확장자를 사용하자.

![ts-tsx.png]({{ page.imgp }}ts-tsx.png)

VS Code에는 TypeScript 안 JSX 태그 자동닫힘(autoclosing)과 같은 JSX용 기능도 포함되어 있다.
`“typescript.autoClosingTags”`에 `false`를 할당하면 JSX 태그 자동닫힘 기능이 비활성화 된다.

## JSDoc 지원

VS Code의 TypeScript intelliSense는 다수의 표준 JSDoc 주석을 이해하며, 이 주석을 사용해 시그니처 헬프, 호버 정보창, 제안에서 입력한 정보와 문서(documentation)을 보여준다.

![ts-doc.png]({{ page.imgp }}ts-doc.png)

TypeScript 코드에서 JSDoc을 사용할 땐, 타입 주석을 포함시켜서 안되는 것을 명시하자. TypeScript 컴파일러는 오직 TypeScript 타입 주석만을 사용하고 JSDoc의 것은 무시할 것이다.

JSDoc 주석 제안(suggestions)을 비활성화 하고 싶다면, `“typescript.suggest.completeJSDocs” : false`를 설정하자.

## Code navigation

코드 네비게이션은 빠르게 TypeScript 프로젝트안을 이동할 수 있게 한다.

- 정의로 이동 **F12** - 심볼을 정의하는 소스코드로 이동한다.
- 정의 보여주기(Peek Definition) **Alt + F12** - 심볼를 정의하는 코드를 인라인에 작은 창을 열어 보여준다.
- 참조로 이동 **Shift + F12** - 심볼에 대한 모든 참조를 보여준다.
- 타입 정의로 이동 - 심볼을 정의하는 타입으로 이동한다. 클래스 인스턴스의 경우, 인스턴스가 정의된곳 대신 클래스 그 자체로 이동한다.
- 구현으로 이동 **Ctrl + F12** - 추상 메서드(abstract method) 또는 인터페이스의 구현으로 이동한다.

심볼 탐색은 Command Palette(**Ctrl + Shift + P**) 에서 Go to Symbol 명령으로 할 수 있다.

- Go to Symbol in Editor(파일내에서 심볼 탐색) **Ctrl + Shift + O**
- Go to Symbol in Workspace **Ctrl + T**

## Formatting

VS Code는 합리적인 기본값으로 설정된 기본 코드 포맷팅을 제공하는 TypeScript 포맷터를 포함하고 있다.

이 내장 포맷터를 설정하기위해 `typescript.format.*` 옵션을 사용한다. 만일 내장 포맷터가 방해가 된다면, `“typescript.format.enable”` 옵션에 `false`를 할당해 기능을 비활성화 할 수도 있다.

좀 더 특화된 코드 포맷팅 스타일을 원한다면, VS Code 마켓플레이스에서 포맷팅 확장기능중 하나를 설치해보자.

## 구문 하이트라이트와 의미 하이라이트

구문 하이라이트 외에도 TypeScript와 JavaScript는 의미 하이라이트를 제공한다.

구문 하이라이트는 어휘 규칙에 기반하여 텍스트에 색을 지정한다. 의미 하이트라이트는 심볼 정보를 기반으로 구문 색상을 풍부하게 한다.

의미 하이라이트가 표시되는지 여부는 현재 컬러 테마에 달려있다. 각 테마는 의미 강조 표시 여부와 의미 토큰의 스타일을 지정하는 방법을 설정할 수 있다.

의미 하이라이트가 활성화되어 있고 컬러 테마에 해당하는 스타일 규칙이 정의되어 있으면 각각의 컬러와 스타일이 표시될것이다.

의미 하이라이트는 다음에 기반하여 색을 변경할 수 있다.

- 리졸브(resolved)된 심볼의 타입 : 네임스페이스, 변수, 프로퍼티, 클래스, 인터페이스, 타입 매개변수
- 변수/프로퍼티가 read-only (const) 또는 수정가능한 것인지
- 변수/프로퍼티 타입이 호출 가능(function 타입) 여부
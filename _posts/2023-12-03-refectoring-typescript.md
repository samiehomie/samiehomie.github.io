---
title: Refectoring
author: sam
date: 2022-12-03 18:32:00 -0500
categories: [VS Code, TYPESCRIPT]
tags: [VS Code, TypeScript]
imgp: /assets/img/basic/
---

# Refectoring TypeScript

Visual Studio Code는 TypeScript 언어 서비스를 통한 TypeScript 리팩토링 지원 기능을 내장하고 있다.

## Rename

가장 간단한 리팩토링 중 하나는 메서드 또는 변수명을 수정하는 것이다. 프로젝트 전체에서 심볼 이름을 변경하고자 한다면 심볼을 커서 아래 두고 **F2** 키를 눌러 심볼 이름을 수정한다.

![ts-refectoring.png]({{ page.imgp }}ts-refectoring.png)

## Refectoring

적용가능한 TypeScript 리팩토링을 보기 위해 소스 코드 영역에 커서를 두고 오른쪽 클릭으로 에디터 컨텍스트 메뉴를 띄우고 **Refector**을 선택하거나 단축키 **Ctrl + Shift + R** 을 입력한다.

![ts-refectoring2.png]({{ page.imgp }}ts-refectoring2.png)

적용할 수 있는 TypeScript 리팩토링은 다음과 같다.

**Extract to method to function** - 선택한 문 또는 표현식을 새 메서드 또는 새 함수로 추출한다.

![ts-opt1.png]({{ page.imgp }}ts-opt1.png)
![ts-opt2.png]({{ page.imgp }}ts-opt2.png)

**Extract to constant** - 선택한 표현식을 새 상수(const)로 추출한다.

**Extract type to interface or type alias** - 선택한 복합 타입(속성을 포함하는 타입)을 타입 별칭이나 인터페이스로 추출한다.

**Move to new file** - 파일 최상위 스코프에 있는 하나 이상의 인터페이스, 함수, 상수, 클래스를 새 파일로 옮긴다. 이 새 파일의 이름은 선택한 심볼의 이름에서 추론된다.

**Convert between named imports and namespace imports** - 명명된 import (`import { Name } from ‘./foo’`)를 네임스페이스 import (`import * as foo from ‘./foo’`) 로 변환한다. 별칭은 모듈 파일명에서 추론한다.

**Convert between default export and named export** - `export default` 에서 명명된 export로(`export const Foo = …`) 변환한다.

**Convert parameters to destructured object** - 긴 인수를 받는 함수를 하나의 인수 객체를 받는 함수로 재작성 한다.

**Generate get and set accessors** - 선택한 클래스 프로퍼티에 대한 getter, setter 함수를 구현해 프로퍼티를 캡슐화한다.

![ts-op3.png]({{ page.imgp }}ts-op3.png)

**Infer function return types** - 함수에 명시적으로 반환 타입 주석을 추가한다.

**Add/remove braces from arrow function** - 한 줄 화살표 함수를 여러 줄 함수로 변환하거나 그 반대로 여러 줄을 한 줄로 변환한다.

## Quick Fixes

Quick Fixes는 간단한 코딩 에러에 대한 수정 제안이다. Quick Fixes는 다음 오류 수정을 제안한다.

- 멤버 엑세스에 빠진 `this` 추가
- 틀린 프로퍼티 이름 수정
- 사용하지 않는 import 또는 닿을 수 없는(unreachable) 코드 제거
- 선언하기

TypeScript 에러에 커서를 이동시키면 이용 가능한 Quick Fixes를 제시하는 불켜진 전구 아이콘이 코드 옆에 보인다. 이 전구 아이콘을 클릭하거나 **Ctrl + .**을 누르면 이용 가능한 리팩토링과 Quick Fixes 목록이 뜬다.

## Unused variables and unreachable code

사용되지 않는 TypeScript 코드, 예를 들어 항상 조건이 `true`인 `if`문의 `else` 블록 또는 참조되지 않는 import는 에디터에서 흐리게 표시된다.

![ts-if.png]({{ page.imgp }}ts-if.png)

이 흐리게 표시된 코드 위에 커서를 놓고 Quick Fix 명령을 트리거(**Ctrl + .**) 하거나 옆에 떠있는 전구 아이콘을 클릭하여 빠르게 이 사용되지 않는 코드를 제거할 수 있다.

사용되지 않는 코드가 흐려지는 기능을 비활성화 하려면, “editor.showUnused” 옵션에 false를 할당한다. 또는 settings를 아래와 같이 수정해 에디터 전체에 대한 설정이 아닌 TypeScript에만 한정하여 비사용 코드 흐림 설정을 끌 수 있다.

```json
"[typescript]": {
    "editor.showUnused":  false
},
"[typescriptreact]": {
    "editor.showUnused":  false
},
```

## Organize Imports

Source Action -> Organize Imports는 import를 정렬하고 비사용 import를 제거한다. Organize Imports는 오른쪽 클릭하면 나오는 컨텍스트 메뉴의 Source Action에서 Organize Imports를 선택하거나 단축키 **Shift + Alt + O** 를 눌러 실행시킨다.

Organize Import를 TypeScript 파일 저장시 자동으로 실행되게 하려면 아래와 같이 설정한다.

```json
"editor.codeActionsOnSave": {
    "source.organizeImports": true
}
```

## Update imports on file move

TypeScript 프로젝트 안에서 다른 파일에 의해 import 되고 있는 파일명이 수정되거나 위치가 바뀐경우, VS Code는 자동으로 바뀐 파일을 참조하고 있는 import를 경로를 바뀐것으로 업데이트한다.

이 기능을 컨트롤하는 옵션은 `typescript.updateImportsOnFileMove.enabled` 이다. 이 옵션에 할당 가능한 값은 아래와 같다.

- `“prompt”` - 기본 값. 파일 변동이 있을시 경로를 업데이트 할 것인지 묻는다.
- `“always”` - 경로를 항상 자동으로 업데이트한다.
- `“never”` - 자동으로 경로를 업데이트 하지 않으며 변경 여부를 묻지도 않는다.
  
## Code Actions on Save

editor.codeActionsOnSave 옵션으로 파일이 저장될때 어떤 코드 액션을 수행할지 설정할 수 있다. 예를 들어, 아래와 같이 설정하여 저장시 Organize Imports를 수행하도록 할 수 있다.

```json
// On save, run both fixAll and organizeImports source actions
"editor.codeActionsOnSave": {
    "source.fixAll": true,
    "source.organizeImports": true,
}
```

또한 editor.codeActionsOnSave에 순서대로 실행한 코드 액션을 배열로 할당할 수 있다. 아래는 할당할 수 있는 코드액션 목록이다.

- `“organizeImports”` - 저장시 organize imports 실행
- `“fixAll”` - 저장시 Auto Fix가 수행 가능한 모든 수정사항(ESLint를 포함한 모든 제공자에게 공급받은 수정사항)을 한번에 처리한다.
- `“fixAll.eslint”` - 오직 ESLint가 제시한 수정사항을 저장시 자동 수정한다.
- `“addMissingImports”` - 저장시 모든 빼먹은 import를 추가한다.

## Code suggestions

VS Code는 `promise`의 `.then` 호출 체인을 `async`와 `await`로 바꾸는것과 같이 몇가지 일반적인 코드 간소화를 자동으로 제안한다.

![ts-promise.png]({{ page.imgp }}ts-promise.png)

이 기능은 `“typescript.suggestionActions.enabled”`에 `false`를 할당해 비활성화 할 수 있다.
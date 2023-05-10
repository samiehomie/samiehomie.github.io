---
title: Using React in VS Code
author: sam
date: 2023-01-10 18:32:00 -0500
categories: [VS Code, JAVASCRIPT]
tags: [VS Code, React]
---

리액트는 페이스북이 개발한 자바스크립트 라이브러리로 유저 인터페이스 구축에 사용된다. VS Code는 리액트 IntelliSense와 뛰어난 코드 네비게이션을 지원한다.

![welcome-to-react](/assets/img/react/welcome-to-react.png)

## Welcome to React

본 튜토리얼에서는 `create-react-app` 생성기를 사용한다. 이 생성기를 사용하고 리액트 애플리케이션 서버를 구독하려면 Node.js 자바스크립트 런타임과 Node.js 패키지 매니저인 npm이 설치돼 있어야한다. Node.js를 설치하면 기본적으로 npm도 함께 설치된다.

> **Tip**: 현재 작업환경에 Node.js와 npm이 정상적으로 설치되었는지 확인하려면 터미널에서 `node --version` 과 `npm --version` 을 입력하자

다음을 입력하여 새 리액트 애플리케이션을 생성할 수 있다.

```bash
npx create-react-app my-app
```

여기서 `my-app`은 생성되는 애플리케이션 프로젝트 폴더명이된다. 리액트 애플리케이션 생성과 의존성 설치에 수분이 소요될 수 있다.

> **Note**: 이전에 `npm install -g create-react-app` 으로 `create-react-app` 을 전역 설치했었다면, `npm uninstall -g create-react-app` 을 사용해 이 패키지를 삭제할 것을 권장한다. 이는 npx가 항상 최신 버전을 사용하도록 하기 위함이다.

폴더 생성이 완료되었다면, 폴더로 이동하여 `npm start` 를 입력하는 것으로 바로 웹 서버를 실행하고 리액트 애플리케이션을 브라우저에서 열 수 있다.

```bash
cd my-app
npm start
```

### Markdown preview

프로젝트 폴더를 확인해보면 `README.md` 마크다운 파일이 있는것을 볼 수 있다. 이 파일은 리액트 전반에 대한 정보와 해당 리액트 애플리케이션에 대해 상세한 설명을 제공한다. 마크다운으로 작성된 파일 이므로 VS Code의 Markdown Preview을 사용하면 더 깔끔하게 화면에 출력할 수 있다. 현재 에디터 그룹에 마크다운 프리뷰창을 띄우려면 **Markdown: Open Preview** (`Ctrl+Shift+V`)을 사용하고 별도 에디터 그룹에 띄우려면 **Markdown: Open Preview to the Side** (`Ctrl+K V`)을 사용하자.

![markdown-preview](/assets/img/react/markdown-preview.png)

### Syntax highlighting and bracket matching

이제 `src` 폴더를 열어 `index.js` 파일을 선택해보자. VS Code에는 다양한 소스 코드 요소에 대한 구문 강조 기능이 있다. 괄호안 구문에 커서를 가져다 놓으면 해당 구문을 담고 있는 열고 닫는 괄호도 함께 선택된다.

![bracket-matching](/assets/img/react/bracket-matching.png)

### IntelliSense

`index.js` 에서 입력을 시작하면 자동 완성과 추천 목록이 뜬다.

![suggestions](/assets/img/react/suggestions.png)

제안중 하나를 선택하고 `.`를 입력하면 IntelliSense를 통해 해당 객체의 메서드와 타입을 확인할 수 있다.

![intelliSense](/assets/img/react/intellisense.png)

VS Code는 자바스크립트 코드의 IntelliSense를 위해 타입스크립트 언어 서비스와 자동 타입 획득(Automatic Type Acquisition, ATA) 기능을 사용한다. ATA는 `package.json` 에서 관리되는 npm 모듈에 대해 타입 선언 파일(`*.d.ts`)을 사용해 타입 정보를 제공한다.

메서드를 선택하면 매개변수에 대한 도움말이 뜬다.

![parameter-help](/assets/img/react/parameter-help.png)

### Go to Definition, Peek definition

타입스크립트 언어 서비스를 사용하는 VS Code는 **Go to Definition**(`F12`) 또는 **Peek Definition**(`Alt+F12`)으로 정의를 확인할 수 있다. `App` 요소에 커서를 올려놓고 마우스 오른쪽 클릭 -> **Peek Definition**을 선택하면 `App.js`에 작성된 `App` 에 대한 정의를 Peek 윈도우(에디터 인라인에 뜨는 간이 창)에 표시한다.

## Debugging React

클라이언트 사이드 리액트 코드 디버그에는 내장 자바스크립트 디버거를 사용한다.

### Set a braekpoint

`index.js`에 braekpoint를 설정하려면 왼쪽 거터(gutter)를 클릭하여 빨간색 원을 생성한다.

![breakpoint](/assets/img/react/breakpoint.png)

### Configure the debugger

최초엔 디버거 설정을 해야한다. 이를 위해 **Run and Debug** 뷰(`Ctrl+Shift+D`)로 이동해 디버거 설정 파일인 `launch.json`을 생성하는 **create a launch.json file** 링크를 클릭하자. 이 후 나타나는 **Select debugger** 드롭다운 목록에선 **Web App (Chrome)**을 선택하자. 그럼 프로젝트안에 새로 생성된 `.vscode` 폴더 안에 `launch.json` 파일이 생성된다. 이 파일에서 웹사이트를 실행하기 위한 세팅을 설정할 수 있다.

예제 진행을 위해 기본 설정된 값에서 하나를 수정하자. `url`의 port를 `8080`에서 `3000`으로 바꾼다.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Edge against localhost",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

개발 서버를 먼저 실행시키고(`npm start`) `F5` 또는 **Run and Debug** 뷰의 상단 녹색 화살표 버튼을 눌러 디버거를 실행시키자. 디버깅이 시작되면서 새 브라우저 창이 열린다. 디버거 이전에 소스 코드가 실행되었으므로 웹 페이지가 새로 고쳐지기 전까진 앞서 설정해 놓은 breakpoint에 도달하지 않는다. 페이지 새로고침 이후에 설정한 breakpoint에 도달한다.

![hit-breakpoint](/assets/img/react/hit-breakpoint.png)

breakpoint에 도달한 상태에서 `F10` 을 눌러 소스 코드를 단계별로 진행시키면서 예제의 `element` 같은 변수에 어떤 값이 담겨 있는지, 클라이언트 사이드 리액트 앱의 콜스택에 어떤 태스크가 처리를 기다리고 있는지를 확인할 수 있다.

![debug-variable](/assets/img/react/debug-variable.png)

### Live editing and debugging

webpack을 사용하고 있다면 live 에디팅과 디버깅을 가능케하는 웹팩의 HMR(Hot Module Replacement)을 활용하여 좀 더 효율적인 워크플로를 가져갈 수 있다. 즉, VS Code를 떠나지 않고 리액트 코드 수정과 디버깅을 하나의 연결된 워크플로로 처리할 수 있는 것이다.

![live editing and debugging](/assets/img/react/nwf.png)

`create-react-app` 으로 프로젝트를 생성하고 최신 VS Code를 사용한다면 별도 설치나 설정 없이 위 과정대로 `launch.json` 파일을 생성하면 이 live 에디팅, 디버깅을 활용할 수 있다.

> **webpack의 HMR**: webpack이 빌드한 웹 애플리케이션의 모듈 변화를 감지해 새로고침 없이 런타임에서 업데이트하는 기능

## Linting

Linter는 애플리케이션을 실행하기 전에 소스 코드를 분석하여 잠재적 문제에 대해 경고한다. VS Code에 포함된 자바스크립트 언어 서비스는 기본적으로 구문 에러 검사를 지원하며, **Problems** 패널(**View > Problems** `Ctrl+Shift+M`) 에서 확인된 문제를 확인할 수 있다.

소스 코드에 작은 에러를 발생시켜보면 꼬불꼬불한 붉은 선과 Problems 패널에 뜬 에러 메시지를 확인할 수 있다.

![js-error](/assets/img/react/js-error.png)

Linter는 코딩 규칙을 적용하고 안티패턴을 탐지하는 보다 정교한 분석을 제공할 수 있다. 대표적인 자바스크립트 Linter는 ESLint다. VS Code의 ESLint 확장 기능과 함께 사용하면 뛰어난 정적 분석 기능을 제공한다.

먼저 쉘에 다음과 같이 입력하여 ESLint를 설치한다.

```bash
npm install -g eslint
```

이어서 ESLint 확장기능을 **Extensions** 뷰에서 'eslint'를 입력하여 찾아 설치한다.

![eslint-extension](/assets/img/react/eslint-extension.png)

ESLint 확장기능을 설치하고 VS Code를 리로드하고 나면 ESLint 설정 파일인 `.eslintrc.js` 를 생성하자. **Command Palette** (`Ctrl+Shift+P`)에 **ESLint: Create ESLint configuration** 명령을 입력해 생성한다.

![create-eslintrc](/assets/img/react/create-eslintrc.png)

이 명령을 실행하면 터피널 패널에 일련에 질문을 띄워 답을 받는다. 모든 답변을 기본값으로 선택하면 프로젝트 루트 폴더에 다음과 같은 `.eslintrc.js` 파일이 생성된다.

```javascript
module.exports = {
  env: {
    browser: true,
    es2020: true
  },
  extends: ["eslint:recommended", "plugin:react/recommended"],
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 11,
    sourceType: "module"
  },
  plugins: ["react"],
  rules: {}
};
```

ESLint는 이제 열려있는 파일을 분석하여 `index.j`에 정의되었으나 한 번도 사용되지 않은 'App'에 대한 경고를 표시한다.

![app-is-unused](/assets/img/react/app-is-unused.png)

`eslintrc.js` 파일에서 ESLint가 강제할 규칙(rules)을 수정할 수 있다. 추가 세미콜론에 대한 규칙을 추가해보자.

```javascript
    'rules': {
        'no-extra-semi': 'error'
    }
```

이제 한 줄에 세미콜론이 여러개 있는 경우 에러임을 알리는 빨간색 꼬블한 줄이 에디터에 표시되고, Problems 패널에 에러 항목이 뜨는것을 볼 수 있다.

![extra-semi-error](/assets/img/react/extra-semi-error.png)

## Popular Starter Kits

본 튜토리얼에서는 간단한 리액트 애플리케이션을 생성하기 위해 `create-react-app` 생성기를 사용했다. 이외에도 첫 리액트 애플리케이션을 구축하는데 도움이 되는 많은 샘플과 스타터 킷이 있다.

### VS Code React Sample

리액트 애플리케이션 [샘플](https://github.com/microsoft/vscode-react-sample, "react application sample")로 Node.js Express 서버 코드를 포함한 간단한 TODO 앱 프로젝트다. 이 샘플을 통해 Babel ES6 트랜스파일러와 사이트 에셋을 번들링하는 webpack의 사용법을 확인할 수 있다.

### TypeScript React

`create-react-app` 생성기 사용시 타입스크립트 템플릿을 명시함으로써 타입스크립트 버전 `create-react-app`을 만들 수 있다.

```bash
npx create-react-app my-app --template typescript
```

## Common questions

### Can I get IntelliSense within declarative JSX?

가능하다. 예를 들어, `create-react-app` 프로젝트의 `App.js` 파일을 열었을 때 `render()` 메서드 안 리액트 JSX에서 IntelliSense가 작동하는 것을 볼 수 있다.

![jsx-intellisense](/assets/img/react/jsx-intellisense.png)

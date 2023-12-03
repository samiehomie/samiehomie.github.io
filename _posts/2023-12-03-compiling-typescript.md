---
title: Compiling
author: sam
date: 2022-12-03 18:32:00 -0500
categories: [VS Code, TYPESCRIPT]
tags: [VS Code, TypeScript]
imgp: /assets/img/basic/
---

# Compiling TypeScript

TypeScript는 JavaScript로 트랜스파일되는 타입화된 JavaScript의 상위 집합이다. TypeScript는 견고한  컴포넌트를 만들수 있도록 돕는 클래스, 모듈, 인터페이스를 제공한다.

## Install the TypeScript compiler
VS Code에는 TypeScript를 지원하는 기능이 포함되어 있지만 TypeScript 컴파일러 `tsc` 는 포함하고 있지 않다. 따라서 TypeScript 코드를 JavaScript로 트랜스파일 하기 위해선 TypeScript 컴파일러를 전역 또는 프로젝트 워크스페이스에 설치해야 한다.

가장 간단한 방법은 Node.js 패키지 매니저인 npm을 사용하는 것으로, npm이 이미 설치되어 있다면 다음 명령으로 TypeScript를 전역설치(`-g`) 할 수 있다.

`npm install -g typescript`

설치가 제대로 되었는지 다음 버전 확인 명령으로 확인할 수 있다.

`tsc --version`

프로젝트에 로컬로 설치(`npm install —save-dev typescript`)하는 방법도 있다. 로컬 설치의 이점으로 다른 TypeScript 프로젝트와의 충돌 문제를 피할 수 있다.

## Compiler versus language service

VS Code의 **TypeScript의 언어 서비스(language service)와 설치한 TypeScript 컴파일러는 별개**임을 명심하자. 
TypeScript 파일을 열고 하단 Status Bar를 보면 VS Code의 TypeScript 버전을 확인할 수 있다.

![ts.png]({{ page.imgp }}ts.png)
뒤에서 VS Code가 사용하는 TypeScript 언어 서비스의 버전을 바꾸는 방법에 대해 설명한다.

## tsconfig.json

TypeScript 프로젝트의 시작은 대게 tsconfig.json 파일을 추가하는 것으로 시작된다. tsconfig.json 파일은 TypeScript 프로젝트의 세팅, 예를 들어 컴파일 옵션과 어떤 파일을 컴파일 대상에 포함시킬지 등을 설정하기 위해 사용한다. 또한, tsconfig.json 파일은 intelliSense(`Ctrl + Space`)를 지원한다.

간단한 tsconfig.json은 아래와 같다.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true
  }
}
```

## Transpile TypeScript into JavaScript

VS Code는 통합된 task runner를 통해 tsc와 통합되었다. 이를 사용해 `.ts` 파일을 `.js` 파일로 트랜스파일 할 수 있다. VS Code task는 또한 통합된 에러 경고 탐지를 Problems 패널에서 제공한다. 다음 과정을 따라 간단한 TypeScript 파일을 트랜스파일링 해보자.

### Step 1: Create a simple TS file

helloworld.ts. 파일을 생성하고 다음 코드를 파일에 입력하자.

```typescript
let message: string = 'Hello World';
console.log(message);
```

통합 터미널을 열고(Ctrl + \`) `tsc helloworld.ts` 명령을 입력한다. 트랜스 파일링을 진행하고 트랜스파일된 helloworld.js JavaScript 파일이 생성된다.

![ts-outcome.png]({{ page.imgp }}ts-outcome.png)

### Step 2: Run the TypeScript build

전역 Terminal 메뉴에서 **Run build Task** (Ctrl + Shift + B)를 실행하자. tsconfig.json을 생성했다면 아래와 같은 선택 항목을 보여준다.

![ts-select.png]({{ page.imgp }}ts-select.png)

**tsc: build** 항목을 선택하자. helloworld.js와 helloworld.js.map 파일이 워크스페이스에 생성된다(내부적으론 `tsc -p .` 명령이 실행된다).

**tsc: watch**를 선택하면 컴파일러가 TypeScript 파일의 변화를 감지하여 매 변화가 있을 때마다 트랜스파일러를 실행한다.

### Step 3: Make the TypeScript Buid the default

TypeScript build task를 기본 build task로 설정하여 **Run Build Task**(Ctrl + Shift + B)를 실행하면 곧바로 실행 되도록 할 수도 있다. 이를 위해 전역 Terminal 메뉴에서 **Configure Default Build Task**를 선택하면 사용 가능한 build task 항목이 제시된다. **tsc: build**를 선택하면 .vscode 폴더 안에 다음 tasks.json 파일이 생성된다.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "type": "typescript",
            "tsconfig": "tsconfig.json",
            "problemMatcher": [
                "$tsc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

`group` JSON 객체에 `kind` 프로퍼티가 `build`로 설정되었고 `isDefault`가 `true`인것에 주목하자. 이제 **Run Build Task**(Ctrl + Shift + B)를 실행하면 task 선택 프롬프트가 뜨지 않고 바로 컴파일을 시작한다.

### Step 4: Reviewing build issues

VS Code task 시스템은 problem matcher를 통해 build 이슈를 탐지한다. 이 problem matcher는 특정 build 툴에 기반해 build 결과를 파싱하고 통합된 이슈 화면과 네비게이션을 제공한다. 위에서 본 tasks.json의 `$tsc`가 TypeScript 컴파일러 결과에 대한 problem matcher다.

예를 들어, TypeScript 파일에 `console.logg` 같은 간단한 에러가 있는 경우, tsc로부터 아래와 같은 결과를 얻을 수 있다.

`HelloWorld.ts(3,17): error TS2339: Property 'logg' does not exist on type 'Console'.`

이 메시지는 terminal 패널(Ctrl + `)의 terminal view 드롭다운에서 **Tasks - build tsconfig.json**을 선택하면 확인할 수 있다.

하단 Status Bar에서는 에러와 경고의 총계를 볼 수 있다. 에러와 경고 아이콘을 클릭하면 발생한 문제 리스트를 확인하고 문제가 있는 해당 위치로 이동할 수 있다.

![ts-status.png]({{ page.imgp }}ts-status.png)

`Ctrl + Shift + M` 단축키를 사용해 열수도 있다.

## JavaScript source map support

TypeScript 디버깅은 JavaScript source map을 지원한다. TypeScript 파일에 대한 source map을 생성하기 위해선, `—sourcemap` 옵션과 함께 컴파일 하거나 tsconfig.json 파일의 `sourceMap` 프로퍼티에 `true`를 할당하면 된다.

## 컴파일된 생성파일 위치 지정

생성된 JavaScript 파일이 TypeScript 소스와 동일한 폴더에 있으면 혼선을 줄 수 있다. `outDir` 속성을 사용해 컴파일러의 출력 디렉터리를 지정할 수 있다.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out"
  }
}
```

## 생성된 JavaScript 파일 숨기기

TypeScript로 작업할 때, 생성된 JavaScript 파일이 파일 익스플로러나 검색 결과에 보이는 것이 원치 않을 수 있다. VS Code는 워크스페이스 세팅의 `files.exclude` 옵션을 통한 필터링 기능을 제공하며, 아래와 같이 생성된 파일을 숨기는 표현식을 쉽게 만들 수 있다.

`**/*.js: { “when”: “$(basename).ts” }`

이 패턴은 같은 이름의 TypeScript 파일이 나란히 있는 JavaScript 파일에 매칭된다. JavaScript 리소스가 TypeScript 파일과 동일한 위치에 생성된 경우 파일 익스플로러에 더 이상 해당 리소스가 표시되지 않는다.

![ts-menu1.png]({{ page.imgp }}ts-menu1.png)
![ts-menu2.png]({{ page.imgp }}ts-menu2.png)

프로젝트 루트에 있는 .vscode 폴더에 위치한 워크스페이스 setting.json파일에 files.exclude 속성을 필터링할 값과 함께 추가하자. `settings.json` 파일은 **Command Palette(Ctrl + Shift + P)**에 **Preference: Open Workspace Settings (JSON)** 명령을 입력하여 열수 있다.

.ts, .tsx 파일에서 생성된 JavaScript 모두를 제외하고 싶다면, 아래 표현식을 사용한다.

```json
"files.exclude": {
    "**/*.js": { "when": "$(basename).ts" },
    "**/**.js": { "when": "$(basename).tsx" }
}
```

여기엔 약간의 트릭이 사용되었다. 글로브 패턴은 키로 사용되므로 유일한(유니크) 값 이어야 한다. 따라서 위 세팅은 유니크 키를 제공하기 위해 사실상 같은 기능을하는 두 개의 다른 글로브 패턴을 사용했다.

## 최신 TypeScript 버전 사용

VS Code는 TypeScript의 언어 서비스를 안정화 버전 기준 최신으로 제공하며 기본적으로 이 버전을 사용하여 워크스페이스에 intelliSense를 제공한다. 이 워크스페이스의 TypeScript 버전은 `*.ts` 파일을 컴파일 하는데 사용하는 TypeScript 버전과는 별개다. 물론 VS Code에 내장된 이 intelliSense를 위한 워크스페이스 TypeScript를 그대로 사용해도 큰 문제는 없지만, 때로는 아래의 이유로 intelliSense에 사용되는 TypeScript 버전을 변경해줄 필요를 느낄 수 있다.

- TypeScript nightly 빌드로 바꿔 최신 TypeScript 기능을 테스트 해보고자 할 때
- 코드를 컴파일할 때 사용하는 것과 동일한 버전의 IntelliSense용 TypeScript를 사용하고자 할 때

활성화된 intelliSense용 TypeScript 버전과 설치된 위치는 TypeScript 파일을 보고 있을 때 하단 Status 바에 표시된다.

![ts-status2.png]({{ page.imgp }}ts-status2.png)

워크스페이스에서 이 TypeScript 기본 버전을 바꾸는 방법에 대해 알아보자.

### TypeScript 워크스페이스 버전 사용하기

워크스페이스에 특정 TypeScript 버전이 설치된 경우(전역이 아니라 프로젝트 package.json에 typescript가 설치된 경우) TypeScript 또는 JavaScript 파일을 열고 Status 바에서 TypeScript 버전 번호를 클릭하여 워크스페이스 버전의 TypeScript와 VS Code가 기본적으로 사용하는 버전 간에 전환할 수 있다. 아래처럼 메시지 박스가 나타나 VS Code가 어떤 버전의 TypeScript를 사용하게 할 것인지 묻는다.

![ts-workspace.png]({{ page.imgp }}ts-workspace.png)

이를 통해 VS Code에 제공되는 TypeScript 버전과 워크스페이스의 TypeScript 버전간 전환을 할 수 있다. 이 TypeScript 버전 선택은 **TypeScript: Select TypeScript Version** 명령어로도 트리거할 수 있다.

VS Code는 워크스페이스의 루트에 있는 node_modules에 설치된 TypeScript의 버전(워크스페이스 버전)을 자동으로 탐지한다. 또한 유저 또는 워크스페이스 settings의 `typescript.tsdk` 속성을 설정하여 명시적으로 사용할 TypeScript 버전을 정할수도 있다. typescript.tsdk 값은 반드시 `tsserver.js` 파일을 포함하는 디렉터리를 가리켜야 한다. `npm list -g typescript` 명령을 사용해 TypeScript가 설치된 위치를 찾을 수 있다. tsserver.js 파일은 보통 lib 폴더 안에 있다.

```json
{
  	"typescript.tsdk": "/usr/local/lib/node_modules/typescript/lib"
}
```

> Tip : 특정 TypeScript 버전을 얻기 위해선, npm install 뒤에 `@version`을 명시해야 한다. 예를 들어, TypeScript 3.6.0 버전을 원한다면 `npm install —save-dev typescript@3.6.0`을 사용한다. next 버전을 프리뷰 해보고 싶다면 `npm install —save-dev typescript@next`를 사용한다.

또한 typescript.tsdk에 워크스페이스에 있는 tsserver.js 파일이 담긴 디렉터리 경로를 할당함으로써 워크스페이스의 특정 TypeScript 버전을 VS Code가 사용하게 할 수 있다.

```json
{
  "typescript.tsdk": "./node_modules/typescript/lib"
}
```

이 typescript.tsdk 워크스페이스 설정은 단지 VS Code에게 TypeScript의 워크스페이스 버전 존재를 알릴뿐이다. 실제로 intelliSense에 이 워크스페이스 버전 TypeScript가 사용되게 하기 위해선, TypeScript: Select TypeScript Version 명령을 실행하고 워크스페이스 버전을 선택해야 한다.

### TypeScript nightly 빌드 사용하기

VS Code에서 최신 버전 TypeScript 기능을 사용하는 가장 간단한 방법은 JavaScript and TypeScript Nightly extension을 설치하는 것이다. 이 확장기능은 자동으로 VS Code에 내장된 TypeScript 버전을 최신 TypeScript nightly 빌드로 교체한다.

## TypeScript와 JavaScript가 혼합된 프로젝트

TypeScript아 JavaScript가 혼합된 프로젝트 운영이 가능하다. TypeScript 프로젝트안에 JavaScript 파일이 정상 작동하도록 하기 위해서는, `tsconfig.json`의 `allowJS` 속성에 `true`를 설정해야 한다.

> Tip : tsc 컴파일러는 jsconfig.json 파일의 존재를 자동으로 감지하지 않는다. –p 인수를 사용하여 tsc가 jsconfig.json 파일을 사용하도록 해야한다(예 : `tsc -p jsconfig.json`).

## Common questions

### TypeScript의 “Cannot compile external module”에러 해결

이 에러는 프로젝트 루트 폴더에 tsconfig.json 파일을 생성함으로서 해결할 수 있다. tsconfig.json 파일로 VS Code가 TypeScript 파일을 컴파일 하는 방식을 컨트롤할 수 있다.

### 일부 에러가 ‘경고’로 보고되는 이유

기본적으로 VS Code 코드 스타일 이슈는 에러가 아닌 경고로 표시한다. 경고로 표시되는 케이스는 아래와 같다.

- 변수가 선언되었으나 사용되지 않는 경우
- 프로퍼티가 선언되고 그 값이 한 번도 사용되지 않는 경우
- 닿을 수 없는 코드(예: 조건이 항상 false여서 사용되지 않는 코드)가 감지된 경우
- 사용되지 않는 라벨
- `switch`에서 의도적으로 `case` 내 `break`를 생략해 다음 case로 이동시키는 Fall Through 케이스
- 코드 모든 경로가 값을 반환하지 않는 경우

이들 케이스를 경고로 다루는 것은 TSLint 등의 다른 툴과 동일하다. 반면 커맨드 라인에서 `tsc`를 실행했을 때는 여전히 에러로 표시된다.

이 동작을 비활성화 하고 싶다면 **settings**에서 `"typescript.reportStyleChecksAsWarnings": false` 로 설정하자.

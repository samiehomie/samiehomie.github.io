---
title: Debugging
author: sam
date: 2022-12-03 19:32:00 -0500
categories: [VS Code, TYPESCRIPT]
tags: [VS Code, TypeScript]
imgp: /assets/img/basic/
---

# Debugging TypeScript

Visual Studio Code는 내장된 Node.js 디버거와 Edge 와 Chrome 디버거를 통해 TypeScript 디버깅을 지원한다.

## JavaScript source map support

TypeScript 디버깅은 JavaScript source map을 지원한다. TypeScript 파일의 컴파일링 결과물로 source map을 생성하려면, `—sourcemap` 옵션과 함께 컴파일을 하거나, tsconfig.json 파일의 sourceMap 프로퍼티 값으로 true를 설정해야한다.

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out",
    "sourceMap": true
  }
}
```

더 높은 수준의 디버깅 컨트롤을 위해서는 디버그 설정 파일은 `launch.json`을 직접 만들어야 한다.

기본 설정을 보려면, **Run and Debug** 창으로 (**Ctrl + Shift + D**) 이동한 뒤 **create a launch.json file** 링크를 선택한다.

프로젝트에서 탐지된 기본 값이 설정된 launch.json 파일이 .vscode 폴더 안에 생성된다.

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/helloworld.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/out/**/*.js"]
    }
  ]
}
```

VS Code는 이 프로그램(예제에서는 helloworld.ts)을 실행하면서 빌드로 preLaunchTask를 포함하고 debugger에 생성된 JavaScript 파일의  위치를 알린다.

launch.json에서 다른 디버그 설정 옵션을 추가하는데 도움을 주는 제안사항과 정보를 제공하는 intelliSense가 작동한다. 또는 하단 오른쪽에 위치한 **Add Configuration** 버튼으로 새 디버그 설정을 launch.json에 추가할 수 있다.

![ts-confg.png]({{ page.imgp }}ts-confg.png)

## Mapping the output location
컴파일되어 생성된 JavaScript 파일이 소스 파일 옆에 없는 경우, launch.json의 `outFiles` 속성을 생성되는 JavaScript 파일 위치로 설정하여 VS Code dubugger가 해당 파일을 찾도록 한다. 원본 소스(TypeScript)에 breakpoint를 설정할 때마다 VS Code는 outFiles 값으로 설정된 glob 패턴으로 지정한 파일을 검색하여 생성된 소스(JavaScript)를 찾는다.

## Client-side debugging
TypeScript는 Node.js 애플리케이션 뿐만 아니라 클라이언트 사이드 코드를 작성하는데도 매우 유용하며, 클라이언트 사이드 코드를 내장된 Edge and Chrome debugger로 디버그할 수 있다.

클라이언트 사이드 디버깅을 확인하기 위해 간단한 웹 애플리케이션을 만들어보자.

HelloWeb 폴더를 생성하고 다음 세 개 파일을 추가하자.

**helloweb.ts**

```javascript
let message: string = 'Hello Web';
document.body.innerHTML = message;
```

**helloweb.html**
```html
<!DOCTYPE html>
<html>
    <head><title>TypeScript Hello Web</title></head>
    <body>
        <script src="out/helloweb.js"></script>
    </body>
</html>
```

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out",
    "sourceMap": true
  }
}
```

앱 빌드를 위해 `tsc`를 실행하고 브라우저에 helloweb.html을 열어(파일 탐색기에서 helloweb.html을 오른쪽 클릭하고 Copy Path를 선택하여 얻은 주소를 브라우저에 붙여넣기 하자) 테스를 해보자.

**Run and Debug** view를 실행하고(**Ctrl + shift + D**), **create a launch.json file** 링크를 클릭한 후 debugger 선택창에서 **Web App (Edge**)와 **Web App (Chrome)** 중 선호하는 것을 선택하자.

생성된 launch.json 파일에서 `url` 속성의 값을 아래와 같이 helloweb.html 로컬 파일의 URL로 수정하자.

```json
 {
  "version": "0.2.0",
  "configurations": [
    {
      "type": "msedge",
      "request": "launch",
      "name": "Launch Edge against localhost",
      "url": "file:///C:/Users/username/HelloWeb/helloweb.html",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

**Run and Debug** 창으로 이동해 상단 드롭다운을 내려보면 새로운 설정인 **Launch Chrome against localhost**가 보인다. 이 설정을 실행하면 위에서 만든 웹페이지가 브라우저에서 실행될 것이다. 에디터에서 helloweb.ts를 열고 왼쪽 여백(gutter)를 클릭하여 breakpoint(빨간색 원)을 추가한다. **F5**를 누르면 디버그 세션을 시작되면서 브라우저가 실행되고 helloweb.ts에서 지정한 breakpoint까지 진행됨을 확인할 수 있다.

![ts-break.png]({{ page.imgp }}ts-break.png)

## Common questions

### 대응하는 JavaScript 파일을 찾지 못해 프로그램을 실행할 수 없는 경우

높은 확률로 launch.json의 `outFiles`나 tsconfig.json의 `“sourceMap”: true`를 설정하지 않았을 것이다. 이를 설정하지 않으면 VS Code Node.js debugger는 TypeScript 소스 코드를 실행중인 JavaScript 코드에 매핑할 수 없다. source map 관련 설정을 켜고 다시 프로젝트를 빌드하자.
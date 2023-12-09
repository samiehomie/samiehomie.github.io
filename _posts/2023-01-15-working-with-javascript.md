---
title: Working with JavaScript
author: sam
date: 2023-01-15 18:32:00 -0500
categories: [VS Code, JAVASCRIPT]
tags: [VS Code, JavaScript]
---

# Working with JavaScript

## IntelliSense

VS Code의 JavaScript IntelliSense는 TypeScript 팀이 개발한 JavaScript 언어 서비스에 의해 구동된다. IntelliSense는 별도 설정없이 대부분의 JavaScript 프로젝트에서 작동하긴 하지만, `JSDoc` 와 함께 사용하거나 `jsconfig.json` 파일을 설정함으로써 IntelliSense를 더 유용하게 만들 수 도 있다.

### Typings and Automatic Type Acquisition

자바스크립트 라이브러리와 프레임워크에 대한 IntelliSense는 타입스크립트의 타입 선언 (typings) 파일에 의해 구동된다.

대다수 유명 라이브러리는 타입 선언 파일을 포함하고 있으므로 이를통해 IntelliSense도 자동으로 제공된다. 라이브러리에 타입 선언 파일이 없는 경우엔 VS Code의 자동 타입 획득(Automatic Type Acquisition)이 자동으로 타입 커뮤니티에서 유지보수하는 타입 선언 파일을 설치한다.

자동 타입 확득은 NodeJS 런타임에 포함된 NodeJS 패키지 매니저(npmjs)를 필요로 한다. 아래 이미지를 보면 lodash 라이브러리의 메서드 설명, 메서드 시그니처, 매개변수 정보가 IntelliSense를 통해 제공되고 있는걸 볼 수 있다.

![javascript-pkg](/assets/img/react/js-assist.png)

자바스크립트 파일에 import 하거나 프로젝트의 package.json 에서 관리되는 패키지에 대한 타입 선언 파일은 VS Code가 자동으로 다운받고 관리한다.

```json
{
  "dependencies": {
    "lodash": "^4.17.0"
  }
}
```

또는 `jsconfig.json`에 타입 선언 파일을 가져올 패키지를 명시적으로 나열할 수도 있다.

```json
{
  "typeAcquisition": {
    "include": ["jquery"]
  }
}
```

대부분의 자바스크립트 라이브러리는 타입 선언 파일을 포함하거나 이용할 수 있는 타입 선언 파일이 별도라도 존재한다. TypeSearch 사이트를 통해 라이브러의 타입 선언 파일을 찾아볼 수 있다.

### Fixing npm not installed warning for Automatic Type Acquisition

자동 타입 획득 기능은 NodeJS 패키지 관리자인 npm을 사용해 타입 선언 파일을 설치하고 관리한다. 따라서 npm이 설치되어 있지 않다면, 자동 타입 획득 관련하여 ‘npm 설치 안됐음’ 경고가 뜰것이다.

만일 npm을 설치했음에도 경고 메시지가 계속 뜬다면, VS Code에 `typescript.npm` 설정을 통해 npm이 어디에 설치되었는지 명시적으로 알려줄 수 있다. 이 설정의 값으로는 로컬 머신에 있는 npm 실행파일의 전체 경로여야 하며, 현재 프로젝트에서 사용중인 npm의 버전과 일치하지 않아도 된다. typescript.npm은 TypeScript 2.3.4+ 를 필요로 한다.

예를 들어 윈도우 환경이라면 settings.json 파일에 아래와 같이 설정할 수 있다.

```json
{
  "typescript.npm": "C:\\Program Files\\nodejs\\npm.cmd"
}
```

## JavaScript projects (jsconfig.json)

jsconfig.json의 존재는 해당 디렉터리가 이 자바스크립트 프로젝트의 루트임을 나타낸다. jsconfig.json은 자바스크립트 언어 서비스에 의해 제공되는 언어 기능에 대한 옵션과 루트 파일을 지정한다. 일반적으로 jsconfig.json 파일은 필수는 아니며, 다음과 같은 필요로 하는 상황이 존재한다.

- 일부 파일을 자바스크립트 프로젝트에서 제외하여, IntelliSense 표시와 같은 지원에서 제외하고자 할 때. 프론트엔드와 백엔드 코드에서 흔하게 있는 케이스다.
- 워크페이스에 복수의 프로젝트가 포함된 경우. 이 경우 각 프로젝트의 루트 폴더에 jsconfig.json 파일을 추가해야 한다.
- 자바스크립트 소스 코드를 다운레벨 컴파일하는 용도로 타입스크립트 컴파일러를 사용중일 때

### Location of jsconfig.json

작성한 코드를 자바스크립트 프로젝트로 정의하려면 아래 보이는 이미지처럼 자바스크립트 코드의 루트에 jsconfig.json 파일을 생성하자. 자바스크립트 프로젝트는 프로젝트의 소스 파일이며 주로 dist로 명명되는 파생 파일 또는 package 파일은 포함되어선 안된다.

![javascript-pkg](/assets/img/react/js-pkg.png)

더 복잡한 구조의 프로젝트의 경우, 하나의 워크스페이스에 복수의 jsconfig.json 파일이 포함될 수 있다. 하나의 워크스페이스에 복수의 프로젝트가 있어 함수명 등을 추천하는 IntelliSense에 서로 별개인 프로젝트 함수명이 추천으로 뜨는 문제등을 막기위해 각 프로젝트 마다 jsconfig.json을 추가한다.

아래 그림은 클라이언트와 서버 폴더가 있는 프로젝트로, 두 개의 별도 자바스크립트 프로젝트를 보여준다.

![javascript-pkg](/assets/img/react/js-pkg2.png)

### Writing jsconfig.json

아래는 jsconfig.json 파일에 대한 간단한 템플릿으로, 컴파일 결과를 설정하는 target 속성에는 ES6를 프로젝트에서 제외할 소스를 지정하는 **exclude** 속성에는 node_modules 폴더를 할당했다. 이 코드를 복사하여 jsconfig.json 파일에 붙여넣자.

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6"
  },
  "exclude": ["node_modules", "**/node_modules/*"]
}
```

exclude 속성은 VS Code가 지원하는 언어 서비스 기능에 어떤 파일을 프로젝트에서 제외시킬지를 알린다. IntelliSense가 너무 느리다면, 이 exclude 속성에 제외시킬 폴더를 추가하자(VS Code는 자동 완성 기능이 느려진 것을 감지하면 이 작업을 수행하라는 메시지를 띄운다). 보통 dist와 같은 빌드 과정에 생성되는 파일을 exclude에 포함시킨다. 이러한 파일은 제안을 두 번 표시하고 IntelliSense를 느리가 한다.

include 속성을 사용하여 프로젝트 파일을 명시적으로 설정할 수 있다. include 속성이 없는 경우 기본적으로 루트 디렉터리 및 하위 디렉터리에 있는 모든 파일을 프로젝트에 포함시킨다. include 특성을 지정하면 해당 파일만 포함된다.

다음은 명시적으로 include 속성을 사용한 예다.

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6"
  },
  "include": ["src/**/*"]
}
```

include 속성에 할당할 경로로 src 폴더만 사용하는게 일반적이며 에러가 적은 방법이다. exclude와 include에 할당하는 경로는 jsconfig.json 파일 위치에 기준한 상대경로임을 주의하자.

### Migrating to TypeScript

타입스크립트와 자바스크립트가 혼합된 프로젝트가 가능하다. 자바스크립트 프로젝트를 타입스크립트 프로젝트로 마이그레이션 하기 위해선 jsconfig.json 파일명을 tsconfig.json으로 수정하고 allowJS 속성에 true를 할당해야 한다. allowJS 속성에 true를 할당하는 것 외에 tsconfig.json은 jsconfig.json과 동일하다.
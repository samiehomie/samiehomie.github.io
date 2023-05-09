---
title: Node.js debugging in VS Code
author: sam
date: 2023-05-07 18:32:00 -0500
categories: [VS Code, JAVASCRIPT]
tags: [VS Code, JavaScript]
---

# Node.js debugging in VS Code

VS Code는 Node.js 런타임에 대한 디버깅 지원을 포함하고 있으며 자바스크립트, 타입스크립트 및 자바스크립트로 변환된 다른 많은 언어를 디버깅할 수 있다. VS Code에서 Node.js 디버깅을 위해 프로젝트를 설정하는 것은 적절한 launch 기본 설정값과 스니펫을 제공하므로 어렵지 않다.

VS Code에서 Node.js 프로그램을 디버깅하는 방법에는 아래와 같은 것들이 있다.

1. VS Code의 통합 터미널에서 실행한 프로세스를 디버깅하기 위해 **auto attach** 사용
2. 자바스크립트 디버그 터미널 사용하기
3. 프로그램을 실행하는 **launch** 설정 사용 또는 VS Code 외부에서 실행되는 **attach to process** 사용하기

## Auto Attach

**Auto Attach** 기능이 활성화되면 Node 디버거가 자동으로 VS Code 통합 터미널에서 실행된 Node.js 프로세스에 연결된다. 이 기능을 활성화 하기 위해선 Command Palette(`Ctrl+Shift+P`) 에서 **Toggle Auto Attach** 명령을 입력하거나 이미 활성화된 경우엔, 상태바의 **Auto Attach** 항목을 사용한다.

Auto Attach엔 총 3가지 모드가 있으며, **debug.javascript.autoAttachFilter** 설정과 **Toggle Auto Attach** 명령 실행후 뜨는 프롬프트에서 선택할 수 있다.

- `smart` - `node_modules` 폴더 밖의 스크립트를 실행하거나, mocha나 ts-node같은 일반적인 'runner' 스크립트를 사용한 경우 해당 프로세스를 디버깅한다. **Auto Attach Smart Pattern** 설정(`debug.javascript.autoAttachSmartPattern`)을 사용해 허용할 'runner' 스크립트 목록을 설정할 수 있다.
- `always` - 통합 터미널에서 실행된 모든 Node.js 프로세스를 디버깅한다.
- `onlyWithFlag` - 오직 `--inspect` 또는 `--inspect-brk` 플래그와 함께 실행된 프로세스만 디버깅한다.

**Auto Attach** 활성화 후에는 터미널을 재시작 해야한다. 터미널 재시작은 터미널 우상단 ⚠ 아이콘을 클릭하거나 새 터미널을 추가하는 것으로 할 수 있다. 재시작후 1초 이내에 디버거가 프로그램에 연결된다.

![auto-attach](/assets/img/nodejs/auto-attach.gif)

**Auto Attach** 이 켜졌으면 VS Code 윈도우 하단에 위치한 상태바에 **Auto Attach** 항목이 나타난다. 이걸 클릭해 **Auto Attach** 모드를 변경하거나 임시로 기능을 비활성화 시킬 수 있다. 임시 비활성화 기능은 디버깅이 필요없는 일회성 프로그램 실행시 유용하다.

### Additional Configuration

**다른 launch 설정 속성**

일반적으로 `launch.json`에 사용되는 속성은 `debug.javascript.terminalOptions` 설정을 사용해 Auto Attach에 적용할 수 있다. node 내부코드(node internals)을 `skipFiles`에 추가하려면 다음 코드를 user 또는 workspace 세팅에 추가하자.

```json
    "debug.javascript.terminalOptions": {
        "skipFiles": [
            "<node_internals>/**"
        ]
    },
```

**Auto Attach Smart Patterns**

Auto Attach `smart` 모드에서 VS Code는 디버거를 디버깅이 필요없는 빌드 툴에 연결하지 않고 코드에만 연결하려 노력한다. 이는 glob 패턴 목록과 메인 스크립트를 매칭하는 것으로 동작한다. 이 glob 패턴은 `debug.javascript.autoAttachSmartPattern` 에서 설정 가능하며 기본 값은 아래와 같다.

```json
[
    "!**/node_modules/**", // node_modules 폴더 아래 모든 스크립트는 제외하라
    "**/$KNOWN_TOOLS$/**" // 그러나 일부 툴은 포함하라
];
```

`$KNOWN_TOOLS$`는 `ts-node`, `mocha`, `ava` 등의 일반적인 'code runner' 목록으로 대체된다. 이 설정이 적합하지 않다면 이 'code runner' 목록을 수정할 수 있다. 예를 들어, `mocha`를 제외하고 `my-cool-test-runner`를 포함한다면 아래와 같이 두 줄을 추가한다.

```json
[
  "!**/node_modules/**",
  "**/$KNOWN_TOOLS$/**",
  "!**/node_modules/mocha/**", // "!" 을 사용해 node modules 하위 "mocha"안 모든 스크립트를 제외
  "**/node_modules/my-cool-test-runner/**" // 커스텀 test runner안 모든 스크립트를 포함
]
```

## JavaScript Debug Terminal

auto attach와 유사한 방식으로 자바스크립트 디버그 터미널(JavaScript Debug Terminal)은 해당 터미널에서 실행한 모든 Node.js 프로세스를 자동으로 디버그한다. 디버그 터미널은 Command Palette에서 **Debug: JavaScript Debug Terminal** 명령을 실행 하거나 터미널 전환 드롭다운에서 **JavaScript Debug Terminal**을 선택해 생성할 수 있다.

![create-debug-terminal](/assets/img/nodejs/create-debug-terminal.png)

### Addtional Configuration

**Other Launch Configuration Properties**

일반적으로 `launch.json`에 사용되는 속성은 `debug.javascript.terminalOptions` 설정을 사용해 디버그 터미널에 적용할 수 있다. node 내부코드(node internals)을 `skipFiles`에 추가하려면 다음 코드를 user 또는 workspace 세팅에 추가하자.

```json
"debug.javascript.terminalOptions": {
    "skipFiles": [
        "<node_internals>/**"
    ]
},
```

## Launch Configuration

launch 설정은 VS Code에서 디버깅을 설정하는 전통적인 방식으로 복잡한 애플리케이션 실행을 위한 대부분의 설정 옵션을 제공한다.

이 섹션에선 고급 디버깅을 위한 기능과 설정들에 대해 자세히 다룰것이다. 소스맵으로 디버깅하기, 외부 코드 넘어가기, 원격 디버깅 하기 등에 대해서 배운다.

## Launch configuration attributes

디버깅 설정은 워크스페이스의 `.vscode` 폴더안 `launch.json` 파일에 저장된다.

아래는 Node.js 디버거와 관련된 `launch.json` 속성 목록이다.

다음 속성들은 launch 설정의 request type이 `launch` 와 `attach` 두 경우 모두에서 사용가능하다.

- `outFiles` - 생성된 자바스크립트 파일 위치를 지정하는 glob 패턴 배열
- `resolveSourceMapLocations` - 소스맵이 파싱되어야 하는 위치에 대한 glob 패턴 배열
- `timeout` - 세션을 재시작할 때, 지정한 시간(밀리초)이 지나면 포기
- `stopOnEntry` - 프로그램 시작되면 즉시 중단
- `localRoot` - VS Code의 루트 디렉터리
- `remoteRoot` - Node의 루트 디렉터리
- `smartStep` - 소스 파일에 매핑되지 않는 코드를 자동으로 넘어가기
- `skipFiles` - 지정한 glob 패턴에 포함된 파일 자동으로 건너뛰기
- `trace` - 진단(diagnostic) 아웃풋 활성화

다음 속성들은 launch 설정의 request type이 `launch` 인 경우에만 사용가능하다.

- `program` - 디버깅할 Node.js 프로그램의 절대 경로
- `args` - 디버깅할 프로그램에 넘길 인수. 이 속성은 배열 형식이며 배열 요소가 각 인수다.
- `cwd` - 디버그할 프로그램이 시작되는 디렉터리
- `runtimeExecutable` - 사용할 런타임 실행 파일의 절대 경로. 기본값은 `node`
- `runtimeArgs` - 런타임 실행 파일에 넘길 선택적 인수
- `runtimeVersion` - Node.js 버전을 관리하는 데 "nvm"(또는 "nvm-windows") 또는 "nvs"가 사용되는 경우 이 특성을 사용하여 Node.js의 특정 버전을 선택할 수 있다.
- `env` - 환경 변수(선택). 문자열 키/값 쌍의 목록으로 환경 변수 지정
- `envFile` - 환경 변수 설정을 포함한 파일의 경로(선택)
- `console` - 프로그램을 시작하기위한 콘솔(`internalConsole`, `integratedTerminal`, `externalTerminal`)
- `outputCapture` - `std`로 설정하면 디버깅 포트를 통해 출력을 수신하는 대신 stdout/stderr 프로세스의 출력이 디버그 콘솔에 표시된다. 이는 `console.*` API대신 stdout/stderr 스트림에 직접 작성하는 log 라이브러리 또는 프로그램에 유용하다.

다음 속성들은 launch 설정의 request type이 `attach` 인 경우에만 사용가능하다.

- `restart` - 종료 시 연결을 다시 시작
- `port` - 사용할 디버그 포트
- `address` - 디버그 포트의 TCP/IP 주소
- `processId` - 디버거는 USR1 신호를 보낸 후 이 프로세스에 연결한다. 이 설정을 사용하면 디버거는 디버그 모드에서 시작되지 않은 이미 실행 중인 프로세스에 연결할 수 있다. `processId` 속성을 사용할 때 디버그 포트는 Node.js 버전(및 사용된 프로토콜)을 기반으로 자동으로 결정되며 명시적으로 구성할 수 없다. 따라서 `port` 속성을 지정하지 않는다.
- `continueOnAttach` - 디버거를 프로세스에 연결할 때 프로세스가 일시 중지된 경우 프로세스를 계속할지 여부를 지정. 이 옵션은 `--inspect-brk`로 프로그램을 시작할 때 유용하다.

### Launch configurations for common scenarios

`launch.json` 파일에서 IntelliSense(`Ctrl+Space`)를 트리거하여 일반적으로 사용되는 Node.js 디버깅 시나리오에 대한 launch 설정 스니펫을 볼 수 있다.

![launch-snippets](/assets/img/nodejs/launch-snippets.png)

구성 **Add Configuration...** 버튼을 사용하여 스니펫을 불러올 수도 있다. `launch.json` 편집기 창의 오른쪽 하단에 위치한 버튼이다.

![add-configuration-button](/assets/img/nodejs/add-configuration-button.png)

다음은 사용 가능한 스니펫이다.

- **Launch Program**: 디버그 모드에서 Node.js 프로그램을 시작한다.
- **Launch via npm**: npm의 'debug' 스크립트로 Node.js 프로그램 시작. package.json에 debug 스크립트를 정의했다면 이를 launch 설정에 직접 사용할 수 있다. npm 스크립트에서 사용되는 debug 포트가 스니펫에 지정된 포트와 일치하는지 확인하자.
- **Attach**: 로컬에서 실행중인 Node.js 프로그램의 debug 포트에 연결한다. 디버그할 Node.js 프로그램이 디버그 모드에서 시작되었고 사용된 디버그 포트가 스니펫에 지정된 포트와 일치하는지 확인하자.

---
title: Debugging
author: SAM
date: 2023-01-03 18:32:00 -0500
categories: [Blogging, Tutorial]
tags: [google analytics, pageviews]
imgPath: /assets/img
---

- [Debugging](#debugging)
  - [Debugger extensions](#debugger-extensions)
  - [Start debugging](#start-debugging)
  - [Run and Debug view](#run-and-debug-view)
  - [Run menu](#run-menu)
  - [Launch configurations](#launch-configurations)
  - [Launch versus attach configurations](#launch-versus-attach-configurations)
  - [Add a new configuration](#add-a-new-configuration)
  - [Debug actions](#debug-actions)
    - [Run mode](#run-mode)
  - [Breakpoints](#breakpoints)
  - [Logpoints](#logpoints)
  - [Data inspection](#data-inspection)
  - [Launch.json attributes](#launchjson-attributes)
  - [Variable substitution](#variable-substitution)
  - [Platform-specific properties](#platform-specific-properties)
  - [Global launch configuration](#global-launch-configuration)
  - [Advanced breakpoint topics](#advanced-breakpoint-topics)
    - [Conditional breakpoints](#conditional-breakpoints)

# Debugging

VS Code의 주요 기능 중 하나는 뛰어난 디버깅 지원이다. VS Code의 내장 디버거는 편집, 컴파일 및 디버그 루프의 능률을 높인다.

![debugging_hero]({{ page.imgPath }}/debugging/debugging_hero.png)

## Debugger extensions

VS Code는 Node.js 런타임에 대한 디버깅 지원을 내장하고 있으며 JavaScript, TypeScript 또는 JavaScript로 변환된 다른 언어를 디버깅할 수 있다.

다른 언어와 런타임(PHP, Ruby, Go, C#, Python, C++, PowerShell 등)을 디버깅하려면 VS Code Marketplace에서 디버거 확장기능을 찾거나 최상위 메뉴에서 **Run** -> **Install Additional Debuggers**를 선택하자.

다음은 디버깅 지원을 포함하는 대표적인 확장기능들 이다.

![debugger extensions]({{ page.imgPath }}/debugging/ext.png)

## Start debugging

다음 문서는 기본 제공하는 Node.js 디버거를 기반으로 하지만 대부분의 개념과 기능은 다른 디버거에도 적용할 수 있다.

## Run and Debug view

**Run and Debug** 뷰를 표시하려면 VS Code 측면의 작업 표시줄(Activity Bar)에서 **Run and Debug** 아이콘을 선택한다. 바로 가기 키 `Ctrl+Shift+D`를 사용할 수도 있다.

![run]({{ page.imgPath }}/debugging/run.png)

**Run and Debug** 뷰에는 실행 및 디버깅과 관련된 모든 정보가 표시되며 디버깅 명령 및 설정이 있는 상단 표시줄이 있다.

**Run and Debug**이 아직 설정되지 않은 경우(`launch.json`이 생성되지 않은 경우) VS Code는 아래와 같은 Run start 뷰를 표시한다.

![debug-start]({{ page.imgPath }}/debugging/debug-start.png)

## Run menu

최상위 메뉴 **Run**에는 가장 일반적인 실행 및 디버그 명령이 있다.

![debug-menu]({{ page.imgPath }}/debugging/debug-menu.png)

## Launch configurations

VS Code에서 간단한 앱을 실행하거나 디버깅하려면 Debug start 뷰에서 **Run and Debug**를 선택하거나 `F5`를 눌러 VS Code가 현재 활성 파일을 실행하도록 한다.

그러나 대부분의 디버깅 시나리오에서 launch 설정 파일을 만드는 것이 디버깅 설정 세부 사항을 설정하고 저장할 수 있기 때문에 유용하다. VS Code는 워크스페이스(프로젝트 루트 폴더)의 `.vscode` 폴더에 있는 `launch.json` 파일에 디버깅 설정 정보를 보관한다.

`launch.json`파일을 만들려면 Run start 뷰에서 **create a launch.json file** 링크를 클릭한다.

![launch-configuration]({{ page.imgPath }}/debugging/launch-configuration.png)

VS Code는 디버그 환경을 자동으로 감지하려고 하지만, 이 작업이 실패할 경우 수동으로 선택해야 한다.

![debug-environments]({{ page.imgPath }}/debugging/debug-environments.png)

다음은 Node.js 디버깅을 위해 생성된 launch 설정이다.

    {
        "version": "0.2.0",
        "configurations": [
            {
                "type": "node",
                "request": "launch",
                "name": "Launch Program",
                "skipFiles": ["<node_internals>/**"],
                "program": "${workspaceFolder}\\app.js"
            }
        ]
    }

파일 탐색기(Ctrl+Shift+E)를 확인해보면, VS Code가 `.vscode` 폴더를 만들고 `launch.json` 파일을 추가한 것을 볼 수 있다.

![launch-json-in-explorer]({{ page.imgPath }}/debugging/launch-json-in-explorer.png)

> **Note** : VS Code에서 폴더를 열지 않은 경우에도 간단한 애플리케이션을 디버깅할 수 있지만 launch 설정을 관리하고 고급 디버깅을 설정할 수 없다. 열려 있는 폴더가 없는 경우 VS Code 상태 표시줄은 보라색이다.

launch 설정에서 사용할 수 있는 속성은 디버거마다 다르다. IntelliSense 제안(`Ctrl+Space`)을 사용하여 특정 디버거에 사용가능한 속성을 확인할 수 있다. 호버 도움말은 모든 속성에서 사용할 수 있다.

한 디버거에서 사용할 수 있는 속성이 다른 디버거에서도 당연히 작동할 것이라 가정하지 말자. launch 설정에 녹색 꼬불꼬불한 선이 표시되면 호버 도움말로 문제가 무엇인지 확인하고 디버그 세션을 시작하기 전에 수정하자.

![launch-json-intellisense]({{ page.imgPath }}/debugging/launch-json-intellisense.png)

자동으로 생성된 모든 값을 검토하고 해당 값이 프로젝트 및 디버깅 환경에 적합한지 확인하자.

## Launch versus attach configurations

VS Code에는 두 가지 핵심 디버깅 모드인 Launch와 Attach가 있으며, 두 가지 각각 다른 워크플로우와 개발자 세그먼트를 처리한다. 워크플로우에 따라 어떤 디버깅 모드가 프로젝트에 적합할지 혼란스러울 수 있다.

브라우저 Developer Tools을 사용하는 경우 브라우저 인스턴스가 이미 열려 있기 때문에 "도구에서 실행"이 어색할 수 있다. DevTools가 열려있다면, DevTools를 열려있는 브라우저 탭에 연결(**attach**)하기만 하면 된다. 반면 서버나 데스크톱 백그라운드를 사용하는 경우에는 에디터가 프로세스를 실행(**launch**)하도록 하고 에디터가 자동으로 새로 실행된 프로세스에 에디터의 디버거를 연결하는 것이 일반적이다.

정리하자면, **launch** 설정은 VS Code가 앱에 연결되기 이전에 앱을 디버그 모드에서 시작하는 방법에 대한 레시피다. 반면 **attach** 설정은 이미 실행중인 앱 또는 프로세스에 VS Code의 디버거를 연결하는 방법에 대한 레시피다.

## Add a new configuration

기존 `launch.json`에 새 설정을 추가하려면 다음 방법 중 하나를 사용한다.

- 커서가 설정 배열 안에 있는 경우 IntelliSense를 사용
- 배열 시작 부분에서 **Add Configuration**버튼을 눌러 스니펫 IntelliSense를 호출
- Run 메뉴에서 **Add Configuration** 옵션 선택

![add-config]({{ page.imgPath }}/debugging/add-config.gif)

VS Code는 동시에 여러 설정을 시작할 수 있는 복합 launch 설정도 지원한다.

디버그 세션을 시작하려면 먼저 **Run and Debug** 뷰에서 **Configuration dropdown**을 사용해 **Launch Program**이라는 설정을 선택한다. launch 설정을 선택했으면 `F5`로 디버그 세션을 시작한다.

또는 **Command Palette**(`Ctrl+Shift+P`)에 **Debug: Select and Start Debugging** 또는 `debug `를 입력하고 사용할 디버그 설정을 선택하여 디버그 세션을 시작할 수도 있다.

디버깅 세션이 시작되자마자 **DEBUG CONSOLE** 패널이 표시되고 디버깅 출력이 표시되며 상태 표시줄의 색상이 변경된다(기본 색상 테마의 경우 주황색).

![debug-session]({{ page.imgPath }}/debugging/debug-session.png)

또한 상태 표시줄에 **debug status**가 나타나 활성화된 디버그 설정을 표시한다. debug status를 선택하면 **Run and Debug**뷰를 열지 않고도 launch 설정을 변경하고 디버깅을 시작할 수 있다.

![debug-status.png]({{ page.imgPath }}/debugging/debug-status.png)

## Debug actions

디버그 세션이 시작되면 에디터 상단에 **Debug toolbar**가 나타난다.

![toolbar]({{ page.imgPath }}/debugging/toolbar.png)

| Action                       | Explanation                                                                                                                                      |
| :--------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Continue / Pause <br> `F5`   | **Continue**: 다음 breakpoint까지 정상적인 프로그램/스크립트 실행을 재개 <br> **Pause**: 현재 라인에서 실행 중인 코드를 검사하고 라인별로 디버그 |
| Step Over <br> `F10`         | 구성요소 단계를 검사하거나 따르지 않고 단일 명령으로 다음 메서드를 실행                                                                          |
| Step Into <br> `F11`         | 실행을 한 줄씩 처리하기 위해 다음 메서드 실행                                                                                                    |
| Step Out <br> `Shift+F11`    | 메서드 또는 서브루틴 안에 있을 때는 단일 명령어인 것처럼 현재 메소드의 나머지 행을 완료하여 이전 실행 컨텍스트로 돌아간다                        |
| Restart <br> `Ctrl+Shift+F5` | 현재 프로그램 실행을 종료하고 현재 실행 설정을 사용하여 디버깅을 다시 시작                                                                       |
| Stop <br> `Shift+F5`         | 현재 프로그램 실행을 종료                                                                                                                        |

> **Tip**: `debug.toolBarLocation` 설정을 사용하여 디버그 툴바의 위치를 제어한다. 에디터 위에 떠 있게하거나 (`floating`, 기본값), **Run and Debug**뷰에 도킹(`docked`) 또는 숨길수(`hidden`) 있다. `floating`로 설정한 디버그 툴바는 드래그하여 수평으로 이동 하거나 에디터 영역까지 내릴 수 있다.

### Run mode

VS Code는 프로그램 디버깅 외에도 프로그램 실행을 지원한다. **Debug: Run (Start Without Debugging)** 액션은 `Ctrl+F5`로 트리거되며 현재 선택한 launch 설정을 사용한다. 대부분의 launch 설정 속성은 '실행(Run)' 모드에서 지원된다. VS Code는 프로그램이 실행되는 동안 디버그 세션을 유지하며 **Stop** 버튼을 누르면 프로그램을 종료한다.

> **Tip**: 이 실행(Run) 액션은 항상 사용할 수 있지만 모든 디버거 확장앱이 '실행(Run)'을 지원하지는 않는다. 이 경우 '실행(Run)'은 '디버그'와 동일하다.

## Breakpoints

breakpoint는 에디터 여백을 클릭하거나 현재 라인에서 `F9`를 눌러 켜고 끌 수 있다. **Run and Debug** 뷰의 **BREAKPOINTS** 섹션에서 breakpoint 제어(활성화/비활성화/재적용)를 수행할 수 있다.

- 에디터 여백의 breakpoint는 일반적으로 빨간색 원으로 표시된다.
- 비활성화된 breakpoint는 회색 원으로 표시된다.
- 디버깅 세션이 시작되면 디버거에 등록할 수 없는 breakpoint가 회색 빈 원으로 바뀐다. 라이브 편집 지원이 없는 디버그 세션이 실행되는 동안 소스를 편집한 경우에도 동일한 현상이 발생할 수 있다.

디버거가 다른 종류의 오류나 예외에 대한 중단(breaking)을 지원하는 경우, 이러한 오류나 예외는 BREAKPOINTS 뷰에서 확인할 수 있다.

**Reapply All Breakpoints** 명령은 모든 breakpoint을 원래 위치로 다시 설정합니다. 이것은 디버그 환경이 느려 아직 실행되지 않은 소스 코드의 breakpoint를 잘못 배치하는 경우에 유용하다.

![breakpoints.png]({{ page.imgPath }}/debugging/breakpoints.png)

`debug.showBreakpointsInOverviewRuler`설정을 활성화하여 에디터의 우측 개요 눈금자에 breakpoint를 표시할 수 있다.

![bpts-in-overview.png]({{ page.imgPath }}/debugging/bpts-in-overview.png)

## Logpoints

로그포인트는 디버거를 중단하지 않고 메시지를 콘솔에 기록하는 breakpoint의 변형이다. 로그포인트는 일시 중지 또는 중지할 수 없는 프로덕션 서버를 디버깅하는 로그를 남기는 데 특히 유용하다.

로그포인트는 다이아몬드 아이콘으로 표시된다. 로그 메시지는 플레인 텍스트이지만 컬리 브레이스(`{}`)에 표현식을 포함할 수 있다.

![log-points.gif]({{ page.imgPath }}/debugging/log-points.gif)

일반 breakpoint와 마찬가지로 로그포인트를 활성화하거나 비활성화할 수 있으며 `condition and/or hit count`를 통해 제어할 수도 있다.

> **Note**: 로그포인트는 VS Code의 내장 Node.js 디버거에서 지원되지만 다른 디버그 확장앱을 통해서도 구현할 수 있다. 예를 들어 Python 및 Java 확장앱은 로그포인트를 지원한다.

## Data inspection

변수는 **Run and Debug** 뷰의 **VARIABLES** 섹션이나 에디터에서 해당 소스 위에 마우스를 올려 검사할 수 있습니다. 변수의 값과 표현식 평가는 **CALL STACK** 섹션에서 선택한 스택 프레임에 기준하여 출력된다.

![variables.png](/assets/img/debugging/variables.png)

해당 변수의 컨텍스트 메뉴의 **Set Value** 액션으로 변수 값을 수정할 수 있다. 또한 **Copy Value** 액션으로 변수의 값을 복사하거나 **Copy as Expression** 액션을 사용하여 해당 변수에 액세스하는 식을 복사할 수 있다(변수의 경우 식별자가 복사됨).

변수와 식은 **Run and Debug** 뷰의 **WATCH** 섹션에서 평가하고 확인할 수 있다.

![watch.png](/assets/img/debugging/watch.png)

**VARIABLES** 섹션에 포커스를 두고 입력하면 변수 이름과 값을 필터링할 수 있다.

![filtering-variables.png](/assets/img/debugging/filtering-variables.png)

## Launch.json attributes

다양한 디버거 및 디버깅 시나리오를 지원하기 위해 많은 `launch.json` 속성이 있다. 위에서 설명한 것처럼 `type` 속성에 값을 지정한 후 IntelliSense(`Ctrl+Space`)를 사용하여 사용 가능한 속성 목록을 볼 수 있다.

![launch-json-suggestions.png](/assets/img/debugging/launch-json-suggestions.png)

다음 속성은 모든 launch 설정에 들어가는 필수 속성이다.

- `type` - launch 설정에 사용할 디버거 타입. 설치된 모든 디버그 확장앱은 내장된 Node 디버거를 위한 `type: node`를 사용하거나 또는 PHP 확장앱이면 `type: php` , Go 확장앱이면 `type: go`를 사용한다.
- `request` - launch 설정의 요청 타입. 현재, `launch`와 `attach`를 지원함.
- `name` - 디버그 launch 설정 드롭다운에 표시되는 이름을 설정함.

다음 속성은 선택 속성이며 모든 launch 설정에 사용가능하다.

- `presentation` - 객체를 할당하며, 객체의 `order`, `group`, `hidden` 속성을 사용해 디버그 설정 드롭다운과 디버그 빠른 선택(quick pick)에 표시되는 설정 및 구성물을 정렬, 그룹화 및 숨길 수 있다.
- `preLaunchTask` - 디버그 세션 시작 전에 task를 시작하려면 이 속성의 값을 `tasks.json`(워크스페이스 `.vscode` 폴더안에 있는)에서 지정한 task의 레이블로 설정한다. 또는 기본 빌드 작업을 수행하도록 `${defaultBuildTask}`로 설정할 수 있다.
- `postDebugTask` - 디버그 세션 맨 끝에 task를 시작하려면 이 속성의 값을 `tasks.json`(워크스페이스 `.vscode` 폴더안에 있는)에서 지정한 task의 레이블로 설정한다.
- `internalConsoleOptions` - 이 속성은 디버깅 세션 중에 디버그 콘솔 패널 표시 여부를 제어한다.
- `debugServer` - **디버그 확장앱 작성자 전용**: 이 속성을 사용하면 디버그 어댑터를 시작하는 대신 지정된 포트에 연결할 수 있다.
- `serverReadyAction` - 디버깅 중인 프로그램이 디버그 콘솔 또는 통합 터미널에 특정 메시지를 출력할 때마다 웹 브라우저에서 URL을 열고자 하는 경우 사용.

많은 디버거는 다음 속성 중 일부를 지원한다.

- `program` - 디버거를 시작할 때 실행할 파일
- `args` - 디버깅할 프로그램에 전달할 인수
- `env` - 환경 변수 (`null`값은 변수를 "정의 해제(undefine)" 하는데 사용될 수 있다)
- `envFile` - 환경 변수가 있는 dotenv 파일 경로
- `cwd` - 종속성 및 다른 파일을 찾기 위한 현재 작업 디렉터리
- `port` - 실행 중인 프로세스에 연결할 때 포트
- `stopOnEntry` - 프로그램이 시작되면 즉시 중단된다.
- `console` - 어떤 콘솔을 사용할지 지정. 예를 들어 `internalConsole`, `integratedTerminal`, `externalTerminal` 등.

## Variable substitution

VS Code는 일반적으로 사용되는 경로 및 값을 변수로 만들고 `launch.json`에서 문자열내 변수를 삽입할 수 있는 변수 치환(variable substitution)을 지원한다. 따라서 디버그 설정에서 절대 경로를 사용할 필요가 없다. 예를 들어 `${workspaceFolder}`는 워크플레이스 폴더의 루트 경로를 제공하고, `${file}`은 활성된 에디터에 열려 있는 파일, `${env:Name}`은 환경 변수 'Name'의 값을 제공한다. 미리 정의된 변수의 전체 목록을 보려면 [Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)을 확인하거나, `launch.json` 문자열 속성 내에서 IntelliSense를 호출하면 볼 수 있다.

    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceFolder}/app.js",
        "cwd": "${workspaceFolder}",
        "args": ["${env:USERNAME}"]
    }

## Platform-specific properties

`Launch.json`은 디버거가 실행되는 플랫폼(운영체제)에 따라 달라지는 값(예: 프로그램에 전달될 인수)을 정의하는 것을 지원한다. 그렇게 하려면 플랫폼별 리터럴을 `launch.json` 파일에 넣고 해당 플랫폼 리터럴 내에서 속성을 지정하면 된다.

다음은 Windows인 경우만 `"args"` 값을 다르게 전달하는 예시이다.

    {
        "version": "0.2.0",
        "configurations": [
            {
                "type": "node",
                "request": "launch",
                "name": "Launch Program",
                "program": "${workspaceFolder}/node_modules/gulp/bin/gulpfile.js",
                "args": ["myFolder/path/app.js"],
                "windows": {
                    "args": ["myFolder\\path\\app.js"]
                }
            }
        ]
    }

속성에 할당 가능한 값은 `"windows"`, `"linux"`, macOS용 `"osx"`이다. 플랫폼별 범위에 정의된 속성은 전역 범위에 정의된 속성보다 높은 우선권을 갖는다.

원격 디버깅 시나리오에서 `type` 속성이 플랫폼을 간접적으로 결정하고 순환 종속성을 초래하기 때문에 플랫폼별 섹션 내에 `type` 속성을 배치할 수 없다.

아래 예제에서 프로그램 디버깅은 macOS를 제외하고는 항상 엔트리에서 중지(**stops on entry**)된다.

    {
        "version": "0.2.0",
        "configurations": [
            {
                "type": "node",
                "request": "launch",
                "name": "Launch Program",
                "program": "${workspaceFolder}/node_modules/gulp/bin/gulpfile.js",
                "stopOnEntry": true,
                "osx": {
                    "stopOnEntry": false
                }
            }
        ]
    }

## Global launch configuration

User settings 내에 `"launch"` 객체를 추가할 수 있다. 그러면 이 `"launch"` 설정이 각 작업영역 전체에 공유된다.

    "launch": {
        "version": "0.2.0",
        "configurations": [{
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${file}"
        }]
    }

## Advanced breakpoint topics

### Conditional breakpoints

표현식, 히트 카운트 또는 이 둘의 조합을 기반하여 디버깅에 조건을 설정할 수 있다.

- **표현식 조건**: breakpoint에 도달했을 때 지정한 표현식이 `true`로 평가될 때만 중단한다.
- **히트 카운트**: 히트 카운트는 breakpoint가 실행을 중단하기 전에 breakpoint에 총 몇 번을 도달(hit)해야 하는지 제어한다. 히트 카운트 처리여부와 표현식의 정확한 구문은 디버거 확장앱마다 다르다.

breakpoint 조건 추가는 breakpoint를 추가 할 때 추가 하거나(**Add Conditional Breakpoint** 액션) 또는 이미 존재하는 breakpoint를 수정하여(**Edit Condition** 액션) 추가할 수 있다. 어느 방식이든 표현식(Expression), 히트 카운트(Hit count)를 선택하고 입력하는 인라인 텍스트 박스가 열리고 그 곳에 원하는 조건을 입력하면 된다.

![hitCount.gif]({{ page.imgPath }}/debugging/hitCount.gif)

**function**과 **excption**의 breakpoint에 대해서도 히트 카운트 및 표현식 조건 수정이 지원된다. 컨택스트 메뉴 또는 새로운 인라인 **Edit Condition** 액션을 통해 조건을 수정할 수 있다.

![breakpoints.gif]({{ page.imgPath }}/debugging/breakpoints.gif)

디비거가 조건부 breakpoint를 지원하지 않으면 **Add Conditional Breakpoint**와 **Edit Condition** 액션은 무시된다.

### Inline breakpoints

실행이 인라인 breakpoint과 연결된 열에 도달할 때만 인라인 breakpoint이 동작한다. 이 기능은 단일 행에 여러 개의 문을 포함한 최소화된(minified) 코드를 디버깅할 때 특히 유용하다.

`Shift+F9`를 사용하거나 디버그 세션 중에 컨텍스트 메뉴로 인라인 breakpoint을 설정할 수 있다. 인라인 breakpoint은 에디터에 인라인으로 표시된다.

인라인 breakpoint도 조건을 포함할 수 있다. 에디터 왼쪽 마진의 컨텍스트 메뉴로 한 행에 있는 여러 breakpoint를 편집할 수 있다.

### Function breakpoints

소스 코드에 breakpoint를 직접 지정하는 대신, 디버거는 함수 이름을 지정하여 breakpoint를 만드는 것을 지원할 수 있다. 이것은 소스를 사용할 수 없지만 함수 이름을 아는 상황에서 유용하다.

**BREAKPOINTS** 섹션 헤더에서 + 버튼을 누르고 함수 이름을 입력하면 함수 breakpoint가 생성된다. 함수 breakpoint는 **BREAKPOINTS** 섹션에 빨간색 삼각형으로 표시된다.

### Data breakpoints

디버거가 데이터 breakpoint를 지원하는 경우 **VARIABLES**뷰 컨텍스트 메뉴에서 데이터 breakpoint를 설정할 수 있다. **Break on Value Change/Read/Access** 명령은 지정한 변수의 값이 변경/읽기/액세스될 때 실행되는 데이터 breakpoint를 추가한다. 데이터 breakpoint는 **BREAKPOINTS** 섹션에서 빨간색 육각형으로 표시된다.

## Debug Console REPL

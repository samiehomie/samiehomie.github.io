---
title: Basic Editing in VS Code
author: sam
date: 2022-11-10 18:32:00 -0500
categories: [VS Code, USER GUIDE]
tags: [VS Code]
imgp: /assets/img/basic/
---

VS Code의 기본 사항에 대해 알아본다.

## Multiple selections (multi-cursor)

VS Code는 빠른 동시 편집을 위한 멀티플 커서를 지원한다. `Alt+Click`을 사용하여 두 번째 커서(얇게 렌더링됨)를 추가할 수 있다. 각 커서는 위치한 컨텍스트에 따라 독립적으로 작동한다. `Ctrl+Alt+Down`은 커서를 아래로 추가하고 `Ctrl+Alt+Up`은 커서를 위로 추가한다.

> **Note**: 그래픽 카드 드라이버(예: NVIDIA)가 이러한 기본 단축키를 덮어쓸 수 있다.

![multicursor.gif]({{ page.imgp }}multicursor.gif)

`Ctrl+D`는 커서가 위치한 단어를 선택하고, 단어가 선택된 상태에서 다시 누르면 다른 위치에 있는 현재 선택된 단어와 같은 단어를 선택한다.

![multicursor-word.gif]({{ page.imgp }}multicursor-word.gif)

> **Tip**: `Ctrl+Shift+L` 를 사용하면 `Ctrl+D` 를 여러번 누르는것과 같이 선택된 단어와 같은 단어를 한 번에 모두 선택한다.

### Shrink/expand selection

단축키 `Shift+Alt+Left`와 `Shift+Alt+Right`를 사용하여 현재 선택 영역을 빠르게 축소하거나 확장한다.

아래 이미지는 `Shift+Alt+Right` 으로 선택 영역을 확장하는 예시다.

![expandselection.gif]({{ page.imgp }}expandselection.gif)

## Column (box) selection

커서를 한쪽 모서리에 놓고 `Shift+Alt`를 누른 상태에서 반대쪽 모서리로 끌면 열 단위로 선택된다.

![column-select.gif]({{ page.imgp }}column-select.gif)

## Save / Auto Save

기본적으로 VS Code에서 변경 사항을 디스크에 저장하려면 직접 `Ctrl+S`를 눌러야 한다.

그러나 자동 저장(`Auto Save`)을 활성화 하여 설정된 딜레이 타임이 지나거나 포커스가 에디터에서 떠나면 바로 자동 저장되도록 할 수 있다. 즉, 이 옵션을 켜면 파일을 직접 저장할 필요가 없다. **File > Auto Save**을 체크하여 딜레이 타임 후 자동 저장을 활성화 할 수 있다. 다시 눌러 체크가 해제되면 자동 저장 기능이 비활성화 된다.

자동 저장에 대한 디테일한 제어를 하려면 user 또는 workspace settings를 열고 아래의 옵션을 설정한다.

- `files.autoSave`: 아래의 값들을 할당할 수 있다.
  - `off` - 자동 저장 비활성화
  - `afterDelay` - 설정된 딜레이 타임 이후 자동 저장(기본 1000 ms)
  - `onFocusChange` - 작성 중인 파일의 에디터에서 포커스가 떠나 이동하면 자동 저장
  - `onWindowChange` - VS Code의 윈도우 밖으로 포커스가 떠나면 자동 저장
- `files.autoSaveDelay`: `files.autoSave`의 값으로 `afterDelay`가 할당되었을 때, 딜레이 타임을 밀리초 단위로 지정한다. 기본값은 1000ms다.

## Hot Exit

기본적으로 VS Code는 종료될 때 파일의 저장되지 않은 변경 사항을 기억한다(저장이 아닌 '기억'이며, 이를 Hot exit, 무중단 종료라 함). Hot exit은 애플리케이션이 **File > Exit**(MacOS에서 Code > Quit)으로 닫히거나 마지막 창이 닫힐 때 트리거된다.

Hot exit는 `files.hotExit` 설정을 통해 컨트롤 할 수 있으며 다음 값을 할당할 수 있다.

- `off`: Hot exit 비활성화
- `onExit`: Hot exit는 애필리케이션이 닫힐 때, 즉 윈도우/리눅스에서 마지막 창이 닫히거나 `workbench.action.quit` 명령이 트리거 될 때 트리거된다. 폴더를 열지 않은 모든 창은 다음 실행시 복원된다.

## Find and Replace

현재 열려 있는 파일에서 빠르게 텍스트를 찾고 대체할 수 있다. `Ctrl+F`를 눌러 에디터에서 찾기 위젯을 열면 검색 결과가 에디터, 개요 눈금자, 미니맵에서 강조 표시된다.

현재 열려 있는 파일에 일치하는 결과가 둘 이상 있는 경우 `Enter` 와 `Shift+Enter` 를 눌러 다음 또는 이전 결과로 이동할 수 있다. 다만, 이때 포커스는 찾기 입력 상자에 있어야 한다.

### Seed Search String From Selection

찾기 위젯을 열면 에디터에서 선택한 텍스트가 찾기 입력 상자에 자동으로 들어가있다. 선택 항목이 비어 있으면 커서 아래의 단어가 대신 찾기 입력 상자에 들어간다.

![seed-search-string-from-selection.gif]({{ page.imgp }}seed-search-string-from-selection.gif)

이 기능은 `editor.find.seedSearchStringFromSelection` 설정에 `false`를 할당하여 끌 수 있다.

### Find In Selection

기본적으로 찾기 작업은 찾기 위젯을 연 파일의 전체 영역에서 실행된다. 전체가 아닌 선택한 범위로 한정하여 찾기 작업을 실행할 수도 있다. 찾고자 하는 영역을 선택하고 찾기 위젯에서 햄버거 아이콘을 클릭하여 이 기능을 설정할 수 있다.

![find-in-selection.gif]({{ page.imgp }}find-in-selection.gif)

선택 영역에서 찾기를 찾기 위젯의 기본 동작으로 사용하려면 `editor.find.autoFindInSelection`에 `always` 또는 `multiline`을 할당한다. `multiline`으로 설정하면 선택한 영역이 여러 줄인 경우에만 선택 영역에서 찾기가 실행된다.

### Advanced find and replace options

찾기 위젯에는 일반 텍스트를 찾아서 대체하는 것 외에도 세 가지 고급 검색 옵션이 있다.

- **Match Case**: 대소문자 무시
- **Match Whole Word**: 단어 전체가 맞아야 매칭
- **Regular Expression**: 정규표현식 사용

교체 입력 박스는 케이스 보존을 지원하며, 케이스 보존(**AB**) 버튼을 클릭하여 해당 기능을 켤 수 있다.

### Multiline support and Find Widget resizing

`Ctrl+Enter`를 누르면 입력 상자에서 개행이 되고 새 줄에 텍스트를 입력할 수 있다.

![multiple-line-support.gif]({{ page.imgp }}multiple-line-support.gif)

긴 텍스트를 검색하기에 찾기 위젯의 기본 크기가 너무 작을 수 있다. 왼쪽 새시를 끌어서 위젯을 확대하거나 왼쪽 새시를 두 번 클릭하여 위젯을 최대화하거나 기본 크기로 축소할 수 있다.

![resize-find-widget.gif]({{ page.imgp }}resize-find-widget.gif)

## Search across files

현재 열려 있는 폴더의 모든 파일 대상으로 빠르게 검색할 수 있다. `Ctrl+Shift+F`를 누르고 검색어를 입력한다. 검색 결과는 검색어가 포함된 파일로 그룹화되며, 각 파일의 검색어 매칭 횟수와 위치가 표시된다. 파일로 그룹화된 검색 결과를 확장하면 해당 파일 내의 모든 검색어 매칭 결과를 미리 볼 수 있다. 여기서 검색 매칭 미리보기를 클릭하면 에디터에서 해당 라인을 바로 보여준다.

![search.png]({{ page.imgp }}search.png)

> **Tip**: 정규 표현식을 사용한 검색도 가능하다.

검색 상자 오른쪽 아래에 있는 줄임표(**Toggle Search Details**)를 클릭하거나 `Ctrl+Shift+J`를 눌러 고급 검색 옵션을 설정할 수 있다. 검색을 설정하기 위한 추가 필드가 표시된다.

### Advanced search options

![searchadvanced.png]({{ page.imgp }}searchadvanced.png)

검색 상자 아래의 두 입력 상자에서 검색에 포함하거나 제외할 패턴을 입력할 수 있다. `example`을 입력하면 워크스페이스에서 `example`이라는 이름의 모든 폴더 및 파일과 매치된다. `./example`을 입력하면 워크스페이스 루트에 있는 폴더 `example/`과 매치된다. 여러 패턴을 구분하려면 `,`를 사용한다. 경로는 슬래시를 사용해야 한다. glob 패턴을 사용할 수도 있다.

- `*` 경로 세그먼트(경로 구분자 /로 나눠지는 경로의 부분을 말함)안 0개 이상의 문자와 매칭. 경로 세그먼트'안' 이므로 경로 구분자인 /는 매칭 안됨.
- `?` 경로 세그먼트안 1개 문자와 매칭
- `**` 아예 없는 것을 포함하여 모든 경로 세그먼트와 매칭('' 부터 './../../' 등 모두 매칭)
- `{}` 여러 조건을 그룹화. 예를 들어 `{**/*.html,**/*.txt}`는 모든 HTML과 text 파일에 매칭된다.
- `[]` 매칭한 문자열의 범위를 지정함. `example.[0-9]`는 `example.0`, `example.1`과 매칭됨.
- `[!...]` 지정한 문자열 범위에 포함된 것을 제외하고 매칭함. `example.[!0-9]`은 `example.a`, `example.b`와 매칭되고 `example.0`와는 매칭되지 않는다.

VS Code는 기본값으로 일부 폴더를 제외하여 검색 결과의 수를 줄인다(예: `node_modules`). 설정의 `files.exclude` 와 `search.exclude` 속성을 수정하여 이러한 규칙을 변경할 수 있다.

검색 뷰의 glob 패턴은 설정의 `files.exclude` 및 `search.exclude`의 것과는 다르게 작동한다. 설정에선 `**/example`을 사용하여 워크스페이스의 하위 폴더 `folder1/example`에 있는 `example`폴더와 매칭한다. 반면 검색 뷰에선 `**` 접두사가 이미 있는것으로 상정한다.

검색 뷰의 **files to exclude** 박스안 **Use Exclude Settings and Ignore Files** 토근 버튼을 확인하자. 이 토글 버튼은 `.gitignore`파일 그리고 설정의 `files.exclude`, `search.exclude` 에서 제외한 파일들을 제외할지 여부를 결정한다.

![Use-Exclude-Settings-and-Ignore-File]({{ page.imgp }}toggle.png)

> **Tip**: 탐색기에서 폴더를 마우스 오른쪽 클릭하고 **Find in Folder**를 선택하여 해당 폴더 내부만 검색할 수 있다.

### Search and replace

여러 파일을 검색하고 다른 단어로 대체할 수 있다. 검색 위젯을 확장하여 바꾸기(Replace) 텍스트 박스가 보이게 한다.

![global-search-replace.png]({{ page.imgp }}global-search-replace.png)

바꾸기 텍스트 박스에 텍스트를 입력하면 변경할 내용과 원본의 diff가 표시된다. 바꾸기 텍스트 박스에서 모든 파일을 바꾸거나, 한 파일만을 바꾸거나, 한 파일의 하나의 변경사항만을 바꿀 수 있다.

![search-replace-example.png]({{ page.imgp }}search-replace-example.png)

> **Tip**: 아래,위 방향키를 사용하여 검색어 기록을 탐색하여 이전 검색어를 빠르게 재사용할 수 있다.

### Case changing in regex replace

에디터 또는 전역에서 검색 및 바꾸기를 수행하면서 정규식 매칭 그룹의 대소문자 변경을 할 수 있다. 이는 수식어 `\u\U\l\L`로 이루어지며, 여기서 `\u`와 `\l`은 단일 문자를 대문자/소문자로, `\U`와 `\L`은 매칭 그룹의 나머지(또는 모든) 문자를 대문자/소문자로 변경한다.

![case-change-replace.gif]({{ page.imgp }}case-change-replace.gif)

예를 들어 `\u\u\u$1`은 그룹의 처음 세 문자를 대문자로 만들고 `\l\U$1`은 첫 번째 문자를 소문자로 만들고 나머지 문자는 대문자로 만든다. 캡처 그룹은 대체 문자열에서 `$n`으로 참조된다. 여기서 `n`은 캡처 그룹의 순서다.

## Search Editor

검색 에디터를 사용하면 구문 강조 표시 및 검색어 주변 행이 포함된 전체 크기의 에디터로 워크스페이스 검색 결과를 볼 수 있다.

다음은 일치 단어 전후에 두 줄의 텍스트가 있는 'SearchEditor' 단어를 검색한 예시다.

![search-editor-overview.png]({{ page.imgp }}search-editor-overview.png)

## Formatting


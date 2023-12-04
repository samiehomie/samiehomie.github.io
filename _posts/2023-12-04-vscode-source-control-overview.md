---
title: Overview
author: sam
date: 2022-12-04 11:32:00 -0500
categories: [VS Code, SOURCE CONTROL]
tags: [VS Code, Source Control]
imgp: /assets/img/basic/
---

# Using GIt source control in VS Code

Visual Studio Code는 통합된 소스 컨트롤 운영시스템(SCM)과 높은 수준의 Git 지원 기능이 포함되어 있다. Git 외에도 많은 소스 컨트롤 제공자를 VS Code 마켓플레이스의 확장 기능을 통해 사용할 수 있다.

## Working in a Git repository

![git.png]({{ page.imgp }}git.png)

좌측 활성바의 소스 컨트롤 아이콘은 항상 Git 저장소에서 현재 몇 개의 변화가 있는지에 대한 개요를 나타낸다. 소스 컨트롤 아이콘을 클릭하면 저장소의 변경점을 다음 세 가지 섹션으로 나눠 보여준다: **CHANGES**, **STAGED CHANGES**, **MERGE CHANGES**

각 항목을 클릭하면 각 파일의 변경된 내용이 상세히 표시된다. 아직 스테이징되지 않은 변경사항 이라면(unstaged changes, CHANGE 섹션), 에디터 오른쪽 화면에서 수정이 가능하다.

VS Code 하단 좌측에는 저장소의 상태를 나타내는 아이콘이 있다. 현재 브랜치, 현재 브랜치의 들어오고 나가는 커밋의 숫자, dirty indicator가 표시된다. 이 상태 표시자를 클릭하면 나타나는 `Git ref` 목록을 선택하면 해당 브랜치로 `checkout` 된다.

.gitignore 등으로 git 에서 제외처리된 파일은 음영 처리된다.

![git-ignore.png]({{ page.imgp }}git-ignore.png)
test 폴더와 그 하위 파일은 .gitignore로 제외처러 되어 흐릿하게 표시된다.

## Cloning a repository

아직 폴더를 열지 않은 상태에서 소스 컨트롤 메뉴로 들어가면 로컬 폴더를 여는 Open Folder와 Clone Repository 두 개의 버튼이 보인다.

![git-clone.png]({{ page.imgp }}git-clone.png)

Clone Repository를 선택하면 Github와 같은 원격 저장소의 URL과 로컬 저장소를 생성할 부모 디렉터리를 묻는다. 완료하면 원격 저장소의 복사 버전이 로컬 저장소로 생성된다.

## Remotes

저장소가 원격 저장소에 연결되어 있고 check out한 브랜치가 원격 저장소의 브랜치에 연결된 상태(upstream link)면, VS Code는 해당 브랜치를 push, pull, sync(pull 명령을 실행하고 뒤이어 push 명령을 수행함)하는 유용한 기능을 제공한다. 이 기능은 Source Control 메뉴 상단 우측의 … 모양의 Views and More Actions 메뉴에서 찾을 수 있다. 이 메뉴에 원격 저장소 추가 삭제 기능도 함께 있다.

VS Code는 원격 저장소에서 주기적으로 변경 사항을 가져올 수 있다. 이를 통해 VS Code는 로컬 저장소가 원격 저장소보다 앞서거나 뒤쳐져 있는 변경사항이 몇 개인지 보여준다. 이 기능은 기본적으로 비활성화 되어 있으며 `git.autofetch` 설정을 통해 활성화 시킬 수 있다.

> Tip : VS Code가 원격 저장소에 요청할 때마다 자격 인증을 요청받고 싶지 않다면, 자격증명 헬퍼(credential helper, GitHub CLI를 설치하고 `gh auth login` 통해 로그인하는 것을 의미)를 세팅해야 한다.

## Git Status Bar actions

현재 check out한 브랜치에 연결된 원격 브랜치가 있다면 하단 상태바 브랜치 표시자 옆에 Synchronize Changes 기능이 활성화 된다. Synchronize Changes는 원격 저장소에서 변경 사항을 pull 하고 그 다음 로컬 커밋을 원격 브랜치에 push한다.

![git-status.png]({{ page.imgp }}git-status.png)

만약 원격 저장소와 연결된 원격 저장소 브랜치 설정이 없는 상태면, Synchronize Changes 대신 Publish 기능이 활성화 된다. 이 기능을 사용하면 현재 브랜치를 원격 저장소에 발행한다.

![git-status2.png]({{ page.imgp }}git-status2.png)

## Gutter indicators

프로젝트에 Git이 적용되어 있는 상태에서 코드를 수정하면 VS Code는 유용한 주석을 거터(gutter)와 눈금자에 표시한다.

- 빨간 삼각형은 라인이 삭제된 곳을 가리킨다.
- 녹색바는 새로 추가된 라인을 가리킨다.
- 파란바는 수정된 라인을 가리킨다.

![git-indicator.png]({{ page.imgp }}git-indicator.png)

## Merge conflicts 

![git-merge.png]({{ page.imgp }}git-merge.png)

머지 충돌이 발생하면 VS Code가 이를 인지한다. 이미지에서 보듯 차이점은 강조되고 둘 중 하나 또는 둘 모드를 적용하는 인라인 액션이 존재한다. 충돌이 처리된 후에는 파일은 Staging Area에 추가되어 커밋이 가능하다.

## 3-way merge editor

머지 충돌을 처리하기 위해 VS Code는 대화형으로 incoming changes와 current changes를 적용할 수 있고 그 결과 생성되는 머지 파일을 확인하고 수정할 수 있는 3-way 머지 에디터를 제공한다. 이 3-way 머지 에디터는 머지 충돌이 발생한 파일의 오른쪽 아래에 있는 Resolve in Merge Editor 버튼을 클릭하여 열 수 있다.

3-way 머지 에디터는 Incoming changes(왼쪽)과 current changes(오른쪽)을 분리된 창에 각각 띄운다. 충돌된 영역이 강조되고 강조 영역 위에 보이는 버튼을 사용하여 충돌을 해결할 수 있다.

![git-merge2.png]({{ page.imgp }}git-merge2.png)

### Resolving conflicts

3-way 머지 에디터는 변경 사항 모두를 선택하거나 둘 중 하나를 택하는 방식으로 충돌을 해결할 수 있게 한다. 또는 머지된 결과 파일을 직접 수정하는 것도 가능하다.

일부 충돌 케이스에서 Accept Combination 버튼이 활성화 된다. 이 버튼을 사용하면 충돌이 발생한 코드를 모두 사용해 합치는 방식으로 충돌을 해결한다. 이 기능은 특히 동일한 문자를 건드리지 않는 동일한 라인의 변경 사항 끼리의 충돌 해결에 유용하다.

Ignore 버튼을 사용하면 incoming, current 변경 사항을 적용하지 않으면서 충돌이 해결된 것으로 표시할 수 있다. 이렇게 하면 충돌 영역이 변경되기 전 상태로 재설정된다.

### Completing the merge

머지 에디터의 result 에티터(하단 화면) 쪽에 아직 처리되지 않고 남아있는 충돌 갯수가 표시된다. 이 숫자를 클릭하면 다음 처리되지 않은 충돌로 이동한다. 모든 충돌이 해결되면, 오른쪽 아래에 있는 Complete Merge 버튼을 눌러 머지를 완료할 수 있다. 머지된 파일은 Staging Area에 추가되고 머지 에디터가 종료된다.

### Alternative layouts and more

머지 에디터의 분리된 3개의 화면 각각 상단에 있는 세 개의 점(…)을 클릭하면 추가 옵션을 선택할 수 있는 컨텍스트 메뉴가 뜬다. 변경사항을 전부 적용하거나, 각 변경사항을 3-way 머지의 공통 커밋, 즉 base와 비교할 수도 있다. 머지의 결과가 표시되는 result 화면의 세 개의 점을 클릭하면 적용된 사항을 모두 되돌리는 reset 기능이 뜬다.

분리된 3개 화면 말고 머지 에디터 전체창 우측 상단에도 세 개의 점 메뉴가 있다. 공통 커밋(base)를 표시하게 하거나 화면 정렬을 수직으로 하는 등의 옵션을 제공한다.

## Viewing diffs

![git-diff.png]({{ page.imgp }}git-diff.png)

파일간 차이 비교(diff)는 File Explorer 또는 OPEN EDITORS 목록에서 첫 파일을 오른쪽 클릭하고 Select for Compare를 선택한 후, 첫 파일과 비교할 두 번째 파일을 오른쪽 클릭하고 Compare with Selected 를 선택하여 실행한다. 다른 방법으론 Ctrl + Shift + P 로 명령 입력창을 열고 File: Compare Active File With … 를 선택해 diff를 할 수도 있다.

### Diff editor review pane

Diff 편집기에는 통합 패치 형식으로 변경 사항을 표시하는 검토 창이 있다. 다음 차이로 이동(F7)과 이전 차이로 이동(Shift+F7)을 사용하여 변경 내용을 탐색할 수 있다. 화살표 키를 사용하여 라인을 탐색할 수 있으며 Enter 키를 누르면 Diff 편집기로 돌아온다.

![git-review.png]({{ page.imgp }}git-review.png)

## Timeline View

File Explorer 하단에 있는 타임라인 뷰는 파일에 대한 이벤트를(예: Git 커밋, 저장) 시간순으로 시각화해서 보여준다.

![git-timeline.png]({{ page.imgp }}git-timeline.png)

VS Code의 내장 Git 지원기능은 지정된 파일의 Git 커밋 기록을 제공한다. 커밋을 선택하면 해당 커밋에 의해 도입된 변경 사항에 대한 diff 창이 열린다. 커밋을 오른쪽 클릭하면 Copy Commit ID와 Copy Commit Message를 선택할 수 있다.

## VS Code as Git editor

CLI에서 VS Code를 실행시킬때 `--wait` 인수를 넘겨 실행 명령이 VS Code 창을 닫을 때까지 대기하도록 할 수 있다. 이 기능은 VS Code를 Git 외부 편집기로 설정하고 Git이 실행된 VS Code가 닫힐때까지 대기하도록 할 때 유용하다(VS Code로 입력받고 VS Code가 닫히면 Git 명령이 수행).

VS Code를 Git 에디터로 설정하려면 다음 과정을 따른다:

1. `code —help` 명령을 CLI에서 실행해본다. 도움 메시지가 확인이 안된다면 code라는 명령어에 VS Code가 연결되어 있지 않은 상태이므로, VS Code를 다시 설치하면서 설치 과정중 Add to PATH 옵션을 선택하자.
2. CLI에 `git config --global core.editor “code --wait”` 명령을 입력한다.

이제 `git config --global -e`를 입력하면 Git 전역 설정파일인 .gitconfig 파일이 VS Code에서 열리고 수정할 수 있다.

### VS Code as Git difftool and mergetool

VS Code 안에서 뿐만 아니라 CLI에서 Git을 사용할 때도 VS Code의 diff와 머지 기능을 사용할 수 있다. 다음 설정값을 Git 설정 파일에 입력하여 VS Code를 diff와 머지 툴로 설정할 수 있다.

```bash
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --diff $LOCAL $REMOTE
[merge]
    tool = code
[mergetool "code"]
    cmd = code --wait --merge $REMOTE $LOCAL $BASE $MERGED
```

이렇게 설정함으로써 `--diff` 옵션을 사용하면 VS Code에서 두 파일을 나란히 비교한다. VS Code의 머지 툴은 Git이 머지 충돌을 발견했을 때 실행된다.

다음은 VS Code가 Git 에디터로 실행되는 대표적인 사례다.

- `git rebase HEAD~3 -i `: VS Code를 사용해 대화형 rebase를 처리한다.
- `git commit` : 커밋 메시지 작성을 VS Code로 처리한다.
- `git add -p` : 해당 명력을 입력하고 이어지는 질문에서 `e`를 선택하면 VS Code가 열린다.
- `git difftool <commit>^ <commit>` : diff 툴로 VS Code가 실행된다.
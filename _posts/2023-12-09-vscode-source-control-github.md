---
title: GitHub
author: sam
date: 2022-12-09 16:32:00 -0500
categories: [VS Code, SOURCE CONTROL]
tags: [VS Code, Source Control, Git, GitHub]
imgp: /assets/img/git/
---

# Working with GitHub in VS Code

GitHub는 소스 코드를 공유하고 저장하는 클라우드 기반 서비스다. VS Code에서 GitHub를 사용함으로써 소스 코드를 공유하고 다른 사람과 협업이 가능해진다.

VS Code에서 GitHub를 시작하기 위해 Git 설치와 GitHub 계정 생성을하고 **GitHub Pull Requests and Issues** 확장앱을 설치해야 한다. 이 토픽에서 우리는 VS Code 내에서 GitHub의 핵심 기능을 어떻게 사용할 수 있는지 알아볼 것이다.

## Setting up a repository

### cloning a repository

Command Palette (Ctrl + Shift + P)에서 **Git: Clone** 명령을 입력해 GitHub에서 저장소를 탐색하고 복사할 수 있다. 또는 Source Control 창에서 **Clone Repository** 버튼을 눌러 복사할 수도 있다(이 방법은 VS Code에서 어떤 폴더도 열지 않은 경우에 가능하다).

![git-clone.png]({{ page.imgp }}git-clone.png)

GitHub 저장소 드롭다운 리스트에서 로컬에 복사할 저장소를 선택한다.

![git-drop.png]({{ page.imgp }}git-drop.png)

### Authenticating with an existing repository

GiHub를 통한 인증확인은 GitHub 인증이 필요한 Git 동작을 VS Code에서 실행하고자 할 때 발생한다. 예를 들어 멤버로 있는 저장소에 push를 하거나 private 저장소를 복사하려는 경우가 이에 속한다. 이 인증을 위해서 별도의 확장앱 설치는 필요없다. GitHub 인증 기능은 사용자가 효율적으로 저장소를 관리할 수 있게 하고자 VS Code에 내장되어 있다.

GitHub 인증을 필요로하는 무언갈 하게되며, 다음 같은 sign in 프롬프트를 보게된다.

![git-auth.png]({{ page.imgp }}git-auth.png)

안내하는 과정을 따라 GitHub에 로그인하고 VS Code로 돌아오자. 기존 저장소에 인증하는 것이 자동으로 동작하지 않는다면, 직접 개인 액세스 토큰을 제공해야 될 것이다.

GitHub에 인증하는 방법에는 유저네임, 패스워드를 통한 인증, 개인 액세스 토큰 사용, SSH 키 사용을 포함해 몇 가지가 존재한다.

만일 로컬 머신에 복제하지 않고 원격 저장소를 수정하길 원한다면, GitHub Repositories 확장앱을 설치해 GitHub 저장소를 직접 수정하고 탐색할 수도 있다.


## Editor integration

### Hover

오픈한 저장소가 있고 `@유저명`으로 특정 유저명을 언급하고 있는 주석이 있다면, 유저이름에 마우스를 올리는 순간 GitHub 스타일의 유저 정보를 보여주는 팝업 레이어가 보일 것이다.

![git-user.png]({{ page.imgp }}git-user.png)

비슷한 기능으로 `#이슈넘버`에 마우스를 올리면 해당 이슈에 대한 상세한 정보가 역시 팝업 레이어로 표시된다.

![git-issue.png]({{ page.imgp }}git-issue.png)

### Suggestions

`“@”` 문자가 있으면 유저 제안 컨텍스트 메뉴가 뜨고 “#” 문자가 있으면 이슈 제안 컨텍스트 메뉴가 뜬다. 이러한 제안 기능은 코드 인라인과 Source Control 창의 인풋 박스에서 활성화 된다.

제안에서 보이는 issue는 GitHub Issues: Queries (githubIssues.queries) 옵션을 통해 설정할 수 있다. 이 쿼리는 GitHub search syntax를 사용한다.

그 외에도 GitHub Issues: Ignore Completion Trigger (githubIssues.ignoreCompletionTrigger) 과 GitHub Issues: Ignore User Completion Trigger(githubIssues.ignoreUserCompletionTrigger) 옵션 세팅을 통해 어떤 파일이 이슈/유저 제안을 보여줄지 설정할 수 있다. 이 설정 옵션들은 값으로 파일 타입을 지정하는 언어 식별자(python, javascript 등 문자열)의 배열을 받는다.

```json
// python은 '#' 문자를 이슈 완성 제안의 트리거로 사용하지 않는다.
"githubIssues.ignoreCompletionTrigger": [
  "python"
]
```

## Pull requests

Pull Requests 창에서 pull requests를 생성, 관리, 확인할 수 있다.

![git-pr.png]({{ page.imgp }}git-pr.png)

pull requests를 표시하는데 사용되는 query는 GItHub Pull Requests: Queries (githubPullRequests.queries) 설정 옵션에서 설정할 수 있으며 GitHub search syntax를 사용한다.

```json
"githubPullRequests.queries": [
    {
        "label": "Assigned To Me",
        "query": "is:open assignee:${user}"
    },
```

### Creating Pull Requests

변경 사항을 branch 또는 fork에 커밋하면 GitHub Pull Requests: Create Pull Request 명령 또는 Pull Requests 창에서 Create Pull Request 버튼을 사용해 pull requests을 생성할 수 있다.

![git-create.png]({{ page.imgp }}git-create.png)

Create Pull Request 버튼을 누르면 PR에 대한 제목, 설명을 입력하고, PR이 초안인지 아닌지를 선택하며 PR을 요청할 타겟 branch와 저장소를 선택할 수 있는 Create Pull Request 창이 생성된다. 저장소가 PR 템플릿을 갖고 있다면 이는 설명란(description)에 자동으로 적용된다.

![git-form.png]({{ page.imgp }}git-form.png)

Create 버튼을 클릭하면 GitHub 원격 branch에 로컬 branch를 push하기 전인 경우, branch를 push할 것인지와 어떤 원격 branch에 push 할 것인지를 먼저 확인한다.

이제 Create Pull Request 창은 Review Mode로 진입한다. Review Mode에서는 PR의 상세 내용을 확인하고 라벨, 리뷰어, 코멘트를 추가하는 작업을 할 수 있다. 모든 준비가 마쳤다고 판단되면 역시 이 창에서 merge를 선택해 merge를 진행할 수 있다.

PR이 머지된 이후엔 메인 branch에 합쳐짐으로써 필요 없게된 로컬과 원격 branch 모두를 삭제할 수 있게된다.

### Reviewing

PR은 Pull Requests 창에서 리뷰할 수 있다. pull requests **Description**에서 리뷰어와 라벨을 할당하고 코멘트 추가, 승인, 닫기, 머지 등 모든 작업을 처리할 수 있다.

![git-desc.png]({{ page.imgp }}git-desc.png)

이 Description 페이지에서 Checkout 버튼을 클릭해 손쉽게 로컬 PR branch로 checkout할 수 있다. 이렇게 하면 VS Code가 전환되어 Review Mode에서 PR의 fork와 branch(상태 표시줄에 표시됨)를 열고 현재 변경 사항뿐만 아니라 모든 커밋 및 이러한 커밋 내 변경 사항을 볼 수 있는 새로운 **Changes in Pull Request**를 추가한다. 코멘트가 달린 파일은 다이아몬드 아이콘으로 표시된다. 파일을 열어 보려면 **Open File** 인라인 액션을 사용하면 된다.

![git-open.png]({{ page.imgp }}git-open.png)

**Changes in Pull Request** 창의 diff 에디터는 로컬 파일을 사용하므로 파일 탐색, IntelliSense 및 편집이 정상 작동한다. diff에 대해 에디터 안에서 코멘트를 추가할 수 있다.

![git-comment.png]({{ page.imgp }}git-comment.png)

단일 코멘트 추가와 전체 검토 작성이 모두 지원된다.

PR 리뷰를 마쳤으면 PR을 머지하거나 Exit Review Mode를 선택하여 작업하던 이전 branch로 돌아갈 수 있다.

## Issues

### creating issues

Issue는 Issues 화면(GitHub 확장 화면 하단에 있음)에서 + 버튼을 누르거나 GitHub Issues: Create Issue from Selection 그리고 GitHub Issues: Create Issue from Clipboard 명령으로 생성할 수 있다. 또는 “TODO” 주석을 통한 코드 액션으로도 생성할 수 있다. issue를 생성하면서 기본 설명을 그대로 사용하거나 상단 연필 아이콘(Edit description)을 클릭하여 issue 설명을 수정할 수 있는 에디터를 불러올 수 있다. 

![git-create-issue.png]({{ page.imgp }}git-create-issue.png)

**GitHub Issues: Create Issue Triggers** (`githubIssues.createIssueTriggers`) 옵션을 사용해 issue를 생성하는 코드 액션 트리거를 설정할 수 있다.

이슈 트리거의 기본 설정은 아래와 같다.

```json
"githubIssues.createIssueTriggers": [
  "TODO",
  "todo",
  "BUG",
  "FIXME",
  "ISSUE",
  "HACK"
]
```

### working on issues

Issues 화면에서 이슈를 확인하고 이슈에 대한 작업을 진행할 수 있다.

![git-issue-window.png]({{ page.imgp }}git-issue-window.png)

이슈를 오른족 클릭하면 나오는 컨텍스트 메뉴의 Start Working on Issue를 선택해 해당 이슈에 대한 처리를 시작하면 새로운 브랜치가 생성되고 그 브랜치로 checkout된다. 하단 상태바에서 이를 확인할 수 있다.

![git-status.png]({{ page.imgp }}git-status.png)

상태바에는 어떤 이슈에 대한 브런치인지도 표시된다(상단 이미지 Issue #4362). 상태바에서 이 이슈를 클릭하면 pull request 생성, 이슈 깃허브 웹사이트에서 열기와 같은 실행가능한 이슈 액션 리스트가 제시된다. 

![git-branch.png]({{ page.imgp }}git-branch.png)

이슈를 통해 생성된 브랜치 이름은 **GitHub Issues: Branch for Issues**(`githubIssues.issueBranchTitle`) 옵션을 통해 설정할 수 있다. 만일 현재 작업방식에 브랜치 생성이 수반되지 않길 바라거나, 브랜치 생성시 매번 직접 브랜치명을 입력하길 바란다면 **GitHub Issues: Use Branch For Issues** (`githubIssues.useBranchForIssues`) 옵션을 설정하면 된다.

이슈에 대한 처리를 마치고 변경 사항을 커밋할 때 Source Control 창의 커밋 메시지 입력창이 이슈관련 메시지로 채워진 것을 볼 수 있다. 이 동작은 **GitHub Issues: Working Issue Format SCM** (`githubIssues.workingIssueFormatScm`)으로 설정할 수 있다.

## GitHub Repositories extension

GitHub Repositories 확장앱을 사용하면 원격 저장소를 로컬에 복사하지 않고도 VS Code에서 GitHub 저장소를 열어 직접 수정하고 커밋할 수 있다. 이 기능은 파일 또는 에셋을 간단히 수정하고 확인만 하려는 경우에 매우 유용하다. 

![git-ext.png]({{ page.imgp }}git-ext.png)

### opening a repository

GitHub Repositories 확장앱 설치후 Command Palette(Ctrl + Shift + P)에서 GitHub Repositories: Open Repository … 명령을 입력하거나 하단 상태바 왼쪽 끝에 있는 원격 저장소 표시자를 클릭하여 저장소를 오픈할 수 있다.

![git-repo.png]({{ page.imgp }}git-repo.png)

Open Repository 명령을 실행하면, GitHub에서 저장소를 열지, Pull Request를 열지 또는, 이전에 연결했던 저장소를 다시 열지를 선택한다.

VS Code에 GitHub 로그인이 되어있지 않다면, GitHub 어카운트 인증 처리를 진행해야 한다.

![git-remote-open.png]({{ page.imgp }}git-remote-open.png)

저장소 URL을 입력해도 되고 텍스트 박스에 원하는 저장소 이름을 입력하여 탐색해도 된다.

저장소 또는 Pull Request를 선택하면 VS Code 윈도우가 리로드되고 File Explorer에 저장소 컨텐츠 보이게된다. 이제 로컬에 복사된 저장소에서 작업하는 것과 같이 괄호 매칭, 문법 강조기능 지원을 받으며 파일을 열어 수정하고 커밋할 수 있다.

로컬 저장소에서 작업하는 것과 하나 다른점은 GitHub Repository 확장앱을 통해 변경사항을 커밋하면 별도로 push를 하지 않아도 곧바로 원격 저장소에 push가 된다는 것이다. GitHub 웹에서 작업하고 커밋하는것과 유사하다.

GitHub Repositories 확장앱의 또 다른 기능은 저장소 또는 branch를 오픈할 때마다 GitHub에서 사용가능한 최신 소스를 가져온다는 것이다. 즉, 로컬 저장소에서 하듯 최신화를 위해 pull을 할 필요가 없다는 것이다.

GitHub Repositories 확장앱은 로컬에 별도로 Git LFS(Large File System)을 설치하지 않아도 LFS-tracked 파일 커밋과 탐색을 지원한다, `.gitattributes` 파일에 LFS에 의해 트랙킹되길 바라는 파일 타입을 추가하자. 그리고 Source Control 화면을 사용해 GitHub에 변경사항을 곧바로 커밋하자.

### Switching branches

하단 상태바의 branch 표시자를 클릭해서 branch를 손쉽게 전환할 수 있다. GitHub Repositories 확장앱의 가장 뛰어난 기능중 하나는 커밋하지 않은 변경 사항을 stash 필요없이 branch를 전환할 수 있다는 점이다. branch가 전환되면 변경 사항을 기억해뒀다 다시 해당 branch로 돌아오면 변경사항을 재적용한다.

![git-switch.png]({{ page.imgp }}git-switch.png)

### Remote Explorer

Remote Explorer을 사용하여 원격 저장소를 빠르게 다시 열수있다. 이 창에서 이전에 어떤 저장소와 branch가 오픈되었는지 확인할 수 있다.

![git-exp.png]({{ page.imgp }}git-exp.png)

### Create Pull Requests

현재 워크플로우가 저장소에 직접 커밋하는 것보다는 Pull Requests를 사용하고 있다면, Source Control 창에서 새 PR을 생성할 수 있음을 알아두자. Create a Pull Request를 클릭하면 제목을 입력하고 새 branch를 생성하는 프롬프트가 뜬다.

![git-create2.png]({{ page.imgp }}git-create2.png)

Pull Request를 생성한 후에는 GitHub Pull Request and Issues 확장앱을 사용해 PR을 리뷰, 수정, 머지할 수 있다. 자세한 내용은 앞선 토픽에서 다뤘다.

### Virtual file system

로컬 머신에 저장소의 파일 없이 GitHub Repositories 확장앱은 메모리에 가상 파일 시스템을 생성하고 사용자는 이를 통해 파일 컨텐츠를 확인하고 수정할 수 있다. 가상 파일 시스템을 사용한다는 것은 로컬 파일임을 상정하는 일부 확장앱과 기능이 이용할 수 없거나 제한적 기능만 가능하다. tasks, 디버깅, 통합 터미널과 같은 기능은 이용할 수 없고 상태바의 원격 표시자에 마우스 올리면 보이는 features are not available 링크를 통해 가상 파일 시스템에서 이용 불가능한 기능이 무엇인지 확인할 수 있다.

![git-virtual.png]({{ page.imgp }}git-virtual.png)

### Continue Working on..

로컬 파일 시스템과 전체 언어 및 개발 도구를 지원하는 개발 환경의 저장소로 작업환경을 전환하고 싶을 때가 있다. GitHub Repositories 확장앱을 통해 이 작업을 쉽게 수행할 수 있다.

- GitHub codespace 생성 (GitHub Codespaces 확장앱이 설치 되어 있어야함)
- 저장소 로컬로 복사
- 저장소 Docker 콘테이너로 복사

개발 환경을 전환하기 위해선 Command Palette (Ctrl + Shift + P) 에 이용가능한 **Continue Working on** 작업환경 명령을 입력하거나, 하단 상태바의 원격 표시자(Remote indicator)를 클릭한다.

![git-working.png]({{ page.imgp }}git-working.png)

브라우저 기반 에디터를 사용중 이라면, Continue Working On… 명령은 저장소를 로컬로 열거나 GitHub Codespaces의 클라우드 호스팅 환경 내에서 여는 두 가지 옵션을 갖는다.

![git-select.png]({{ page.imgp }}git-select.png)

커밋되지 않은 변경 사항과 함께  Continue Working On…을 처음 사용하는 경우 VS Code 서비스를 사용하여 보류 중인 변경 사항을 저장하는 **Cloud Changes**를 사용해 선택한 개발 환경에 편집 내용을 가져올 수 있다.

이 변경사항은 대상 개발 환경에 적용되면 VS Code 서비스에서 삭제된다. 커밋되지 않은 변경 사항 없이 계속 진행하도록 선택한 경우, 이 기본 설정을 `“workbench.cloudChanges.continueOn” ; “prompt”` 설정을 통해 언제나 바꿀 수 있다.
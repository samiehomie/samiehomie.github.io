---
title: Git
author: sam
date: 2022-12-09 11:32:00 -0500
categories: [VS Code, SOURCE CONTROL]
tags: [VS Code, Source Control, Git]
imgp: /assets/img/git/
---

# Introduction to Git in VS Code

소스 코드를 손쉽게 관리하고 다른 사람들과 협업을 원한다면 Git과 GitHub는 필수적인 툴이다. 

## Open a Git repository

VS Code는 로컬부터 Github Codespaces같은 클라우드 기반 원격까지 Git 저장소에서 프로젝트를 시작할 수 있는 여러 방법을 제공한다.

### Clone a repository locally

![panel.png]({{ page.imgp }}git-panel.png)

Github에서 저장소를 `clone` 하기 위해서 **Git: Clone** 명령을 실행하거나 Source Code 창에서 **Clone Repository** 버튼을 클릭해야 한다. GitHub를 clone 하면 VS Code는 GitHub로 인증하라는 메시지를 표시한다. 이를 인증하면 사용가능한 모든 저장소를 검색할 수 있고 private 저장소를 clone할 수 있다. 다른 Git 제공자의 경우, 저장소 URL을 입력하고 Clone을 선택한 후 폴더를 선택한다. VS Code는 로컬에 저장소 복사가 완료되면 해당 폴더를 오픈한다.

### Initialize a reository in a local folder

새로운 로컬 저장소를 초기화하기 위해서는 폴더를 VS Code에서 열고 **Source Control** 창에서 **Initialize Repository** 버튼을 클릭한다. 이로서 오픈한 현재 폴더에 새 Git 저장소가 생성된다.

### Publish local repository to GitHub

로컬 Git 저장소를 생성하고나면 이제 이 로컬 저장소를 GitHub에 발행할 수 있다. 로컬 저장소를 GitHub에 발행하면, 새로운 저장소가 GitHub 계정에 생성되고 이 새 원격 저장소에는 로컬 코드가 들어와 있다. 이렇게 소스 코드를 원격 저장소에 저장하는 것은 효과적인 코드 백업 이면서 다른 사람과 협업을 가능케한다. 더불어 **GitHub Actions**를 사용하면 워크플로우 자동화도 가능하다.

![git-sc.png]({{ page.imgp }}git-sc.png)

Source Control 창에서 **Publish to GitHub** 버튼을 클릭하자. 그럼 저장소 이름과 저장소에 대한 설명 그리고 저장소를 public 또는 private으로 만들지 선택하게 된다. 저장소 생성이 완료되면 VS Code는 로컬 코드를 원격 저장소에 push 하게 된다. 코드는 이제 GitHub에 백업되고, 커밋과 pull request를 통해 다른 사람과 협업을 진행할 수 있게 되었다.

### Open a GitHub repository in a codespace

GitHub Codespaces는 GitHub 저장소를 브라우저에서 별도 소프트웨어 설치 없이 곧바로 코드를 수정할 수 있는 클라우드 기반 개발환경을 제공한다. GitHub Codespaces는 개인 용도는 무료다.

GitHub Codespaces 확장 프로그램을 VS Code에 설치하고 GitHub 로그인을 한다. 그리고 명령창에 **Codespaces: Create New Codespace** 를 입력하고 원하는 저장소와 브랜치를 선택한다. 선택한 Codespace가 새 창으로 열린다.

![git-space.png]({{ page.imgp }}git-space.png)

또는, GitHub의 Codespaces site에서 제공하는 템플릿으로 시작할 수도 있다. 만일 이미 브라우저에서 codespace를 오픈한 상태라면 **Codespaces: Open in VS Code Desktop** 명령을 입력해 VS Code에서 열리게 할 수 있다.

### Open a GitHub repository remotely

VS Code의 원격 저장소 지원기능을 활용하여 로컬 컴퓨터에 원격 저장소를 복사하지 않고 GitHub 저장소를 수정하고 탐색할 수 있다. 이 기능은 코드 전체를 로컬 머신에 복사하지 않으면서 원격 저장소를 빠르게 수정할 수 있어 유용하다.

첫째로 **GitHub Repositories** 확장 프로그램을 설치하자. 그리고 **Remote Repositories: Open Remote Repository...** 명령을 입력하거나 **Explorer** 뷰에서 **Open Remote Repository** 버튼을 클릭하면 선택할 수 있는 GitHub 저장소 목록이 뜬다.

![git-remote.png]({{ page.imgp }}git-remote.png)

> Tip : 코드 또는 터미널 명령 실행이 필요하면 **Continue Working on** 명령으로 별도에 중간 과정없이(seamlessly) 원격 저장소에서 codespace로 전환할 수 있다.

## Staging and committing code changes

Git 저장소 설치가 완료되면 새로이 수정되거나 생성된 코드의 커밋과 스테이징으로 코드 변화를 추적할 수 있게 된다.

![git-track.png]({{ page.imgp }}git-track.png)

파일을 스테이징 하기 위해선 Source Control 화면에서 파일 옆에 있는 +(plus) 아이콘을 눌러 파일을 **Staged Changes** 섹션에 추가한다. Staged Changes 섹션에 포함된다는 것은 다음 커밋에 이 파일이 포함됨을 가리킨다. 또한 파일 옆 -(minus) 아이콘을 눌러 스테이징 상태를 버리고 이전 상태로 되돌릴 수 있다.

스테이징된 변경 사항을 커밋하기 위해서 상단 텍스트 박스에 커밋 메시지를 입력하고 Commit 버튼을 누른다. 이로서 해당 변경 사항이 로컬 Git 저장소에 저장되며, 따라서 필요하면 이전 버전 코드로 되돌아가는 것이 가능해졌다. 또한 Explorer 하단에 잇는 Timeline 창이 활성화 되면서 모든 로컬 파일의 변화와 커밋을 확인할 수 있다.

![git-commit.png]({{ page.imgp }}git-commit.png)

## Pushing and pulling remote changes

로컬 저장소에 커밋을 생성했다면 이제 이것을 원격 저장소에 push 할 수 있다. Sync Changes 버튼에는 몇 개의 커밋이 push 되고 pull 될 것인지가 표시된다. Sync Changes를 클릭하면 새 원격 커밋을 다운로드(pull)하고 새 로컬 커밋을 원격 저장소에 업로드(push)한다.

![git-pull.png]({{ page.imgp }}git-pull.png)

> Tip : 설정 옵션중 **Git: Autofetch**에 true(기본 저장소), all(모든 저장소)를 할당하면 자동으로 fetch 하여 항상 트래킹 저장소(예: origin/master)가 최신 상태를 유지하도록 할 수 있다.

push, pull은 물론 각각의 명령을 통해 개별적으로 수행할 수 있다.

## Using branches

Git의 branch를 사용하여 동시에 여러 버전의 코드에서 작업을 할 수 있다. 이 기능은 새로운 기능을 실험하고 메인 코드에 영향을 주지 않으면서 대량의 코드를 수정 작업을 진행할 때 유용하다. 

![git-bar.png]({{ page.imgp }}git-bar.png)

하단 status바에 있는 branch 지표는 현재 branch를 보여주며 기존의 브랜치 또는 새 브랜치로 전환할 수 있다. 새 branch를 생성하기 위해선 branch 표시자를 선택하고 새 branch를 생성하는 방식으로 현재 브랜치 기준으로 분기하기와 다른 로컬 브랜치 기준으로 분기하기를 선택한다. 그리고 새 branch 이름을 입력하고 확인하여 branch 생성을 마친다. 새 branch가 생성되면서 작업 환경도 새 branch로 바로 이동한다. 이제 메인 branch에 영향을 주지 않으면서 코드 수정을 진행할 수 있다.

> Tip : GitHub Pull Requests and Issues 확장 프로그램을 사용하면 issue에서 곧바로 새 브랜치를 생성할 수 있다. 이를 통해 새 로컬 branch에서 작업을 시작하고 자동으로 Pull Requests를 미리 작성할 수 있다.

이 브랜치를 원격 저장소에 push 하려면 Source Control 창에서 Publish Branch를 클릭하자. 이를 통해 원격 저장소에 새 branch가 생성되고 이 브랜치에서 다른 사람들과 협업이 가능해진다.

### creating and reviewing GitHub pull requests

Git과 GitHub에서 pull requests는 동료들과 코드를 리뷰하고 각 브랜치의 코드 변경사항을 메인 브랜치에 머지하기 위한 수단이다.

VS Code에서 pull requests를 사용하기 위해선 GtiHub Pull Requests and Issues 확장 프로그램을 설치해야 한다. 이 확장 프로그램은 pull requests와 이슈 트래킹 기능을 VS Code에 추가한다. 이를 활용하여 pull requests(이하 PR)를 생성하고 리뷰하며 머지하는 작업을 VS Code 내에서 처리할 수 있다.

PR을 생성하려면 메인 브랜치와 별도 브랜치에서 추가된 코드를 원격 저장소에 push 해야한다. 그리고 Source Control 화면에서 Create Pull Requests 버튼을 클릭한다. 그러면 PR에 대한 설명과 제목을 입력할 수 있는 PR 생성 폼이 열린다. 여기서 변경 사항을 머지시킬 브랜치를 선택한다. 마지막으로 Create 버튼을 누르면 PR이 생성된다.

PR을 리뷰하기 위해선 **Source Control** 창에서 **Review Pull Request** 버튼을 누르고 리뷰하길 바라는 PR을 선택한다. 새 에디터 창에서 PR이 오픈되고 여기서 PR을 리뷰하고 코멘트를 남길 수 있다. 코드 리뷰후 머지하기로 마음 먹었다면, **Merge** 버튼을 눌러 PR을 타겟 브랜치로 머지를 진행한다.

## Using Git in the built-in terminal

모든 Git 상태가 로컬 저장소에 유지되므로 VS Code의 UI, 내장 터미널 또는 GitHub Desktop과 같은 외부 도구 간에 쉽게 전환이 가능하다. 또는 VS Code를 기본 Git 에디터로 설정해 VS Code를 커밋 메시지와 그외 Git 관련 파일 수정하는데 사용할 수 있다.

### Git Bash on Windows

Git Bash는 Git과 그외 커맨드-라인 툴을 다루기 위한 Unix-like 커맨드라인 인터페이스를 제공하는 윈도우를 위한 shell 환경이다. VS Code의 통합 터미널은 shell로서 Git Bash를 지원하며 이를 통해 개발자는 별도의 과정 없이 Git Bash를 개발 워크플로우에 통합시킬 수 있다. Git Bash는 Git 설치 과정중 관련 옵션을 끄지 않았다면 Git과 함께 설치된다.

![git-terminal.png]({{ page.imgp }}git-terminal.png)

View > Terminal (Ctrl + `) 로 터미널을 열고 새로운 shell을 열기위해 터미널 패널에 있는 + 아이콘 옆 화살표를 클릭한다. Git Bash가 설치된 상태면, 나타난 리스트 중에 Git Bash가 있을 것이다. 터미널 사이드바에서 각각의 터미널과 shell을 클릭하여 전환할 수 있다. 이렇게 VS Code에 Gti Bash가 설정되면 이제 VS Code의 터미널에서 곧바로 Git 명령을 사용할 수 있게된다.

Git Bash를 기본 shell로 설정하고 싶다면 터미널 패널의 + 아이콘 옆 드롭다운을 누르고 **Select Default Shell(Profile)**을 선택하자. 사용가능한 shell 목록이 열리고 여기서 Git Bash를 선택하면 이제 기본 shell로 Git Bash가 설정된다. 이후 터미널은 Git Bash와 함께 열린다.

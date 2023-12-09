---
title: FAQ
author: sam
date: 2022-12-09 18:32:00 -0500
categories: [VS Code, SOURCE CONTROL]
tags: [VS Code, Source Control, Git, GitHub]
imgp: /assets/img/git/
---

# Git FAQ

## Git 커밋을 되돌리거나 취소하는 방법

Git: Undo Last Commit 명령을 사용하여 마지막 커밋을 되돌릴 수 있다. 이를 통해 모든 변경 사항을 포함해 브랜치를 커밋 직전 상태로 되돌린다. 이 명령은 또한 Source Control 창 상단에 More Actions(… 모양 메뉴) 하위 Commit 메뉴에서도 선택가능하다.

## 로컬 브랜치 이름 수정하기

**Git: Rename branch...** 명령을 입력하면 새 이름을 입력받는 프롬프트가 뜬다.

## 가장 최근 커밋 메시지 수정하기

**Git: Commit Staged (Amend)** 명령을 사용해 마지막 커밋 메시지를 수정할 수 있다.
 
명령을 입력하면 마지막 커밋 메시지를 수정할 수 있는 에디터가 열리고 수정후 저장하면 커밋 메시지가 변경된다. 수정전에 해당 커밋에 포함되는 파일들에 변경사항이 없는지 확인하자.
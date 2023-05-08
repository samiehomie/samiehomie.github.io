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

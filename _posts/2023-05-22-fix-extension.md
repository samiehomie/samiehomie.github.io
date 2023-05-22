---
title: "VS Code 확장앱 오류 해결"
author: sam
date: 2023-05-22 18:32:00 -0500
categories: [개발 사전, 오류]
tags: [VS Code, prettier]
---

코드를 균일한 규칙으로 정렬해주는 포맷터(fomatter)인 **Prettier**가 아래 메시지를 띄우면서 갑자기 작동하지 않았다.

```
Extension 'prettier' is configured as formatter but it cannot format 'JavaScript'-files
```

관련하여 찾아낸 해결 방법은 전부 기본 VS Code 셋팅의 부재에서 발생하는 오류에 대한 대처여서 도움이 되지 않았다.

> **Note**: 참고로 VS Code 설정은 적용 범위(**User** 전역 세팅과 **Workspace** 프로젝트 세팅)와 사용하는 언어에 따라 적용되는 값을 달리할 수 있다. 또한, 세팅을 설정하는 방식에도 UI 설정창을 이용하거나 `settings.json`을 직접 작성하는 두 가지 옵션을 제공한다. VS Code 설정 방식에 대해선 [공식 문서](https://code.visualstudio.com/docs/getstarted/settings)를 읽어 꼭 숙지해보길 추천한다. 어렵지 않아 한번 숙지하면 prettier 설정 정도는 따로 찾아보지 않고 할 수 있을 것이다.

**결과적으론, VS Code 확장앱이 설치된 폴더를 직접 삭제한 뒤 재설치하여 해결 했다.** VS Code 확장앱은 은근히 별 이유없이 자주 프로그램이 깨진다. 이런 경우엔 **Extensions** 뷰에서 삭제, 재설치 하는 것으로는 해결이 되지 않는다. 윈도우의 경우 보통 아래 경로에 확장앱이 설치되어 있다. 문제가 있는 확장앱 폴더를 찾아 삭제하고 재설치 해보자.

```
C:\Users\사용자명\.vscode\extensions
```

참고로 확장앱 폴더명은 확장앱 **ID**와 동일하다.

![eslint-id.png](/assets/img/etc/eslint-id.png)

해당 확장앱 페이지에서 톱니바퀴 아이콘을 누르면 **Copy Extension ID** 컨텍스트 메뉴가 보이는데 이것을 클릭하면 확장앱 ID가 복사된다. 이 ID를 `C:\Users\사용자명\.vscode\extensions` 이로 이동해 검색하면 해당 확장앱이 설치된 폴더를 찾을 수 있다.

![eslint-id2.png](/assets/img/etc/eslint-id2.png)

참고로 이 ID는 `settings.json`에서 설정할 때도 사용한다.

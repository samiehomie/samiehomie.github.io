---
title: EPERM operation not permitte 오류해결
author: sam
date: 2023-05-10 18:32:00 -0500
categories: [Knowhow, Error]
tags: [npm, pnpm]
---

npm으로 패키지를 재설치 하거나 업데이트 할 때 `EPERM: operation not permitted` 오류가 발생하는 경우가 있는데, 에러 메시지로 보아 내부적으로 관리되는 심볼 중복이 발생하고 이를 처리할 권한이 없다 판단하여 오류를 던지는것 같다.

물론 추측이고 정확한 이유는 찾지 못했다. VS Code를 사용하는 나는 아래와 같은 방법으로 문제를 해결 하였다.

```bash
# npm 캐시 삭제
npm cache clean --force

# npm 최신 버전 업데이트
npm install -g npm@latest --force
```

VS Code 통합 터미널에서 진행하였으며, 문제 해결이 되지 않은 경우 별도 터미털을 관리자 권한으로 실행하여 다시 진행해보길 바란다.

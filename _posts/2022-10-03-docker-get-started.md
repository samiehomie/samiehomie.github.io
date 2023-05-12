---
title: 애플리케이션 컨테이너화하기
author: sam
date: 2022-10-03 18:32:00 -0500
categories: [Docker, Get started]
tags: [docker]
---

이 섹션의 다음 과정을 완료하려면 다음이 필요하다.

- 로컬에서 실행중인 Docker. 지시에 따라 Docker를 [다운로드(https://docs.docker.com/get-docker/)]하고 설치한다.
- Git 클라이언트
- IDE나 텍스트 에디터. VS Code를 권장한다.
- 컨테이너 및 이미지에 대한 개념적 이해

## 애플리케이션 가져오기

애플리케이션을 실행하려면 먼저 애플리케이션 소스 코드를 컴퓨터로 가져와야 한다.

1. 다음 명령으로 [getting-started](https://github.com/docker/getting-started/tree/master) 저장소를 로컬에 복사한다.
2. 복사된 저장소의 `getting-started/app` 디렉토리를 확인하면 `package.json` 파일과 `src`, `spec` 두 개의 하위 디렉토리를 확인할 수 있다.

![ide-screenshot.png](/assets/img/docker/ide-screenshot.png)

## 애플리케이션의 컨테이너 이미지 빌드

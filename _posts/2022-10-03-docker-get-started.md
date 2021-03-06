---
title: 애플리케이션 컨테이너화하기
author: sam
date: 2022-10-03 18:32:00 -0500
categories: [Docker, Get started]
tags: [docker]
---

이 섹션의 다음 과정을 완료하려면 다음이 필요하다.

- 로컬에서 실행중인 Docker. 지시에 따라 Docker를 [다운로드](https://docs.docker.com/get-docker/)하고 설치한다.
- Git 클라이언트
- IDE나 텍스트 에디터. VS Code를 권장한다.
- 컨테이너 및 이미지에 대한 개념적 이해

## 애플리케이션 가져오기

애플리케이션을 실행하려면 먼저 애플리케이션 소스 코드를 컴퓨터로 가져와야 한다.

1. 다음 명령으로 [getting-started](https://github.com/docker/getting-started/tree/master) 저장소를 로컬에 복사한다.
2. 복사된 저장소의 `getting-started/app` 디렉토리를 확인하면 `package.json` 파일과 `src`, `spec` 두 개의 하위 디렉토리를 확인할 수 있다.

![ide-screenshot.png](/assets/img/docker/ide-screenshot.png)

## 애플리케이션의 컨테이너 이미지 빌드

컨테이너 이미지를 빌드하려면 `Dockerfile`을 사용해야 한다. Dockerfile은 명령 스크립트를 포함하는 파일 확장자가 없는 텍스트 기반 파일이다. Docker는 이 스크립트를 사용하여 컨테이너 이미지를 빌드한다.

1. `package.json` 파일이 위치한 `app` 디렉터리안에 Dockerfile 파일을 만든다.
2. 코드 에디터를 사용하여 다음 내용을 Dockerfile에 입력한다.

   ```
   # syntax=docker/dockerfile:1

   FROM node:18-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   EXPOSE 3000
   ```

3. 다음 명령을 사용하여 컨테이너 이미지를 빌드한다.

   ```bash
   # cd /path/to/app
   cd app
   docker build -t getting-started .
   ```

   `docker build` 명령은 Dockerfile을 사용하여 새 컨테이너 이미지를 빌드한다. Docker가 많은 레이어를 다운로드하는데 이는 `node:18-alpine` 이미지에서 시작하기를 원한다고 빌더에게 지시했기 때문이다. 하지만, 로컬에는 해당 이미지가 없으므로 Docker는 이미지를 다운로드했다.

   Docker가 이미지를 다운로드한 후 Dockerfile의 지시문이 애플리케이션에 복사되고 `yarn`을 사용하여 애플리케이션의 종속성을 설치한다. `CMD` 지시문은 이 이미지에서 컨테이너를 시작할 때 실행할 기본 명령을 지정한다.

   마지막으로 `-t` 플래그는 이미지에 태그를 지정한다. 태그란 단순히 최종 이미지에 붙인 사람이 읽을 수 있는 이름으로 생각하자. 이미지 이름을 `getting-started`으로 지정했으므로 컨테이너를 실행할 때 해당 이미지를 참조할 수 있다.

   `docker build` 명령 끝에 있는 `.`는 Docker에게 현재 디렉터리에서 `Dockerfile`을 찾아야 함을 알려준다.

## 애플리케이션 컨테이너 시작하기

이제 이미지가 생겼으니 컨테이너에서 애플리케이션을 실행할 수 있다. `docker run` 명령을 사용한다.

1. `docker run` 명령으로 컨테이를 시작하고 생성한 이미지 이름을 지정한다.

   ```bash
   docker run -dp 3000:3000 getting-started
   ```

   `-d` 플래그를 사용하여 새 컨테이너를 분리(detached) 모드로 (백그라운드에서) 실행한다. `-p` 플래그를 사용하여 호스트의 포트 3000과 컨테이너의 포트 3000 간의 매핑을 생성할 수도 있다. 포트 매핑이 없으면 애플리케이션에 액세스할 수 없다.

2. 몇 초후에 브라우저에서 http://localhost:3000를 열어보면 애플리케이션을 확인할 수 있다.
   ![todo-list-empty.png](/assets/img/docker/todo-list-empty.png)
3. 앱에서 아이템을 추가해보며 제대로 작동하는지 확인해보자. 아이템을 완료로 표시하고 제거할 수 있다. 프런트 엔드가 백엔드에 아이템을 성공적으로 저장하고 있다.

컨테이너를 간단히 살펴보면 `getting-starte` 이미지를 사용하는 하나 이상의 컨테이너가 포트 3000에서 실행되고 있는 것을 볼 수 있다. 컨테이너를 보려면 CLI 또는 Docker Desktop의 그래픽 인터페이스를 사용할 수 있다.

`docker ps` 명령을 터미널에서 사용해 컨테이너를 나열할 수 있다.

```bash
docker ps
```

다음과 비슷한 출력값을 확인할 수 있다.

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
df784548666d        getting-started     "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        0.0.0.0:3000->3000/tcp   priceless_mcclintock
```

또는 Docker 데스크톱에서 **Containers** 탭을 선택해 컨테이터 목록을 확인할 수도 있다.

![dashboard-two-containers.png](/assets/img/docker/dashboard-two-containers.png)

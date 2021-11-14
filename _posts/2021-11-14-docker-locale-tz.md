---
layout: single
title: "Docker Mysql Timezone, 한글 문제"
category: "tech"
tags: [docker, mysql]

---

사이드 프로젝트에서 Docker를 사용해 Mysql 스키마를 넣어 두고 사용하다가 더미데이터도 Docker 이미지에 포함을 시키기로 하였습니다.

진행을 하면서 한글이 깨지는 문제, 시간이 UTC 기준으로 저장되고 보여지는 문제가 있었습니다.

이를 Docker Compse 파일의 Mysql 설정에 LC_ALL, TZ 환경변수를 아래와 같이 설정하여 해결하였습니다.

```yaml
version: "3.8"

services:
  mysql:
    image: mysql:8.0
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: root-pw
      MYSQL_USER: user
      MYSQL_PASSWORD: user-pw
      MYSQL_DATABASE: database
      LC_ALL: C.UTF-8
      TZ: Asia/Seoul
```

### Locale

보충 설명을 하면 Mysql에서 제공하는 Docker 이미지는 debian을 기초로 하고 있고 해당 이미지에서는 posix를 기본 locale로 사용합니다. posix는 기본 posix(시스템에서 locale 명령어로 확인 시 C)와 posix와 utf8을 결헙한 C.UTF-8 로케일이 있습니다. 한글이 정상적으로 입력되기 위해서는 locale을 C.UTF-8로 설정해주면 되며, LOCALE 관련 환경변수들을 한번에 설정해줄 수 있는 LC_ALL을 통해 설정해주면 됩니다.

![image-20211114233142951](/assets/images/image-20211114233142951.png)

### Timezone

Mysql에서 별도 timezone을 설정하지 않으면 SYSTEM 설정을 따라 갑니다. MySQL의 설정을 바꾸는 것보다는 SYSTEM의 설정을 환경변수로 조정해주는 것이 더 편하기 때문에 compose 파일에 TZ 환경변수를 Asia/Seoul 로 설정하는 것으로 하였습니다. 

![image-20211114234516695](/assets/images/image-20211114234516695.png)

저의 경우 DataGrip을 사용하고 있는데 Connection 설정에서 serverTimezone을 Asia/Seoul로 Mysql과 동일하게 해주어야 정상적으로 연결이 되었습니다.

![image-20211114234749150](/assets/images/image-20211114234749150.png)
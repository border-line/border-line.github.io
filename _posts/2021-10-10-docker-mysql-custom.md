---
layout: single
title: "Docker Mysql Customization"
category: "tech"
---

mysql을 docker로 사용하다보면 데이터 추가, 유저 추가 등 기본 설정을 추가한 이미지를 만들고 싶은 경우가 많습니다.

이런 경우에는 원하는 shell script 또는 sql 파일을 docker-entrypoint-initdb.d 로 복사하면 됩니다. 자세한 부분은 [Mysql Docker 공식 페이지](https://hub.docker.com/_/mysql)의 **<u>Initializing a fresh instance</u>** 을 참조하시면 됩니다.

### 디렉토리 구조

![image-20211011000150076](/assets/images/image-20211011000150076.png)

### Dockerfile

```dockerfile
FROM mysql:8.0

COPY  /docker-entrypoint-initdb.d /docker-entrypoint-initdb.d
```

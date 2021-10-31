---
layout: single
title: "Mysql on docker - Socket vs TCP"
category: "tech"
tags: [mysql, docker]


---

[docker mysql 공식 이미지](https://hub.docker.com/_/mysql)에 프로젝트에 사용되는 스키마와 더미 데이터를 추가한 이미지를 만들어 사용하고 있습니다.

처음 사용 시에 3306 포트를 container에 연결해 두었지만 Mysql Client 프로그램(dbeaver, datagrip,sequel pro 등)에서 접속할 수 없는 문제를 겪었습니다. 이를 해결한 과정과 mysql 접속 방식에 대한 것을 간단히 소개합니다.

### 1. 로컬 MySQL은 왜 떠 있었니..

해결이 오래 걸리도록 했던 가장 큰 이유는 로컬에 brew로 설치한 MySQL이 실행되고 있었기 때문입니다. docker mysql container를 `-dp3306:3306` 옵션으로 실행했고 문제 없이 실행되었기 때문에 로컬에는 mysql이 실행되어 있지 않은 것으로 판단했습니다.

그래서 아래 이미지와 같이 Acess denied가 나오는 것이 docker mysql container에서 발생한 것으로 판단하고 계정 설정 문제 쪽으로 검색하고 조치를 취했습니다.

![image-20211031122359904](/assets/images/image-20211031122359904.png)

하지만 어떤 조치를 취해도 위 에러는 계속 발생했는데 그 이유는 위 명령어는 로컬에 있는 mysql로 접속하는 명령어이기 때문입니다. 그러니 당연히 docker mysql container에 설정한 계정 정보로는 Access denied가 떨어질 수밖에 없었습니다.

로컬에 mysql이 떠 있다는 것은 어떤 조치를 해도 해결이 되지 않는다는 것이 이상해서 확인해 본 것이었데 발견해서 다행이었습니다. 

### 2. Socket vs TCP

로컬 mysql을 stop 시킨 다음에 자신 있게 동일한 명령어를 쳤습니다. 하지만 이번에는 다른 에러가 떨어졌습니다.

![image-20211031122557251](/assets/images/image-20211031122557251.png)

/tmp/mysql.sock 파일이 없다는 것인데 관련해서 조사하면서 mysql 접속 방식에는 Socket을 이용한 방식과 TCP를 이용한 방식이 있다는 것을 알게 되었습니다. Socket의 경우 동일 호스트(서버)에서 접근할 때 사용이 되며 파일 기반으로 접속이 이루어집니다. 아마 TCP에 비해 빠르다는 장점을 가지고 있을 것 같습니다. 그리고 TCP의 경우 외부에서 접근할 때 또는 명시적으로 접속 도메인 또는 IP가 지정이 될 때 사용됩니다.

한가지 더 알아야할 것은 동일 호스트 내에서 접근 시에 Socket 사용이 장려하는 것 때문인지 host 옵션을 localhost로 주어도 TCP가 아닌 Socket 방식으로 동작한다는 점입니다. 그래서 동작이 동일할 것으로 보이는 아래 두 명령어는 다른 결과를 보입니다.

- `mysql -hlocalhost -uroot -p` : Socket 방식을 이용하며 Socket 통신에 사용되는 파일이 없는 경우 에러 발생
- `mysql -h127.0.0.1 -uroot -p` : TCP 방식을 이용하며 3306 포트에 연결되어 있는 docker에 정상 접속

결론적으로는 docker로 실행한 mysql에 docker를 실행한 기기에서 접근하기 위해서는 host를 localhost가 아닌 127.0.0.1로 설정하면 됩니다.
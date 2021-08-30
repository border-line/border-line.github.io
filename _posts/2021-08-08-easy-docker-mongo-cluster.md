---
layout: single
title: "Easy Docker with MongoDB"
category: tech
tags: [docker, mongodb, db]
---

일을 하면서 Docker를 쓰면 좋겠다 싶었지만, 왠지 제대로 해야 할 것 같은 마음에 계속 미루고 있었습니다.

그러던 차에 기존 Document DB로 운영하고 있던 것을 비용감소를 위해 EC2로 옮기게 되었는데 이 과정에서 Cluster를 구성해야 할 일이 생겼습니다.

EC2에 직접 몽고DB를 설치하여 운영하고 Scale-out이 필요할 때는 미리 떠 놓은 AMI를 활용해 인스턴스를 추가하는 방법이 있습니다.

하지만.. 이렇게 진행하는 경우 정정 클러스터 구성을 찾기 위해 테스트를 진행하는 것이 시간이 정말 많이 들어 갑니다..!!
그리고 아무래도 이후에 다른 운영체제 또는 회사 내부에 있는 개발서버에 동일한 환경을 구성하기란 참 어렵습니다.

그래서 이 기회에 docker를 사용하여 실제 클러스터 생성 시에 DB, Collection, Index까지 함께 생성되도록 만들어 보기로 했습니다.

## Docker 기본 배우기 - Easy Docker

Docker를 배우는데 시간이 오래 걸릴 것으로 생각했지만 작업을 하는데 필요한 기본 지식은 Docker 공식 홈페이지에서 제공하는 Getting Started를 읽는 것으로 모두 배울 수 있었습니다.
다른 업무를 함게 하면서 반나절, 하루 정도 걸렸던 것 같습니다.

## Mongo DB Cluster

### Cluster restart issue 1 - multi service

MongoDB는 최초 설치 후에 사용자 계정을 생성하고 다시 보안이 활성화된 모드로 재시작을 해주어야 합니다.

그래서 먼저 mongodb를 실행했다가 유저 정보를 생성하고 다시 보안이 활성화된 모드로 실행을 해야 하는데 이 때 그냥 진행하면 첫번째 실행이 종료가 되었을 때 컨테이너도 같이 종료가 되니다.
이를 아래 docker 공식 문서를 참조하여 해결하였습니다.
https://docs.docker.com/config/containers/multi-service_container/

### Cluster restart issue 2 - timing

mongodb는 javascript 파일을 사용해서 데이터를 넣을 수 있습니다.
standalone으로 실행할 때는 문제가 없지만 cluster로 실행할 때는 각 요소별 타이밍을 잘 잡아주는 것이 굉장히 중요합니다.
docker로 각 container로 실행할 때 다른 서버가 준비가 안되었는데 javascript 파일을 실행하면 유저 정보 또는 데이터 생성에 실패하게 됩니다.

맞추어주어야 하는 타이밍은 크게 아래 3가지 입니다.

1. mongos & config
   mongos와 config 서버가 모두 정상적으로 연결되어 있어야 user 생성이 정상적으로 이루어지는 것으로 확인됩니다.
   그래서 mongos는 config 서버가 모두 정상 연결이 되기를 기다렸다가 user 생성을 해야 하고 config 서버도 mongos가 정상 실행이 된것을 된 후에 몇초 정도 기다렸다가 재시작을 해야 합니다.

2. mongos & replica
   db, collection, index를 미리 만들어주는 작업은 적어도 하나의 replica set이 cluster에 연결이 되어 있어야 하는 것으로 확인이 되었습니다.
   그 전에는 실행을 해도 데이터가 생성이 되지 않았습니다.

   그래서 mongos 환경변수에 기본 replica set 정보를 준 후에 sh.status() 명령어에서 그 replica set이 있는지 확인하는 방법으로 관련 처리 진행 시점을 맞추었습니다.

3. replica
   config와 replica set의 node들이 replica로 연결이 되었는지 확인해야 합니다.

   각 node들에서 rs.status().ok 여부를 확인하고 재시작하는 것으로 관련 처리를 진행하였습니다.

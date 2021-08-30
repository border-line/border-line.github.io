---
layout: single
title: "Nest JS 프로젝트 구조"
category: tech
tags: [nestjs, structure, architecture]
---

nestjs로 작업을 하면서 동일한 mvc라고 하더라도 spring과는 다른 프로젝트 구조를 가져가야 한다는 것을 깨달았습니다.

## Spring에서 사용하던 project 구조

- common
- Controller
  - LoginController
  - SignUpController
- Service
  - inf
    - LoginService
    - SignUpService
  - impl
    - LoginServiceImpl
    - SignUpServiceImpl

## nestjs project 구조

nestjs에서는 기존에 spring에서 사용하던 구조를 가져가기가 쉽지 않았습니다. 우선 nestjs에서는 쉽게 모듈, 컨트롤러, 서비스 등을 추가할 수 있도록 cli tool을 제공하고 있는데요. 이 tool을 통해서 위와 같은 구조를 만드는데에는 어려움이 있었습니다.

왜냐하면 nest g co login 와 같은 명령어로 Controller를 생성시에
아래가 아닌
src/contoller/login.controller.ts

아래와 같이 만들어지기 때문입니다.
src/login.controller.ts

nestjs에서는 module 사용을 권장하는데 module 밑에 controller, service를 만들어도 module의 최상위 위치에 만들어지지 별도 service, controller 폴더를 만들지 않습니다.

여러 service가 만들어질 것이고 그렇다면 cli에서 service를 하나로 묶어 관리하기 쉽게 해주면 안되었을까? 라는 질문이 들었습니다.

그래서 어떻게 해야할까(기존 spring에서 작업하던 것처럼 해야 하나, 아니면 nestjs cli를 통해 쉽게 작업할 수 있는 방향으로 해야 하나) 고민하다가 nestjs 공식 홈페이지에 소개되고 있는 철학을 보게 되었습니다.

![image-20210829235040594](/assets/images/image-20210829235040594.png)

우선 위 소개에서 nestjs는 javascript 프레임워크의 architecture 에 대한 문제해결을 나온 것이구나라는 것을 알게 되었습니다.

그리고 loosely coupled 라는 단어가 있는데 많은 인싸이트를 주었습니다.

nestjs는 프로그램은 서로 독립적인 모듈의 합으로 보는구나. 그리고 각 모듈은 작은 하나의 기능을 담당해서 굳이 폴더를 나눌 필요 없는 수준의 작은 기능들로 구성 되어야 하는 것이겠구나라는 생각이 들었습니다.

또한 프로젝트를 하다보면 common 패키지에 대한 필요가 있는데, 각 모듈간 서로 공유하는 것도 최소한으로 해야겠다. 그러면 우선 common이 없이 한번 가보자라는 생각을 하게 되었습니다. 물론 이후에 필요에 의해 추가할 수는 있을 것 같습니다.

다만 아직 아래 질문에 대해 명확한 답은 내리지 못했습니다.

- 그런데 모듈이 다른 모듈을 쓸 수 있고 여러 모듈로 나뉘다보면 그 의존성을 관리하는게 어려운데.. 모듈들의 디펜던시를 어떻게 잘 관리할 수 있을까?

프로젝트 진행하면서 지금 적용한 구조의 장단점 및 아직 해결 못한 고민에 대한 답을 이후에 한 번 적어보려고 합니다.

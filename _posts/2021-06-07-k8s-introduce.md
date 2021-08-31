---
layout: single
title: "MSA - Kubernetes - Intro"
category: "tech"
tags: [msa]
---

> edX에서 쿠버네티스를 공부하면서 알게 된 내용을 정리하고 공유합니다.

## Kubernetes

## 정의

[쿠버네티스는 웹사이트](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)에 따르면 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼이다.

쿠버네티스는 배의 조종사라는 의미인데 컨테이너라는 배들의 조종사 역할을 하는 프로그램으로 생각하면 된다.

- Define Kubernetes.
- Explain the reasons for using Kubernetes.
- Discuss the features of Kubernetes.
- Discuss the evolution of Kubernetes from Borg.
- Explain the role of the Cloud Native Computing Foundation.

## 기능들

쿠버네티스는 컨테이너 오케스트레이션을 위한 많은 기능들을 제공한다.

자동 빈 패킹 : 컨테이너의 리소스 필요와 제한 사항에 따라 컨테이너들을 스케쥴링 한다.

회복성 : 문제가 있는 컨테이너를 재시작하고, 문제가 있는 컨테이너에 트래픽을 보내지 않는다.

수평적 스케일링 : 하나의 서버에서 CPU 및 다른 지표에 따라 또는 수동으로 스케일링을 할 수 있다.

서비스 디스커버리 및 로드 밸런싱 : 컨테이너들은 각자 IP를 받고, 컨테이너들의 집합에 하나의 DNS 이름을 할당한다. 이를 이용해 각 컨테이들의 집합으로 로드 밸런싱을 한다.

자동화된 롤링 업데이트 및 롤백 : 쿠버네티스는 다운타임이 없도록 헬스체크를 하면서 설정 변경이나 업데이트를 롤링해서 진행하거나 롤백을 해준다.

비밀 및 설정 관리 : 쿠버네티스는 민감한 데이터와 세부 설정을 컨테이너 이미지와 분리해서 관리한다. 민감한 정보로 구성된 비밀은 깃헙과 같은 스택 설정에 노출되지 않고 바로 어플리케이션에 전달된다.

저장공간 관리 : 쿠버네티스는 로컬 스토리지, 외부 클라우드 프로바이더의 것, 분산화된 스토리지, 네트워크 시스템 스토리지로부터 자동으로 SDS(software-defined storage) 솔루션을 컨테이너에 마운트 한다.

배치 실행 : 배치 실행 및 긴 시간이 걸리는 작업을 지원한다.

# Why Use K8s

쿠버네티스는 많은 기능들을 제공하면서도 이동 가능하고 확장 가능하다. 로컬 환경, 원격 가상 환경, 기계, 클라우드 등 어디에서든지 배포될 수 있으며 써드 파티 오픈 소스 툴을 지원하고 지원을 받음으로써 사용자들에게 더 강화된 기능을 제고할 수 있다.

쿠버네티스 아키텍쳐는 모듈화 되어 있고 플러그인 방식이다. 쿠버네티스가 모듈화되어 있거 디커플 되어 있는 마이크로 서비스 어플리케이션을 오케스트레이션 하지만, 스스로도 디커플된 마이크로서비스 패턴을 따르고 있다.쿠버네티스의 기능은 커스텀 리소스, 연산자, API, 스케쥴링 룰을 추가함으로써 더 풍부해질 수 있다.

쿠버네티스는 강력한 커뮤니티의 지원을 받고 있으며 전세계 2800여 컨트리뷰터를 가지고 있으며 94000여 건의 커밋을 가지고 있다.

출시된지 얼마 되지 않았음에도 많은 기업들이 자신의 워크로드를 운영하는데 쿠버네티스를 하용하고 있다.

The [Cloud Native Computing Foundation](https://www.cncf.io/) (CNCF) is one of the projects hosted by [the Linux Foundation](https://www.linuxfoundation.org/).

CNCF aims to accelerate the adoption of containers, microservices, and cloud-native applications.

쿠버네티스는 구글 Borg 시스템으로부터 영감을 많이 받았다. Borg는 구글이 10년 전세계 운영에 사용해 온 컨테이너와 워크로드 Orchestrator다. 쿠버네티스는 아파치 라이센스 2.0으로 제공되고 Go로 만들어진 Go Languagefh TduwuTek.

2015년 6월에 첫번째 버전이 나왔으며 구글에 의해 시작되었고 CNCF에 기증되었다.

새로운 쿠버네티스는 3개월마다 버전이 나오고 있다.

구글 Borg는 수만개의 머신들 클러스터들 사이에서 구동되는 수천개의 다른 어플리케이션으로 부터 나오는 수천개의 잡들을 구동하는 클러스터 매니저이다.라고 2015년에 구글이 발표했다.

10년 넘게 Borg는 구글의 시크릿이었다. 우리가 사용하는 Gmail, Drive 등이 Borg를 사용해 서비스 되고 있다.

최초의 쿠버네티스 작성자들은 구글에서 예전에 Borg를 사용했고 개발했던 사람들이었다. 그들의 지식과 경험을 쿠버네티스에 쏟았다.

아래 몇가지 기능들은 Borg로부터 온 것이다.

Borg로 부터 온 기능들

- - - API servers
    - Pods
    - IP-per-Pod
    - Services
    - Labels.

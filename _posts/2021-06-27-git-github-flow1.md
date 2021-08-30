---
layout: single
title: "Git & GitHub"
category: tech
tags: [git, github]
---

Git과 GitHub은 프로그램 버전 관리, 협업 툴로 아래 2가지 목적을 위해 사용이 됩니다.

1. 버전 관리 (프로그램의 성장 과정 기록)

2. 협업

   - 서로 독립적으로 작업하고 쉽게 합칠 수 있도록

   - 서로 쉽게 리뷰할 수 있도록

Git과 GitHub은 위 2가지 이유를 위해 사용이 되기 때문에 당연히 위 2가지를 잘할 수 있는 방법으로 사용이 되어야 합니다. 그리고 이 목적을 이루기 위해 정말 다양한 방법이 있을텐데, 그 중에서도 많은 사람들이 괜찮다고 생각하는 대표적인 Flow(사용 방법, 사용 흐름)들이 있습니다.

- Git Flow
- GitHub Flow
- GitLab Flow

[GitLab Flow 소개 페이지](https://docs.gitlab.com/ee/topics/gitlab_flow.html)에서 Git Flow, GitHub Flow의 장단점을 함께 설명하며 GitLab Flow를 소개하고 있기 때문에 해당 글을 통해 쉽게 3가지 Flow에 대한 대략적인 이해를 할 수 있습니다.

어느 Flow가 독보적인 장점을 가지고 있다기 보다는 장단점이 있기 때문에 3가지 방법의 장단점을 이해하고 자신의 팀에 맞게 사용방법을 디자인해서 진행하면 됩니다.

좋은 Flow는 각 팀에 따라 다르지만 제가 참여해 진행하고 있는 사이드 프로젝트 팀의 Flow를 디자인할 때 고려했던 기본적인 사항에 대해서 간단히 소개합니다.

먼저 간단히 용어를 정리하면

- Upstream : 팀이 함께 공유하는 Repository
- Origin : 팀의 Repository를 fork 해 생성되어 있는 자기 계장의 Repository
- Local : Origin을 clone해 생성되어 있는 작업 PC의 Repository

와 같습니다.

Upstream이 있고 Origin을 둔 이유는 아래 2가지 입니다.

1. Pull Request를 통해 Review를 쉽게 받을 수 있게 됩니다.
2. 끝나지 않은 작업 내용을 최종 Repository(Upstream)에 반영하지 않고도 다른 사람들에게 보여줄 수 있는 방법이 생깁니다.

그리고 GitLab Flow의 아이디어와 같이 main을 develop으로 사용하고 배포 브랜치(prod)를 따로 두려고 하였고

프로그램 성장과정이 아래와 같이 기록되는 것이 아니라

![image-20210627121844305](/assets/images/image-20210627121844305.png)

아래와 같이 기록되기를 원했습니다.

![image-20210627121805539](/assets/images/image-20210627121805539.png)

그래서 하나의 기능 개발할 때 사용되는 명령어들은 대략적으로 아래와 같은 Flow를 따르도록 설계해 보았습니다.

![image-20210627122020938](/assets/images/image-20210627122020938.png)

아직 프로젝트가 정식으로 시작되지 않아서 몇가지 사항을 더 알아보고 최종 Flow를 결정하려고 합니다.

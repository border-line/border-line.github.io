---
layout: single
title: "Git - Pull Request Confict와 Rebase 동작 원리"
category: tech
tags: [git]
---

git 과 github으로 작업을 하다보면 upstream으로 pull request 를 날리고 upstream에서 rebase 로 pull을 받아야 하는 경우가 생깁니다.

upstream에서 rebase 로 pull을 받아야 하는 경우는 보통 아래 명렁어를 통해 진행합니다.

```bash
git pull upstream main --rebase
```

이 때 의도하지 않게 confict가 나는 경우가 있는데 바로 pull request 과정에서 동일한 파일을 수정한 여러 커밋에 대해 squash and merge 했을 때 입니다.

이는 pull할 때 준 옵션인 rebase의 동작원리 때문에 발생합니다.

rebase는 새로운 base가 되는 branch로 HEAD를 옮긴 후 기존 브랜치의 커밋들을 하나씩 현재 HEAD에 재적용을 합니다.

그리고 동일한 변경 내용이 있는 경우에는 해당 commit은 버리게 됩니다.

squash and merge는 여러 변경 내용이 하나로 합쳐져 있기 때문에 기존에 나누어져 있던 커밋들을 적용했을 때 conflict가 나는 것입니다.

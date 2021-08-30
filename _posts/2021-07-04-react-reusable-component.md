---
layout: single
title: "재사용이 가능한 Component"
category: tech
tags: [react]
---

회사에서 React, Redux로 개발을 하면서 React와 Redux의 패러다임의 차이로 인해 오히려 서비스 기능 개발과 관리가 어려워졌었습니다.

그 이유는 React는 컴포넌트 별로 외부와 독립된 각자의 state를 가지고 다른 곳에서도 재사용이 가능하도록 설계되었는데, Redux는 Global State를 React 컴포넌트에 연결 시켜 주고 컴포넌트들끼리 그 값을 공유할 수 있도록 하기 때문입니다.

Redux를 통해 여러 Global State에 연결된 컴포넌트는 다른 곳에서 재사용하기가 어려워지는 경우가 많습니다. 왜냐하면 Global State에 연결된 값들을 다른 값으로 바꾸어 주고 싶을 때 방법이 없기 때문입니다. 결국 다른 Global State가 연결된 컴포넌트를 다시 만들어야 합니다.

이를 해결하기 위한 방법을 고민하다가 Redux와 같은 상태 관리 라이브러리에 연결될 컴포넌트를 만들 때 직접 연결된 컴포넌트를 만드는 것이 아니라 Global State에 연결되어 있지 않은 Component를 만들고, 해당 Component와 Global State 를 연결시켜 return 해주는 별도 Wrapper 컴포넌트를 만드는 방법이 있다는 것을 알게 되었습니다.

다음 글에서는 예제와 함께 더 자세히 살펴보도록 하겠습니다.

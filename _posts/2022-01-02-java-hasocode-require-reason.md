---
layout: single
title: "JAVA에서 hashCode 정의가 필요한 이유"
category: "tech"
tags: [java]
---

우연한 기회에 eqault, hashCode의 의미에 대해 물음을 당했고,
추가로 hashCode는 항상 정의해야 하는지 그렇다면 이유는 무엇인지 고민하게 되었다.

관련해서 알아보았을 때 아래 3가지 정도 이유 때문에 hashCode를 재정의해야 하는 것으로 보인다.

### 1. Object 명세

일반적으로 생각해도 두 Object가 같다면 동일한 hashCode를 가져야 한다.

그렇지 않다면 프로그램이 동작하면서 예상하지 못한 동작을 하게 될 수도 있다.

<br/>
그렇기 때문에 equals를 재정의하는 경우에 hashCode도 함께 재정의 해주어야 한다.

### 2. HashMap, HashSet

hashCode는 HashMap, HashSet에서 사용이 되고 HashMap, HashSet은 자주 사용되는 자료구조이다.

hashCode를 재정의 하지 않으면 HashMap, HashSet에서 성능과 동작 이슈가 있을 수 있기 떄문제 재정의를 해주어야 한다.

### 3. Stream distint

디버그 모드로 확인 했을 떄 Stream의 distinct에서 hashCode를 사용하는 것으로 확인이 된다.

distinct는 프로그램 흐름에서 종종 쓰이기 때문에 hashCode를 재정의해주는 것이 좋다.

---
layout: single
title: "MonboDB vs Elastic Search"
category: tech
tags: [db, compare]
---

어떤 서비스를 만들 때 정말 많은 DB 후보군들이 있다.

하나의 DB 안에서 어떻게 튜닝해야 성능이 잘 나오는지 아는 것도 정말 중요하지만 그 전에 어떤 상황에서 어떤 DB를 사용해야 하는지 아는 것도 매우 중요하다.

최근 회사에서 기존 AWS Document DB(Monbo DB) 대신에 Elastic Search로 전환하려고 하는 이슈가 있었다.

적재되어 있는 데이터에서 조건에 매칭되는 아이템을 가져오기 위해 부분에서 대체하기 위해서인데 데이터를 가져오는 부분과 관련해서만 더미데이터를 만들고 테스트를 진행하였다.

3000만건 정도까지 1~10ms 정도에서 성능이 나와 문제 없다고 생각했는데 문뜩 기존 Mongo DB와 Elastic Search 의 차이는 무엇이고 어느 경우에 어떤 것을 써야하지라는 생각이 들었다.

그래서 Mongo DB와 Elastic Search 를 비교하는 글을 몇가지 찾아보았고 아래와 같은 간단한 차이를 알게 되었다.

- Monbo DB

  - 범용 데이터베이스로 Schemaless로 Elastic Search보다 좀 더 강력한 Update, 쿼리, 집합 기능을 제공한다. 사용하기 쉽게 되어 있다.

- Elastic Search
  - 검색엔진으로 적재되어 있는 데이터에서 여러 조건을 주어 찾는 것에 특화되어 있다. 다만 Update 하거나 Write할 때 성능에 대해서는 한번 테스트가 필요하다. 잘 사용하려면 튜닝을 많이 해야 한다.

아직 몇가지 글만 보았지만, 추가적으로 더 조사하고 정확히 어떤 경우에 쓰면 좋은지 알아보고 정리해보려고 한다.

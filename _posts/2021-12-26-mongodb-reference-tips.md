---
layout: single
title: "MongoDB Embeded vs Reference"
category: "tech"
tags: [mongodb]
---

NoSQL DB 선택 시 아래와 같은 이유로 MongoDB가 자주 선택이 되는 것 같습니다.

1. 개발자에게 친숙한 JSON 형태로 데이터 관리
2. 자유로운 스키마
3. 괜찮은 성능과 Scale-out
4. 다양한 API

그렇지만 막상 RDBMS를 사용하다가 MongoDB를 사용하기 시작하면 관계가 있는 데이터를 어떻게 다루어야 하는지에 대해 막막하게 됩니다.

MongoDB에서도 RDBMS와 비슷한 lookup 기능을 제공하지만 성능이나 기능면에서 많이 부족합니다.

연관 관계에 있는 데이터를 2가지 방법으로 표현할 수 있습니다.

### 1. Embeded Document

아래 예시에서 author 안에 books를 넣는 것처럼 Document안에 Document를 넣는 방식입니다.

```json
{
  "_id": "1",
  "author": "author1",
  "books": [
    {
      "name": "book1",
      "genre": "genre1"
    },
    {
      "name": "book2",
      "genre": "genre2"
    }
  ]
}
```

### 2. Reference

연관 데이터를 서로 다른 collection에 두고 key로 참조하는 방식입니다.

```json
{
  "_id": "1",
  "author": "author1"
}
```

```json
{
	"name": "book1",
	"genre": "genre1",
	"author_id": "1",
},
{
	"name": "book2",
	"genre": "genre2",
	"author_id": "1",
}
```

이 때 refernce key를 어디에 두느냐에 따라 성능 차이가 있습니다.

위 예시의 경우 key를 one to many에서 many인 books 쪽에 두었는데 이는 books에 데이터를 넣거나 뺄 때 author를 업데이트하지 않아도 되기 때문입니다.

key를 author쪽에 두면 books가 생기거나 업데이트 될 때마다 author를 업데이트해주어야 하는 문제가 있습니다.

하지만 read가 주요 액션이고 one to many 관계에서 one 데이터가 주로 읽히며 이 때 many쪽 key가 필요한 상황이라면 편의를 위해 one 쪽에 reference key를 둘 수도 있습니다.

## Embeded vs Reference

### Embeded를 사용해야 하는 경우

1. 연관 관계가 one to one 인 경우
2. 연관 관계가 one to many이지만 many가 많지 않고 many의 대부분의 데이터가 one과 함께 사용이 될 때

### Reference를 사용해야 하는 경우

1. 데이터를 많이 써야하고 one to many에서 many가 많고 무거워질 수 있을 때. 이 경우에 embeded로 하면 하나의 document가 너무 커져 성능 이슈가 발생하게 된다.

### Embeded와 Reference를 함께 사용 하는 경우

1. 특정 데이터의 일부분만 주로 사용되는 경우 혼용할 수 있음. 예를 들어 작가의 최신 책만 작가와 함께 조회가 된다면 책들을 books에 reference 방식으로 두고 최신 책만 author에 추가로 넣어 둠.

### 참조 글

[https://betterprogramming.pub/embedded-vs-referenced-documents-in-mongodb-how-to-choose-correctly-for-increased-performance-d267769b8671](https://betterprogramming.pub/embedded-vs-referenced-documents-in-mongodb-how-to-choose-correctly-for-increased-performance-d267769b8671)

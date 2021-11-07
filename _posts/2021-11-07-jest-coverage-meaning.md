---
layout: single
title: "Jest Coverage Meaning"
category: "tech"
tags: [jest, coverage]


---

Github Action을 이용해 Test Coverage가 어느 이상 되지 않으면 PR이 닫히도록 하기로 했습니다.

NestJS를 사용하고 있고 테스트 프레임워크는 Jest를 사용하고 있습니다. Jest는 아래와 같이 4개의 다른 기준으로 Coverage를 보여줍니다.

![image-20211107210758602](/assets/images/image-20211107210758602.png)

각 기준은 아래와 같습니다.

### Stmts 

Statements의 약자로 `const a = 3;`과 같은 하나의 지시문(instruction)을 기준으로 Coverage를 계산합니다. 해당 파일에 전체 10개의 지시문이 있는데 3개만 실행이 되었다면 Statements Coverage는 30%입니다. 

### Branch

처음에 git의 branch로 오해를 했지만 전혀 상관이 없습니다. 프로그램 실행 흐름 상 branch를 말하는 것으로 if문을 사용한 경우에 나뉘는 실행 흐름을 기준으로 Coverage를 계산합니다. 한 파일 안에 if문이 하나 사용이 되었는데 참,거짓 중 하나만 테스트 되었다면 Branch Coverage는 50%입니다. if문이 없는 경우는 100%입니다.

### Funcs

테스트 시에 실행된 함수의 수를 기준으로 Coverage를 계산합니다. Class의 경우 constructor로 포함하여 계산되며 한 파일 내 constructor 포함 4개의 함수가 있는데 테스트 시 3개가 실행이 되었다면 Function Coverage는 75%입니다. 각 함수의 내부 길이는 Function Coverage에 영향을 주지 않습니다.

### Lines

라인을 기준으로 Test Coverage를 계산합니다. 파일내에 100라인이 있는데 30라인만 테스트 시 실행되었다면 Line Coverage는 30%입니다. 한 라인에 여러 지시문이 들어 있는 경우에는 Statements Coverage와 차이가 생기게 됩니다.



## 결론

Branch는 if문이 없는 경우 Coverage가 너무 높게 나온다는 점에서, Funcs는 함수 내부 길이를 반영하지 않는다는 점에서 메인 Coverage 기준으로 삼기는 어려웠습니다. 

Line, Stmts 중 고민하다가 Code Format에 영향을 받는 Line(1줄에 1개의 지시문이 있는지 3개의 지시문이 있는지에 따라 코드 내용이 바뀌지 않았음에도 결과가 달라짐) 보다는 Stmts 기준으로 측정한 Coverage를 메인 Coverage로 사용하는 것이 합리적이라고 결론을 내렸고 해당 Coverage 기준으로 팀 내 소통을 하고 있습니다.


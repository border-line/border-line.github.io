---
layout: single
title : "JAVA 8을 사용하는 당신에게"
category: "share"

---

이 글은 잘 안되고 있는 것을 비판하기 보다는 저를 포함한 보다 많은 사람들이 생산성을 높이는 것에 관심을 두고 새로운 것을 도전하도록 하기 위해 작성되었습니다.

![image-20210519165749723](/assets/images/image-20210519165749723.png)

만약 **석기시대에 사람 100명**과 **철기시대 사람 100명**이 각 시대의 무기를 가지고 전투를 한다면 누가 이길까요?

석기시대 사람들이 무기 활용을 기가 막히게 잘 하고 철기시대 사람들은 대부분 아직 무기를 제대로 다룰 줄 모르는 경우에는 예외가 있을 수 있겠지만 **대부분은 철기시대 사람들이 전투에서 이길 것입니다.**

이 이야기를 프로그래밍 언어로 가져와 보겠습니다.

![image-20210519112026631](/assets/images/image-20210519112026631.png)

**JAVA 6을 사용하는 팀**과 **JAVA8을 사용하는 팀** 중 누가 장기적으로 더 높은 생산성을 낼까?

사람의 구성, 팀의 개발 프로세스에 따라 다르겠지만 대부분은 JAVA 8을 사용하는 팀이 더 높은 생산성을 낼 것입니다. 이는 언어 자체의 성능, 편리 기능 그리고 다른 최신 Framework들(Spring 등)의 지원 여부에 있어 JAVA8이 훨씬 유리하기 때문입니다.

그러면 JAVA8을 사용하는 팀과 최신 버전의 JAVA 또는 개발 난이도가 훨씬 쉽거나 컴퓨팅 환경 변화를 잘 반영하는 언어(Node, Go, Python, Elixir, Node, Rust 등)를 사용하는 팀 중 누가 더 높은 생산성을 낼까요?

이런 질문을 던지는 이유는 [2020년 jetbrain 조사](https://www.jetbrains.com/ko-kr/lp/devecosystem-2020/java/)에 따르면 **여전히 75%의 개발자들이 JAVA 8을 사용하고 있는 것으로 확인**되기 때문입니다. (우리 회사에서도 JAVA8을 사용하고 있습니다.)

위에 던질 질문에 답을 하다보면 자연스럽게 2가지 선택지가 우리 앞에 놓이게 됩니다.

1. 최신 버전의 JAVA 사용
2. 소프트웨어 개발 환경 변화를 잘 반영하는 다른 언어 사용

개인적으로 현대 OOP는 한계가 있다고(버그에 취약하고 스파게티 코드가 되기 쉽다고) 생각하여 함수형 프로그래밍을 지향하는 방향으로 가려고 하기 때문에 2번을 시도하려고 합니다. 

하지만 JAVA가 여전히 아래있는 **IT 기업이 기술을 선택하는데 기준이 되는 3가지 부분**에서 높은 점수(2,3번에서 특히 높은 점수)를 받고 있다고 생각하며, 이미 JAVA로 구현되어 있는 서비스와 팀에게는 최신 자바로 전환하는 것이 더 합리적인 선택일 수 있다고 생각합니다. 

- IT 기업의 기술 선택 기준 3가지
  1. 안정적인 성능과 쉬운 개발 난이도
  2. 관련 개발자를 구하기
  3. 관련 문서 및 커뮤니티 지원



그래서 JAVA 8에서 최신 버전 JAVA로 넘어 가는가는 것, 그리고 JAVA 8과 그 이후 버전의 변화에 대해 간략하게 설명해보려고 합니다.

JAVA8이 2014년에 나왔습니다. 그리고 곧 JAVA 17이 나오는 상황(2021.05 기준이며 2021.09 출시 예정)에서 70%가 넘는 개발자들이 여전히 JAVA8을 사용하는 이유는 아래 2가지가 있지 않을까 싶습니다.

1. Oracle JAVA Licence 변경으로 인한 혼란
2. JAVA 7 -> 8로 한 번 경험한 Major 업데이트에 대한 두려움

하나씩 살펴보겠습니다.

### 1) Oracle JAVA Licence - JAVA는 유료?

Oracle은 2019년 1월부터 추가로 배포되는 Oracle Java는 기존 BCL 라이센스가 아닌 OTN(Oracle Technical Network) 라이센스로 배포가 되며 해당 버전들을 사용하기 위해서는 JAVA SE 구독 서비스를 사용해야 한다고 발표했습니다.

이 때문에 2019년 1월 이후에 공개되는 JAVA를 사용하면 비용이 발생하는 것으로 오해를 하고 있는 경우가 있을 수 있습니다. JAVA는 무료인가에 대해 [Oracle 공식 홈페이지 SDK FAQ](https://www.oracle.com/kr/java/technologies/javase/jdk-faqs.html)에서 아래와 같이 무료라고 답변을 하고 있습니다.

![image-20210518004058137](/assets/images/image-20210518004058137.png)

또한 매 버전 릴리즈마다 유료 버전인 Oracle JDK와 Open JDK를 제공하고 있는데 패키징 부분을 제외하고 기능적인 면에는 차이가 없다고 이야기 합니다.

![image-20210518220808396](/assets/images/image-20210518220808396.png)

Open JDK를 사용하면서 성능 이슈가 있다는 이야기가 있지만 [2018년 11일월 게재된 오라클 블로그 글](https://blogs.oracle.com/java-platform-group/oracle-jdk-releases-for-java-11-and-later)에 따르면 JAVA 11부터는 아래 4가지 기능들(기존 Commercial로 제공이 되던 기능)까지 Open JDK에 포함이 되었기 때문에 Oracle JDK와 Open JDK는 본질적으로 동일하다고 이야기 합니다.

[JAVA 11부터 Open JDK에 포함된 기능들]

- [Java Flight Recorder](http://openjdk.java.net/jeps/328),
- [Java Mission Control](http://openjdk.java.net/projects/jmc/),
- [Application Class-Data Sharing](http://openjdk.java.net/jeps/310),
- [ZGC](http://openjdk.java.net/jeps/333).

이 내용들에 따르면 Oracle 정책 변화로 인해 JAVA 자체가 유료화로 된다고 생각했던 사람들(관련 정책 이야기를 들었을 당시의 나), Open JDK가 있는 것을 알지만 성능상 문제가 있을 것으로 생각되어 사용하지 않던 사람들은 **<u>오해를 풀고 JAVA 11 이상 Oracle Open JDK를 사용해 볼 수 있습니다.</u>** 다른 버전의 Open JDK도 있어 Oracle Open JDK라고 이야기 했으며 [다른 Open JDK 종류를 설명해놓은 글](https://u2ful.tistory.com/15)들도 함께 보면 좋을 것 같다.

단 Open JDK는 LTS 지원이 없는데 Oracle JDK와 Open JDK 차이를 더 자세히 알기 원하는 사람은 아래 글을 보시면 좋을 것 같습니다.

[https://www.baeldung.com/oracle-jdk-vs-openjdk](https://www.baeldung.com/oracle-jdk-vs-openjdk)



### 2) Major Update에 대한 두려움

Java 7 -> Java 8로의 변화는 컸으며 이에 적응하는 비용은 컸습니다. 그래서 바뀐 배포 정책으로 인해 6개월마다 올라가는 JAVA Versoin은 오히려 최신 JAVA를 쓰는데 심리적인 저항감을 주는 것 같습니다.

이 심리적 저항감을 줄이기 위해서는 JAVA 버전 배포의 새로운 사이클에 대해 이해할 필요가 있습니다.

JAVA 9부터 기존 2~3년 마다 있던 Major Update를 버리고 6개월마다 새로운 기능을 추가한 새로운 버전을 출시하는 것으로 변경되었습니다. 그리고 Oracle JDK는 장기적으로 지원할 버전으로 3년마다 LTS(Long Term Support) 버전을 지정해 관리합니다. (2021.05.19 현재 JAVA 11이 최신 LTS 버전이며, 올해 9월에 출시되는 JAVA 17도 LTS 버전이다.) 

이러한 변화는 **빠르게 변화하는 시장의 변화에 맞춰 빠르게 기능을 추가**하고, 개발자들이 새로운 버전을 적용하는 비용을 분산시키고 낮춰 **자신들의 서비스에 보다 쉽게 적용할 수 있도록 하기 위함**으로 해석됩니다.

기존에 JAVA 7 -> 8 -> 9 와 같은 버전 변화에는 Major Update라는 말을 썼는데, 오라클의 설명에 따르면 Major Update는 이제 예전 용어이며 JAVA 9 이후에 더이상  Major Update는 없고 기능 추가만 있을 뿐이라고 합니다. **JAVA 9 -> 10 -> 11 은 JAVA 8 -> 8u20 -> 8u40과 비슷**하지 JAVA 7 -> 8 -> 9가 아니라고 설명합니다.

예를 들어 JAVA 9은 이전 버전과 19,000개의 소스 코드 변화가 있었지만, JAVA9와 10은 2700개(JAVA 8과 JAVA 8u40의 차이와 비슷한 수준)의 소스 코드 변화가 있다고 한다. 관련 내용은 아래 글에서 더 자세히 확인할 수 있습니다. 

[https://blogs.oracle.com/java-platform-group/update-and-faq-on-the-java-se-release-cadence](https://blogs.oracle.com/java-platform-group/update-and-faq-on-the-java-se-release-cadence)

현재 JAVA 8을 사용하고 있다면 JAVA 9 이상으로 넘어가는데 한 번의 큰 장애물이 있겠지만, 한번 전환하면 이후 출시되는 버전에 대해 적용 비용이 낮다는 점 그리고 계속해서 추가되는 편의기능들과 JAVA11부터 추가된 기존 Oracle 유료 기능들을 생각하면 충분히 전환을 시도해볼만하다고 생각합니다.

---

# JAVA 8 ~ JAVA 15 Overview

아래 2개 글을 참조하여 작성되었습니다.

- [https://medium.com/swlh/from-java-8-to-java-15-in-ten-minutes-f42d422a581e](https://medium.com/swlh/from-java-8-to-java-15-in-ten-minutes-f42d422a581e)
- [https://en.wikipedia.org/wiki/Java_version_history](https://en.wikipedia.org/wiki/Java_version_history)

### JAVA8

- Functional Programming
  - Lambda
    - 익명 함수 객체를 만들 수 있도록 해줍니다.
  - Method Reference
    - Lambda 표현의 한 형태
    - 현재 존재하는 메소드를 참조해 Lambda 표현을 만듭니다.
  - Stream
    - Stream은 흐름이라는 말로 배열이나 콜렉션 데이터를 람다를 활용하여 처리할 수 있도록 해줍니다.
    - 기존 명령형으로 되어 있던 데이터 처리(for, while 등)을 선언적인 방법으로 처리할 수 있게 해줍니다.
    - 아래 글에 stream이 정말 잘 설명이 되어 있습니다.
    - [https://futurecreator.github.io/2018/08/26/java-8-streams/](https://futurecreator.github.io/2018/08/26/java-8-streams/)

- Optional
  - 메소드에서 리턴에 값이 없음을 명시적으로 표현하거나, Null이 반환될 경우 시스템에 큰 에러를 발생시킬 수 있는 경우에만 리턴타입으로 사용될 수 있도록 만들어졌습니다. (그러나 별도 제약이 없기 때문에 메소드 리턴타입 이 외의 다양한 곳에서 사용이 되곤 합니다.)
  - Optional이 의도하지 않게 사용이 되어 Optional을 제대로 사용하기 위한 지침 굉장히 많이 존재하니다.
    - [https://dzone.com/articles/using-optional-correctly-is-not-optional](https://dzone.com/articles/using-optional-correctly-is-not-optional)
  - 사용하는데 지침이 많다는 것은 오용하기 쉽다는 이야기입니다. 또 동일한 기능을 3항연산자 등 간단한 처리로 대신할 수 있고 3항 연산자가 성능(속도)가 훨씬 빠릅니다. 개인적으로 대규모 프로젝트에서는 오히려 Optional은 쓰지 않는 것이 더 좋다고 판단하고 있다.

### JAVA9

- JShell 
  - Chrome 개발자 도구 console에서 javascript 테스트하는 것처럼 쉽게 java 코드를 테스트해볼 수 있게 되었습니다.

- Module
  - module-info.java를 통해 더 명환학 의존 관계를 설계할 수 있게 되었습니다.
  - JRE 환경에서 이전처럼 모든 패키지를 로드하지 않고 필요한 패키지만 로드 할 수 있게 되었기 때문에 가볍고 더 좋은 성능을 낼 수 있습니다.



### JAVA 10

- Type Inference With Var(var 키워드를 통한 타입 추론)

  ```java
  var x = new HashSet<String>();
  ```

  - 드디어 위와 같은 형태의 선언이 가능해졌습니다.

 

### JAVA 11(LTS)

- Single Source File Lanch
  - 하나의 파일은 컴파일 없이 실행시켜볼 수 있게 되었습니다.
  - java ./main.java 와 같은 형태의 명령어가 가능합니다.(기존에는 main.java를 javac로 컴파일 후 java ./main.class 로 실행)



### JAVA 12

- New Switch Expression(Preview, Preview는 enable-preview 옵션을 주어야 해당 버전에서 사용할 수 있습니다. e.g. java --enable-preview Main.java)
  - 직접적으로 변수에 값을 할당하는 Switch 문이 추가되었습니다.

```java
// old
int speciesNo = 3;
String species;
switch(species) {
    case 1: species = "puppy"; break;
    case 2: species ="cat"; break;
    case 3: species ="bird"; break;
    default: species = "unknown";
}

// java 12
var speciesNo = 3;
var species = switch(type) {
    case 1 -> "puppy";
    case 2 -> "cat";
    case 3 -> "bird";
    default -> "unknown animal";
}

```



### JAVA 13

- Text Blocks - Multi Line String (Preview)  
- JAVA에서도 Multi Line String을 사용할 수 있게 되었습니다.

```java
// Old
String html = "<html>";
html += "<table>\n";
html += "</table>\n";
html += "</html>\n";

// java 13
var html = """
<html>
    <table>
    </table>
</html>
""";

```



### JAVA 14

- record 키워드를 통한 데이터 클래스(Preview)

  - record를 통해 데이터 클래스를 손쉽게 정의하고 불변 객체를 손쉽게 만들 수 있게 되었습니다.

    

    ```java
    record Person(String name, int age) { }
    Person me = new Person("john", 30);
    System.out.println(me.name());
    ```

  

- Pattern Matching for instanceof(Preview)

  ```java
  // old
  if(obj instanceof List) {
    List list = (List) obj;
    System.out.println(list.size());
  }
  
  // java 14
  if(obj instanceof List) {
      System.out.println(obj.size());
  }
  ```



### JAVA 15

- Sealed classes(Preview)

  - 특정 인터페이스 및 클래스가 어떤 클래스에만 상속을 허용할 것인지를 선언해줄 수 있습니다.

  ```java
  public sealed interface Animal permits Dog, Cat {
    ...
  }
  ```
  
  


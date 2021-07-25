---
layout: single
title : "AWS Beanstalk heap dump를 통한 장애 상황 해결"
category: "trouble"

---

지난주 회사에서 장애가 발생했는데 장애 관련 확인 중 이상한 현상을 확인하였습니다.

AWS Beanstalk을 통해 운영되고 있는 인스턴스에 요청은 많이 가고 있지 않은데 CPU가 90~100%를 치는 것이었습니다.

재시작을 하면 괜찮아지고 조금 있으면 다시 상황이 반복되었는데 이를 위해 상황 파악을 하고 조치했던 내용을 공유합니다.

우선 aws beanstalk의 tomcat으로 실행되어 있는 인스턴스에는 jdk development tool 들이 설치가 되어 있지 않기 때문에 각 자바 버전에 맞게 아래와 같은 명령어로 설치 해야 합니다.

```bash
sudo yum install java-1.8.0-openjdk-devel
```

이 후 tomcat process 확인 후에 jstat으로 heap 사용이 어떻게 진행되고 있는지 확인합니다.
```bash
ps -ef | grep java # tomcat process id 확인
sudo jstat -gcutil -h20 -t <tomcat pid> 3000 3000
```

아래와 같이 확인이 되었는데 이를 통해 Old Generation 영역이 Full GC가 일어난 후에도 계속 99%를 유지하고, 이로 인해 Full GC가 계속 일어나는 것을 확인할 수 있었습니다.

![jstat-beanstalk](/assets/images/jstat-beanstalk.png)


자세한 확인을 위해 아래 jmap 명령어로 heap dump를 떠서 확인을 진행했습니다.
```bash
sudo -u tomcat jmap -dump:live,file=/tmp/test.hprof <tomcat pid>
```

위 명령어로 생긴 힙덤프 파일을 로컬로 가져와 확인 시에 Session 객체가 상당한 비중을 차지하는 것을 확인할 수 있었습니다.

운영 중인 서버는 사용자가 잠시 거쳐가는 서버로 한두번의 요청만 날리며 세션을 유지할 필요가 없는 서비스입니다.

이를 통해 예상한 시나리오는 기존에 트래픽이 많지 않아 불필요한 세션이 유지가 되어도 괜찮았는데, 트래픽이 증가하면서 불필요한 세션이 계속 생기고 이로 인해 Old Generation이 계속 가득찬 상태가 되어 Full GC가 일어나는 것이었습니다.

그래서 Session Duration을 Tomcat에서 설정 가능한 최소 시간인 1분으로 설정하고 배포하였습니다.

배포 후 확인 시 이전에 Full GC가 일어난 후에도 계속 Old Generation이 99%로 유지되던 것이 Full GC 후에 60% 정도로 내려가며 이전과 같은 CPU 90~100%가 되는 상황은 발생하지 않았습니다.
---
layout: single
title: "Client to Server Network 연결 과정 이해! Step by Step!"
category: "tech"
tags: [network]

---

처음 프로그래밍을 접하는 경우에 네트워크는 정말 생소합니다. 

관련해서 쉽게 이해할 수 있도록 사용자가 임의의 사이트를 조회하는 과정을 점점 더 심화해서 보는 방법을 통해 설명해 보려고 합니다.

Level 1부터 7까지 있으며 Level 2 부터 있는 밑줄은 전 단계에 비해 추가된 부분을 의미합니다. 

제가 네트워크 전문가가 아니다보니 작성한 내용 중에 정확하지 않은 부분이 있을 수 있는데 확인 되는 것들은 메일(lord1229@gmail.com)로 피드백 전달드립니다. (곧 댓글 기능 추가 예정입니다..!) 또 밑에 레벨과 레벨에 대한 태그는 쉬운 이해를 위해 달아 놓은 것으로 정확하게 매칭되지 않을 수 있는 점 참고 부탁드립니다.

## Level 1 (일반인)

1. www.domain.com 을 친다.
2. 화면이 나온다.

## Level 2 (좀 아는 일반인)

1. www.domain.com을 친다.
2. <u>**www.domain.com이 가리키는 ip를 찾는다.**</u>
3. <u>**그 ip 주소로 가서 정보를 받아온다.**</u>
4. 화면이 나온다.

## Level 3 (IT에 관심 있는 일반인)

1. www.domain.com을 친다.
2. www.domain.com이 가리키는 ip를 찾는다.
3. 그 ip 주소로 간다.
4. <u>**관련 프로토콜의 기본 포트(http는 80, https는 443) 또는 별도로 지정된 포트로 가서 작업을 요청하고 응답을 받는다.**</u>
5. 화면이 나온다.

## Level 4 (프로그래머 지망생)

1. www.domain.com을 친다.
2. www.domain.com이 가리키는 ip를 찾는다.
3. 그 ip 주소로 간다.
4. 관련 프로토콜의 기본 포트(http는 80, https는 443) 또는 별도로 지정된 포트로 가서 작업을 요청하고 응답을 받는다.
5. <u>**응답을 브라우저에게 준다.**</u>
6. **<u>브라우저가 응답 받은 html 코드를 내가 보기 좋게 보여준다.</u>**

## Level 5 (초보 프로그래머)

1. www.domain.com을 친다.
2. <u>**ISP DNS Resolver에서 www.domain.com이 가리키는 ip를 찾는다.**</u>
3. <u>**ISP DNS Resolver가 해당 정보가 없다면 Root DNS, TLD(Top Level Domain) DNS, 일반 DNS에 물어서 해당 도메인에 해당하는 ip 주소를 가져온다.**</u>
4. 해당 ip 주소로 간다.
5. 관련 프로토콜의 기본 포트(http는 80, https는 443) 또는 별도로 지정된 포트로 가서 작업을 요청하고 응답을 받는다.
6. 응답을 받아가서 브라우저에게 돌려준다.
7. 브라우저가 응답 받은 html 코드를 내가 보기 좋게 보여준다.

## Level 6 (네트워크 공부를 시작한 초보 프로그래머)

1. www.domain.com을 친다.

2. ISP DNS Resolver에서 www.domain.com이 가리키는 ip를 찾는다.

3. ISP DNS Resolver가 해당 정보가 없다면 Root DNS, TLD(Top Level Domain) DNS, 일반 DNS에 물어서 해당 도메인에 해당하는 ip 주소를 가져온다.

4. 해당 ip 주소로 간다.

5. <u>**TCP 3 hand shaking을 시작한다.**</u>

   1. <u>**Client -> Server : SYN(Syncronize sequence numbers)**</u> 

   2. <u>**Server -> Client :  SYN, ACK(Acknowlegement)**</u>

   3. <u>**Client -> Server : ACK**</u>

6. 관련 프로토콜의 기본 포트(http는 80, https는 443) 또는 별도로 지정된 포트로 가서 작업을 요청하고 응답을 받는다.

7. 응답을 받아가서 브라우저에게 돌려준다.

8. 브라우저가 응답 받은 html 코드를 내가 보기 좋게 보여준다.

9. **<u>브라우저는 효율적인 통신을 위해 동일 도메인에 대해 최대 6개의 TCP 연결을 살려둔다.</u>**

   - 도메인별 최대 connection은 브라우저마다 다름.

   - 관련 RFC : https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html

   - 관련 글 : https://docs.pushtechnology.com/cloud/latest/manual/html/designguide/solution/support/connection_limitations.html

10. **<u>브라우저가 종료될 때 또는 브라우저가 일정 이상 사용이 되지 않은 연결을 닫을 때 TCP 4 handshaking을 사용해 연결 종료</u>**

    - <u>**브라우조 종료 및 브라우저가 일정 이상 사용이 되지 않은 연결을 닫을 때 TCP 연결을 닫는다는 것은 개인적인 추정.**</u>
    - <u>**4 hand shaking**</u>
      1. <u>**Client -> Server : FIN**</u>
      2. <u>**Server -> Client : ACK**</u>
         - <u>**Client로부터 종료 요청을 받았지만 처리해서 넘겨줘야 할 것이 남아 있을 수 있기 때문에 알았다는 ACK를 우선 보낸다.**</u>
      3. <u>**Server -> Client : FIN**</u>
         - <u>**Server는 전송 해야 하는 데이터를 모두 보낸 후에 FIN을 보낸다.**</u>
      4. <u>**Client -> Server : ACK**</u>
         - <u>**Client는 Server로 ACK를 보내지만 혹시 FIN 보다 늦게 오는 데이터가 올 수 있어 일정 시간 연결을 살려둔다.**</u>

## Level 7 (네트워크를 더 깊게 공부한 초중급 프로그래머)

1. <u>**인터넷 연결 시 근처 공유기 또는 모뎀에게 DHCP(Dynamic Host  Configuration Protocol)를 통해 자신의 IP, 가장 가까운 라우터 IP, 가장 가까운 DNS IP를 전달 받는다.**</u>

   - <u>**고정된 IP를 사용하지 않고 DHCP를 사용하는 이유는 제한된 IP수로 다수의 기기가 사용하는 것을 효율적으로 하기 위함.**</u>

2. www.domain.com을 친다.

3. <u>**ARP(Address Resolution Protocol)을 통해서 가장 가까운 라우터의 MAC 주소를 알아낸다.**</u>

   - <u>**ARP는 IP는 알고 있지만 MAC 주소를 모를 때 사용된다.**</u>
   - <u>**라우터는 서로 다른 네트워크 간에 데이터 패킷을 전송하는 장치.**</u>
   - <u>**알아낸 MAC 주소를 통해 라우터와 통신을 하게 되며, 라우터를 통해 외부망에 접속할 수 있게 된다.**</u>
   - <u>**한번 ARP를 통해 받아온 MAC 주소는 캐싱되며 다음 패킷 전송 시에 사용된다.**</u>

4. <u>**접속 하려는 도메인(www.domain.com)이 로컬 주소 목록에 있는지 확인 한다.**</u>

   -  <u>**MAC의 경우 /etc/hosts 파일에 정의되어 있음.**</u>

5. 로컬 주소 목록에 없는 경우 1번 단계에서 설정된 DNS 서버에 도메인에 매치되는 IP를 물어본다.

6. DNS에 정보가 없다면 Root DNS, TLD(Top Level Domain) DNS, 일반 DNS에 물어서 해당 도메인에 해당하는 ip 주소를 가져온다.

7. <u>**라우터를 통해 해당 ip 주소로 가며 NAT(Network Address Tranlation) 또는 NAPT를 통해 사용한다.**</u>

   - <u>**라우터는 서로 다른 망을 이어주는 역할을 하는 것으로 DNS 서버가 외부망에 있는 경우에도 라우터를 통해 DNS와 통신한다.**</u>
   - <u>**NAT는 하나의 IP를 여러 호스트가 함께 쓸 수 있도록 해준다.**</u>
     - <u>**출발지와 목적지 정보를 토대로 Translation을 하는데 여러 호스트가 동일한 목적지로 가는 경우에는 NAT로 구분할 수 없기 때문에 별도 포트를 두고 이를 통해 구분하는 NAPT(Network Address Port Translation)가 사용된다.**</u>

8. TCP 3 hand shaking을 시작한다.

   1. Client -> Server : SYN(Syncronize sequence numbers) 

   2. Server -> Client :  SYN, ACK(Acknowlegement)

   3. Client -> Server : ACK

9. 관련 프로토콜의 기본 포트(http는 80, https는 443) 또는 별도로 지정된 포트로 가서 작업을 요청하고 응답을 받는다.

10. 응답을 받아가서 브라우저에게 돌려준다.

11. 브라우저가 응답 받은 html 코드를 내가 보기 좋게 보여준다.

12. 브라우저는 효율적인 통신을 위해 동일 도메인에 대해 최대 6개의 TCP 연결을 살려둔다.

13. 브라우저가 종료될 때 또는 브라우저가 일정 이상 사용이 되지 않은 연결을 닫을 때 TCP 4 handshaking을 사용해 연결 종료

    - 브라우조 종료 및 브라우저가 일정 이상 사용이 되지 않은 연결을 닫을 때 TCP 연결을 닫는다는 것은 개인적인 추정.
    - 4 hand shaking
      1. Client -> Server : FIN
      2. Server -> Client : ACK
         - Client로부터 종료 요청을 받았지만 처리해서 넘겨줘야 할 것이 남아 있을 수 있기 때문에 알았다는 ACK를 우선 보낸다.
      3. Server -> Client : FIN
         - Server는 전송 해야 하는 데이터를 모두 보낸 후에 FIN을 보낸다.
      4. Client -> Server : ACK
         - Client는 Server로 ACK를 보내지만 혹시 FIN 보다 늦게 오는 데이터가 올 수 있어 일정 시간 연결을 살려둔다.
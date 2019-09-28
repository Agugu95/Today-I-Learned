# Redirection and Load-Balancing  
HTTP 프로토콜 위에서 거치는 수많은 서버들(프록시, 캐시, 게이트웨이, 분산(미러링) 서버)등을 통해 일어나는 리다이렉션  
그리고 리다이렉션을 통한 부하 분산(Load-Balancing)을 알아봄.  
- 리다이렉션
  - 리다이렉션 프로토콜    
  - 라우팅과 포워딩  
  - 리다이렉션과 로드밸런싱 프로토콜  

## Redirection  
사실 하나의 서버에만 접근할 수 있게 한다면 편하고 리다이렉션도 필요가 없다.  
하지만 우리의 HTTP 프로토콜이 원하는 것은 신뢰성 있는 트랜잭션, 최소한의 Latency, 네트워크 트래픽 감소다.  
누구나 신뢰성 있는 데이터를 요구할 것이고, 한번의 요청에 30분씩 기다리길 원하지 않고, 너무 많은 트래픽에 자신의 서버가 터지길 바라지도 않을테니  
이는 필연적으로 여러 배포 장소(서버)를 만들게 되었고, 피할 수 없는 리다이렉션을 만들게 되었다.  
결론적으로 리다이렉션은 서버에 오는 부하를 분산하기 위한 부하 균형을 떠맡게 되었고 둘은 공존하는 상태다.  
실제로 대부분의 리다이렉션은 들어오는 요청을 어떻게 최적으로 분산하고, 부하시킬지를 포함하고 있다.  

참고   
- 모든 HTTP 트래픽은 프록시가 있다면 반드시 프록시 서버를 거치게 된다.  
- 웹서버는 IP별 요청을 다룬다.  

## 리다이렉션 프로토콜  
서버 리다이렉션  
- _HTTP 리다이렉션_  
- _DNS 리다이렉션_  
- _Anycast Addressing(임의 캐스트 어드레싱)_  
- _IP MAC Forwarding_  
- _IP Address Forwarding_  

프록시 리다이렉션  
- Proxy auto-configuration(PAC)  
- Web Porxy Autodiscovery Protocol(WPAD) 웹 캐시 자동발견   
- Web Cache Coordination Protocol(WCCP) 웹 캐시 조직  
- Internet Cache Protocol(ICP)  
- Cache Array Routing Protocol(CARP)  

주로 쓰이는 리다이렉션 기법은 서버 리다이렉션으로 프록시/캐시/서버 모두에서 쓰이는 서버 리다이렉션만을 공부한다.  

### 서버 리다이렉션  
- HTTP 리다이렉션  
HTTP 리다이렉션은 요청을 받은 서버가 리다이렉트 기능을 제공해준다.  
```
Request URL: http://www.google.com/
Request Method: GET
Status Code: 307 Internal Redirect
Referrer Policy: no-referrer-when-downgrade
```
내가 HTTP 요청을 통해 Google에 접근하려 했지만, 307 Redirect 메세지를 던지며 요청을 리다이렉션 시켰다.  

      302 Redirect vs. 307 Redirect  
      302는 더 이상 Redirect가 아닌 Found로 참조가 있음을 알려준다.  
      리다이렉션까지 수행될 수 있는 건 307 Redirect으로 HTTP 메소드를 유지한 채로 임시 리다이렉션을 해준다.  
      동일 메소드로의 접근이 가능하다.  

실제로 리다이렉션은 https:// 헤더를 응답받고 그 주소로 메소드를 유지한 채 접근되었다.    
```
Location: https://www.google.com/
Non-Authoritative-Reason: HSTS

Request URL: https://www.google.com/
Request Method: GET
Status Code: 200 
Remote Address: [2404:6800:4004:81b::2004]:443
Referrer Policy: no-referrer-when-downgrade
```

그런데 문제가 있다.  
1. 원 서버의 리다이렉트는 무려 두번의 접근이 필요하다.  
HTTP 요청 -> 원 서버 리다이렉트 -> 리다이렉션 된 요청 -> 응답  
2. 원 서버가 리다이렉트 시키지 못한다면 리다이렉션 될 서버들도 접근 할 수 없다.  
3. HTTPS의 경우 한번은 HTTP 요청이 들어가게 된다.  

그래서 HTTP 리다이렉션만을 따로 사용하는 경우는 없고 보통 다른 리다이렉션 기법과 같이 사용한다.  

- DNS 리다이렉션  
서버의 IP 주소를 이용한 것으로 하나의 도메인에 여러 IP 주소가 존재할 수 있다.  
DNS가 어떤 서버의 IP 주소를 반환할지는 Round-Robin(순환)방식을 주로 이용한다.  
하지만 모든 서버에 대해서 올바르게 순환이 될지 보장할 수 없다.  
그렇기 때문에 DNS는 보통 부하 분산에 대한 알고리즘을 소유하고 있고, 이를 이용하여 Root DNS가 로컬 DNS로부터 온 요청을 처리하는 형태로 이루어진다.  

- Anycast Addressing(임의 캐스트 어드레싱)  
라우터의 라우팅에 의지한 기법으로 라우터가 말하는 가장 가까운 서버에 접근하는 방식이다.  
예로 서울, 뉴욕, 도쿄에 서버를 두고 있고 서울에서 요청이 들어온다면 라우터는 자신이 소유한 가장 가까운 서버인 서울로 라우팅하게 된다.  

- IP MAC Forwarding  
HTTP 프로토콜로 전해지는 메세지는 네트워크 계층부터 올라온 데이터 패킷을 소유하고 있다.  
각 패킷은 아이피 주소와 TCP 포트번호를 소유하고 있고, 이를 상위 프로토콜로 올라오기 전에 Layer-Switch를 이용하여  
포워딩 된 맥 주소를 통해 요청을 분산하는 기법이다.  

- IP Address Forwarding  
MAC이 아닌 Layer 4의 IP 주소를 통해 라우팅 하는 방식으로 NAT(네트워크 주소 변환)으로도 불린다.  
그리고 Layer 4의 Switch는 반드시 연결된 커넥션을 통해 클라이언트에게 응답을 돌려줘야 한다.  



  




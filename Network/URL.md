#URL과 URI  
URL이 뭔지는 알지만 정확히 URL이 뭘까?  
REST API 방식은 리소스에 URL을 지정한 것을 구현하는 방식인데..  

## URI(Uniform Resource Identfier) 또는 URL   
- URL이란  
URL은 웹 브라우저가 리소스를 찾는 방식이다.  
원래는 URI에 URL과 URN이 속해있지만 대부분의 URI는 URL방식을 따르기 때문에 현재는 URI = URL이라고 봐도 된다.  
URL은 http://www.github.com/Agugu95/Today-I-Learnd/Network/URL.md 같은 형식으로 이루어져 있다.  
각 부분은  
  - http://  
  URL이 사용할 프로토콜을 알려주는 스킴  
  - www.github.com  
  URL이 접근할 서버의 위치  
  실제로 이 주소는 DNS를 거쳐 실제 서버의 위치로 매핑된다.  
  - /Agugu95/Today-I-Learnd/Network/URL.md  
  서버에 존재하는 리소스의 경로  
로 이루어져 있고 이는 실제로 URL이 프로토콜://서버 주소/리소스 의 형태를 띄고 있는 것과 같다.  

- URL 문법  
그런데 브라우저의 URL을 보다보면 ?aaa=bbbb&cccc 와 같은 이상한게 붙어있는 경우가 있다.  
이는 Query String이라고하는 URL에게 추가적으로 범위를 주기 위한 질의 문자열이다.  
만약 데이터를 담고 있는 DB와 연결되어있을 경우 빠르게 데이터를 검색하기 위해  
http://www.github.com/Agugu95/Today-I-Learnd/Network?data=123 과 같은 형식으로 URL을 작성할 수 있다.  
Query를 처리하는 게이트웨이를 위해 보통은 &로 구분 된 이름=값 형식을 사용한다.  
http://www.github.com/Agugu95/Today-I-Learnd/Network?data=123&data2=456  


https://www.google.com/search?q=url+encoding&oq=url+encod&aqs=chrome.0.0l2j69i57j0l3.4442j0j8&sourceid=chrome&ie=UTF-8이 URL은  
  - https://
  - www.google.com
  - /search ~ 
  로 이루어진 URL이고 실제 리소스 경로는  
  q=url+encoding 
  oq=url+encod
  aqs=chrome.0.012j69i57j013.4442j0j8
  sourceid=chrome
  ie=UTF-8  
  로 이루어져 있다.  

- URL 인코딩  
URL은 서버가 가지고 있는 리소스의 경로를 표현하기때문에 반드시 안전해야한다.  
하지만 ASCII만으로는 URL을 충분하게 표현할 수 없게되었고 (7bit만으로 URL을 표현해야했기 때문에)  
안전하지 않은 문자를 인코딩할 수 있도록 %Encoding (Escape Octet)이 만들어졌다.  
%Encoding은 ASCII로 표현할 수 없는 문자에 % 기호와 두 개의 16진수를 이용하여 표현한다.  
URL에는 띄어쓰기가 없기때문에 띄어쓰기를 표현할 수 있는 %20을 사용하여 'Today%20I%20Learnd'와 같이 표현한다.  
  > [추가설명](https://perishablepress.com/stop-using-unsafe-characters-in-urls/)

- Symbolic URL과 Hard URL  
Symbolic link는 Base URL을 기준으로 자신의 위치를 표현하고, Hard link는 모든 경로를 표현한다.  
http://www.github.com/Agugu95/Today-I-Learnd/Network/URL.md  
  - Symbolic  
  ../../../URL.md  
  실제로 이 상대 URL은 Base URL을 통해 새로운 절대경로로 만들어진다.  
  - Hard  
  http://www.github.com/Agugu95/Today-I-Learnd/Network/URL.md

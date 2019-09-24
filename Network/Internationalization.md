# Internationalization  
- Internationalization(국제화)에 따른 HTTP Charset

## HTTP에서의 Charset  
우리가 실제로 작성한 데이터들은 종류에 상관없이 HTTP 프로토콜을 통해서 보낼 수 있다.  
이러한 것들은 실제로 프로토콜을 통해서 보내지는 데이터는 신호인 Bit일 뿐이기에 가능한 것이다.  
그렇다면 미국에서 영어로 작성된 데이터를 어떻게 한국의 한글을 사용하는 우리가 받을 수 있을까?  
  - Charset  
  Charset은 프로토콜을 통해서 전해질 엔터티 콘텐츠 비트들을 특정한 문자 체계로 변환할 수 있다는 걸 알려준다.  
  이러한 Charset 알고리즘은 MIME(Multipurpose Internet Mail Extension)에 정의되어 있고, 실제로 HTTP의 콘텐츠 타입 헤더에서  
  Content-Type: text/html; charset=iso-8859-6  
  이라고 되어 있다면 이는 iso-8859-6이라는 Charset Decoding방식을 사용해서 비트들을 맵핑하라는 뜻이다.  
  만약 Content-Type 헤더에서 보내준 디코딩 방식대로 디코딩하지 않는다면 우리는 깨지거나 전혀 다른 데이터를 받게될 것이다.  
  이러한 MIME Charset은 US-ASCII, utf-8 등을 포함하고 있다.  
  
    *Charset은 문자의 집합이 아닌 문자를 인코딩 하는 알고리즘의 집합이다!
    iso-8859-1은 이미 인코딩된 문자집합이고 MIME은 문자 인코딩을 위해 iso-8859-1 Charset을 사용한다.*  
  
  - [Coded Character Set](https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%9E%90_%EC%9D%B8%EC%BD%94%EB%94%A9)  
  이미 코딩된 문자 집합으로 US-ASCII와 같은 문자집합이 해당되는 Character Encoding(문자 인코딩)이다.  
  **HTTP 완벽가이드와 번역자의 해설에 따르면 Charset이라는 잘못된 표현이 문자 인코딩과 같이 굳어졌기 때문에  
  RFC 명세에서 허용하였고, 명세를 읽을 때 주의해야한다고 하고있다.**  
  ![image](https://user-images.githubusercontent.com/38939634/65487743-7ea18300-dee2-11e9-9e24-02f0fbdb4e39.png)  
  US-ASCII와 같은 Charset들은 이미 인코딩 된 문자들이 배열에 인덱싱되어 있는 것들을 가져다 쓰는 것이다.  
  
  - 가변 인코딩과 UTF-8  
  문자의 인코딩은 고정 인코딩 방식과 가변 인코딩 방식이 존재한다.  
  UTF-8은 가변 인코딩을 사용하는 문자 인코딩 방식에서 가장 널리 쓰이는 인코딩 구조로 가변 인코딩 방식을 사용한다.  
  UTF-8은 8bit 중 인코딩 된 문자의 길이를 표현한 것들을 제외한 나머지에 인코딩 데이터를 담을 수 있다.  
  ![image](https://user-images.githubusercontent.com/38939634/65488356-f1f7c480-dee3-11e9-99fb-1d2eede0cf49.png)  
  자세한 설명은 [한글 인코딩](https://blog.me/fkrns1744)  
  
이외에도 iso-2022-jp (일본어), EUC-KR, en-us 등이 있지만 넘어가자  

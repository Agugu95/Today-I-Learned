# 구조화(Structured)
- 구조화란?  
  뭔가가 정렬 되어서 만들어져있는 것.. 같은 느낌인데 구조화가 정확히 뭘까?  
  일단 프로그래밍 적 개념에서는 구조화는 절차적 프로그래밍의 하위 개념이지만 GO-TO를 없앤 개념  
  제일 유명한 다익스트라의 구조적 프로그래밍에 따르면 하나의 프로그램의 논리 구조는 제한된 몇 가지 방법만을 사용하여 비슷한 서브 프로그램으로 구성된다.  
  이는 각각의 구조와 그 사이의 관계를 이해하면 전체 프로그램의 이해를 쉽게 할 수 있으며  
  [SoC (Separation-Of-Concerns)](https://medium.com/@smartbosslee/%EA%B4%80%EC%8B%AC%EC%82%AC%EC%9D%98-%EB%B6%84%EB%A6%AC-separation-of-concerns-soc-8a8d09df066d)  구조를 만드는데 유리하다.  
    ```
    클래스의 단일 책임 원칙과 비슷한 개념으로 보인다.  
    만약  DB를 사용하고, DB를 연결하고, 데이터를 가져오고, 목록을 표현하는 코드라면   
          <?php
        $conn = new mysqli(HOST, ID, PW, DB);
        if ($conn->connect_error) {
            die($conn->connect_error);
        }
        echo “<!DOCTYPE html>
        <html>
        <head>
            <meta charset=’utf-8'>
        </head>
        <body>“;
        $result = $conn->query(“SELECT * FROM tasks”);
        if($result) {
            echo “<ul>”;
            while($row = $result->fetch(MYSQLI_ASSOC)) {
                echo “<li>” . $row[‘name’] . “</li>”;
            }
            echo “</ul>”;
        } else {
            echo “<p>할 일이 없습니다.</p>”;
        }
        echo “</body>
        </html>”;  
    ```  

  이 짧은 코드에서만  
  DB가 바뀌면? 연결 방식은? 데이터가 많아지면? 화면 구성이 바뀌면?과 같은 수많은 Concerns가 보인다.  
  이런 수많은 관심사를 하나의 프로그램이 전부 소유하게 하지 말고 가능하면 쪼개자는 것이 Separation Of Concerns라고 한다.  
  그 외 GO-TO문의 해로움 등이 있지만 결국 객체지향적 프로그래밍의 시초라고 봐도 무방하겠다.

## 구조화를 해야하는 이유와 어떻게 해야할까?  
  구조화의 이유는 구조화의 목적대로 프로그램을 각 관심사별로 분리하여 각 서브 프로그램이 하나씩의   
  관심사만을 소유하게 만들어 프로그램의 복잡성을 감소시키는 것이 그 이유이다.  
  
  [Separation Of Concerns에 대한 좀 더 자세한 번역글](https://kwangyulseo.com/2015/05/29/%EA%B4%80%EC%8B%AC%EC%82%AC%EC%9D%98-%EB%B6%84%EB%A6%ACseparation-of-concerns/)  
  나는 서버 입장에서 보는게 좋으니 서버 입장으로 보자면  
  ```
  단순히 유저의 등록을 확인하는 코드
  function registerUser() 
  {
    var request = receiveRequeset();
    validRequest(request);
    cononicalizeEail(request);
    updateDbFromRequest(request);
    sendEmail(resquest.Email);
    return ResponseMessage("success");
   }
   ```
   wow.. 여기에 전부 예외를 처리해준다면 엄청난 코드가 탄생할 것 같다.
   링크에서 말하자면 여기서 벌써 처리해야 할 Concerns는  
   - 기본 로직  
   - 예외 처리  
   - 비동기   
   - 메모리 할당과 해제 ( C++ 기준 )  
   - 파라미터 전달  
   - Thread Safety  
   - Event  
  결국 이 모든 것을 분리하여 한번에 하나씩의 관심사를 가지게 하는 것이 구조화의 목적이자 이유이다.  

- [구조화의 구현](https://medium.com/@smartbosslee/php-%EC%98%88%EC%A0%9C%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-mvc-%ED%8C%A8%ED%84%B4-1628b47b1b04)  
  구조화의 구현은 캡슐화를 통해 각 관심사를 분리하고, 계층화를 시킴으로써 완성된다.  
  예로 MVC 패턴을 들자면 Model(Persistence) - View(Presentation) - Controller(Logic)  
  ```
  ```  

## 계층화 versus 구조화  
구조화는 관심사를 분리하여 하나의 프로그램이 하나의 관심사만을 가지게 하는 것이라고 알았다.  
그렇다면 계층화는 뭘까?  
- 계층화  
사실 구조화와 비슷하긴 한데 구조화에서 좀 더 구체적인 개념으로 각 계층을 분리한 것이라고 한다.
관심사를 분리해야한다는 것을 알았다면, 관심사를 어떤 Layer별로 분리 할 것인가?  

- 구조화와의 차이  
차이라기보다 구조화를 어떻게 구현할 지의 문제인 것 같다. 
  - Presentation Layer  
  - Application Layer  
  - Domain Layer  
  - Persistence Layer  
  이렇게 나누어진 계층을 통해 각 계층별 관심사를 분리하는 것이 계층화와 구조화가 아닐까?  
  [Architecture 구현 패턴](https://mingrammer.com/translation-10-common-software-architectural-patterns-in-a-nutshell/#1-%EA%B3%84%EC%B8%B5%ED%99%94-%ED%8C%A8%ED%84%B4-layered-pattern)  

## 구조화에 대해 알아야 하는 이유는?  
내 생각으로는 결국 코드를 짜는 것은 사람이기때문이 아닐까 싶다.  
실제로 서버 운영의 핵심은 디버깅과 트러블 슈팅, 이슈 해결이 중심으로 알고 있는데  
프로그램을 계층별로 구분하고, 각 계층별로 관심사를 분리한다면 이슈의 확인과 트러블 슈팅이 더욱 쉽기 때문이 아닐까?  
실제로 내가 그러한 이슈가 있을만한 서버를 구성해본 적은 없지만 아마도 그럴 것이라고 생각한다.  
  - Web App 기준 계층화와 구조화를 이룬 Architecture  
  ![image](https://user-images.githubusercontent.com/38939634/64836845-b5e07c00-d626-11e9-977a-60d9eea0ebd9.png)
  
## 결국 구조화란?  
- 하나의 프로그램을 구성하는 코드에서 각 코드 별 관심사를 분리하는 것  
- 계층화를 통해 각 코드가 구성해야할 부분을 구분하고, 구조화를 통해 분리 된 계층에서 관심사 별로 코드를 구성한다.  
- 구조화 된 코드와 설계?는 각각 하는 역할을 추론이 가능하다.  
- 계층화(Layering)는 Abstraction(추상화)의 기법을 사용하며 '책임'을 구성한다.  
callback이 왜 계층화를 위배하지 않는지에 대한 정말 정말 좋은 글을 발견해서 인용  
  
  >계층화는 복잡함을 다루는 기술로서 추상화(Abstraction)라는 기법을 사용한다.   
  계층화는 책임과 관련된 것이다.    
  단순하지만 인접한 하위 계층에서 제공되는 것만 사용(Use)한다는 원칙을 지켜야 한다.   
  이것이 중요한 이유는 하위 계층에서 상위 계층으로의 사용(Use)이 있거나,   
  인접한 하위 계층을 뛰어넘어 더 아래에 있는 계층으로의 접근이 생기면,   
  코드의 계층화가 깨져서 이해하기 어려운 코드가 만들어지기 때문이다.    
  (혹은, 논리적인 비약으로 인해서 추상화 수준이 같은 계층에서도 달라짐)     
  따라서, 계층화를 시킨다고 할 경우에는 반드시 이런 점을 지켜야 한다.   
    
  >예외적인 경우라고 볼 수 있는 콜백(Callback)은 계층화적인 측면에서의 위반이 아니다.    
  (상위 계층에서 정의된 함수를 하위 계층의 실행 중에 호출되는 것이 콜백이다).   
  하위 계층의 책임(Responsibility)이 있지 않고, 상위 계층에 책임이 있기 때문이다.   
  즉, 하위 계층은 상위 계층에서 전달 받은 요청을 처리할 책임을 가지지만,   
  콜백의 경우는 상위 계층에서 그 책임을 가지고 있다.   
  콜백 함수가 해야할 일이나, 그 함수가 수행되고난 결과들에 대한 것은 오로지 상위 계층이 담당한다.  
  
  >하위 계층은 상위 계층의 요청인 특정한 일이 발생했을 때 호출될 함수를 등록시켜주는 것으로 책임 범위가 한정된다.   
  그외의 계층화를 위반하는 합리적인 경우라고 보는 것은, 모든 계층에서 사용하는 공용 모듈이나 컴포넌트를 가지는 경우다.   
  이런 경우에도 계층화를 명시적으로 표현하기 위해서, 어떤 계층이 공용 모듈이나 컴포넌트를 호출하는지를 명시적으로 화살표를 사용해서 알려주어야 한     다.
  
  >즉, 단방향만으로 화살표를 가도록 만들어야 계층화 위반에 해당하지 않는다.   
  쌍방향으로 이어진다면, 결국 계층간에 순환 구조가 생성되기 때문이다.   
  물론, 이런 구조를 많이 가질수록 좋은 소프트웨어는 좋은 구조가 아니다.   
  될 수 있으면 이런 것들을 줄여주는 것이 좋은 구조를 시스템에 반영할 수 있게 된다.   
  계층화 시킨 코드들은 각 모듈이나 계층에서의 변화가 상위 혹은 하위 계층으로 전달되는 영향이 제한적이기에 변화에도 유연한 시스템을 만들수 있게된다.  
## 번외  
계층화와 구조화를 공부하다보니 번뜩하고 깨달은 것은 Spring을 사용하는 이유가 이게 아닐까 싶었다.  
Spring Framework는  
    - 컨테이너를 통해 객체를 Spring이 관리한다.  
    - Plain Old Java Object 방식으로 특정 인터페이스에 종속되지 않는다.  
    - Inversion of Control을 통해 Spring이 제어권을 소유한다.  
    - Dependency Injection을 통해 각 구조화 계층 간 의존성을 연결 시켜준다.  
    - Aspect-Oriented Programing을 통해 각 코드를 구조화 시킬 수 있다.  
    - Persistent API를 적극 지원한다.  
    - 많은 라이브러리를 분리, 결합 시킬 수 있다.  
    
구조화 프로그래밍을 지원하고, 계층 별 구조화에 따른 의존성을 사용자가 신경 쓸 필요 없다는 것, 라이브러리 확장성  
이것이 스프링을 사용하는 핵심적인 이유가 아닐까?



# 참조
- [SoftwareArechitecture 구조화와 계층화 - O'Reilly](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)  
- [WebApp 계층화 - University of Debrecen](https://gyires.inf.unideb.hu/GyBITT/08/index.html)  
- [10가지 소프트웨어 아키텍쳐 패턴](https://mingrammer.com/translation-10-common-software-architectural-patterns-in-a-nutshell/)  
- [구조적 프로그래밍 - wikipedia](https://ko.wikipedia.org/wiki/%EA%B5%AC%EC%A1%B0%EC%A0%81_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [관심사의 분리 - 코딩스쿨](https://gamecodingschool.org/2015/05/29/%EA%B4%80%EC%8B%AC%EC%82%AC%EC%9D%98-%EB%B6%84%EB%A6%ACseparation-of-concerns/)  


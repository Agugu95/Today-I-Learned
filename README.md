# Today-I-Learned
## 백엔드 프로그래밍  
### 구조화(Structured)
1. 구조화란?  
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

2. 구조화를 해야하는 이유와 어떻게 해야할까?  
그렇다면 구조화를 해야하는 이유는 뭐고 어떻게 해야할까?  
- 구조화의`` 이유  
  구조화의 이유는 구조화의 목적대로 프로그램을 각 관심사별로 분리하여 각 서브 프로그램이 하나씩   의 관심사만을 소유하게 만들어 프로그램의 복잡성을 감소시키는 것이 그 이유이다.  
  
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

3. 계층화 versus 구조화  
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
  

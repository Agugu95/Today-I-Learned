# 자바에서의 RDB 사용  
기본적으로 RDB는 Persistent Layer에 위치하고, SQL이라는 독자적 언어를 통해 데이터를 관리한다.  
어플리케이션(프로그램)에서 RDB를 사용하기 위해선 SQL을 작성하고, RDBMS에게 맞는 프로토콜을 통해 SQL 쿼리를 수행하는 방식으로 이루어진다.  
이러한 방식 중에서  
- JDBC
- ODBC
- SQL Mapper
- Object Relationship Mapping 
을 알아본다.  

## JDBC란?  
- JDBC란 무엇인가?  
JDBC는 Java DataBase Connectivity로 자바에서 RDB에 직접 접근할 수 없으니, 그것을 대신 해주는 자바 내장 API다.  
RDB에 접근해서 역할을 수행하기때문에 Core API라고도 하고, RDB의 연결 과 Query 실행 및 트랜잭션 관리까지 지원해준다.  
근데 문제는 JDBC만으로는 실제로 역할을 수행할 수 없다는게 유머  
JDBC 드라이버가 필요한데, 실제로는 JDBC 드라이버가 자바 어플리케이션 상의 요청을 DBMS의 형태로 변경하여 전달해준다.  
>MySQL Connector와 같은 것들이 이에 해당하는듯  
당연히 DBMS가 하나만 존재하는 것이 아니니 내가 Oracle도 사용하고 싶고, MariaDB도 사용하고싶다면 전부 다른 JDBC 드라이버를 사용해야한다.  
그래서 이를 보완하기 위해서 존재하는 것이 ODBC와 추상화 라이브러리다.  

## ODBC란?  
- JDBC와는 다른 ODBC  
ODBC는 Open DataBase Connectivity 뭐 대충 DB의 프리한 연결을 위한 것 같다.  
앞서 말했듯이 RDB를 다루는 RDBMS는 여러개다.  
RDBMS는 RDB와 SQL을 통한 통신을 통해 데이터를 관리하고, Application은 RDBMS와 통신을 통해  
데이터를 간접적으로 컨트롤한다.  
각 통신은 소켓을 통해 이루어지는데 문제는 JDBC에서 말했듯이 각 RDBMS마다 원하는 프로토콜이 다르다는 것  
Oracle은 Oracle Driver가 필요하고, MS-SQL은 MS Driver, MySql은 Mysql Connector가 필요하니  이 얼마나 비효율적인가  
그래서 나온게 ODBC API로 모든 RDBMS에서 동일하게 작동할 수 있는 API를 만든 것이다.  
실제로는 각 RDBMS에 맞는 드라이버로 변환되지만 Application 단에서는 ODBC Driver가 제공해주는 API에 따라 코드를 작성하면 된다.  
``` Application <-> ODBC Manager <-> RDBMS에 맞게 변환 되는 ODBC Driver <-> RDBMS```  
그런데 이건 각 DBMS에서 ODBC Driver를 제공해주기에 DBMS가 설치되어있어야 한다고 한다.  

## SQL Mapper와 ORM  
- ORM이란 무엇인가?  
ORM은 보통 JPA (Java Persistence API)를 뜻하며, Java EE의 표준 ORM 기술 방식이라고 한다.  
RDB의 테이블을 객체지향적으로 사용하기 위한 목적을 가지고 있는데  
여기서 의문은 "왜 RDB 테이블을 객체지향적으로 사용해야하는가?" 였다.  
**그 이유는 RDB 자체는 Relational Data Base로 테이블 자체는 Key : Value 방식으로 존재하고 이는 객체로써의 접근을 어렵게 만들기 때문에  
사용을 한다는데 이 부분은 조금 더 알아봐야할듯**  
결론적으로 ORM을 사용하는 이유는 SQL을 통한 테이블에 대한 접근을 최대한 줄여, 비지니스 로직에 집중하기 위함이고  
이로써 얻을 수 있는 장점은 SQL을 직접 작성하지 않아도 되며, CRUD에 대한 반복적인 작업을 대신해줄 수 있다는 것  
-> 이게 아마 HeidiSQL을 사용했을 때랑 비슷한 것 같은데, RDB의 테이블을 모델(객체)로 매핑해서 사용하는 것과 비슷한 것 같다.  
차이를 알아봐야겠다.  
자바에서는 Hibernate를 사용하는 듯 

- SQL Mapper
실제로 JDBC의 경우에는 하나의 테이블을 조작한다고 했을 때 그 테이블을 조작하는 쿼리가 전부 소스코드 안에 들어가게 된다.  
>Mybatis와 JDBC는 뭐가 다른가?   <https://hub.packtpub.com/why-mybatis/>  
만약 JDBC를 이용해서 5번의 쿼리를 날리려고 한다면 코드 내에 5개의 쿼리가 남아있을거고....  
각각 다른 클래스임에도 동일 쿼리가 존재하니 이를 한곳으로 모아서 빼자!    
그리고 이걸 XML로 관리하자! 가 Sql mapper인 MyBatis의 목적인 것 같다.

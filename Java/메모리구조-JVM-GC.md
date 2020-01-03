# 자바에서 메모리를 알아본다.  
- JVM (Java Virtual Machine)과 메모리 할당  
- Java Stack/Heap Space과 메모리 할당  
- GC (Garbage Collection)와 메모리 관리    
- Java Call Stack  

## JVM과 메모리 할당  
### JVM
![image](https://mirinae312.github.io/img/jvm_gc/JVMHeap.png)  
JVM은 Runtime Data Area(실행 데이터 영역)으로 불리며 여러 영역으로 분리되어 있다. 

### runtime에서의 JVM  
![image](https://i.imgur.com/Vy1JC1b.png)  
  1. 프로그램 실행 시 JVM은 OS로부터 메모리를 할당  
  2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일  
  3. Class Loader를 통해 JVM Runtime Data Area로 로딩  
  4. 로더에 의하여 로딩 된 .class들은 Execution Engine을 통해 Interpret(해석)  
  5. 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행  
  이 과정에서 Execution Engine에 의해 GC의 작동과 쓰레드 동기화가 이루어짐  
  
### Runtime Data Area의 영역 분류  
- 모든 Thread 공유 
   - Method Area  
      - JVM에서 읽어온 클래스와 인터페이스 정보가 저장
      - Class Constructor와 Method Code가 저장  
      즉 클래스의 인스턴스가 생성된 후 메소드 실행 시 모든 정보는 Method Area에 저장   
      - Runtime Constant Pool  
      각 클래스, 인터페이스 상수, 메서드 필드와 모든 레퍼런스가 담겨있음.  
      런타임 상수 풀의 역할은 이미 있는 메소드나 필드의 참조를 통해 중복을 막음.  
    
   - Heap  
   동적 데이터가 할당되어 저장, GC 동작 대상  
   오랜 시간 참조가 없는 객체들은 GC를 통해 제거  
  
- 개별 Thread 할당  
  - JVM Language Stacks  
  Local Variable, Prameter, 메소드 정보, 임시데이터가 저장  
  JVM은 스택 프레임을 Push/Pop하는 연산만 수행  
  printStackTrace()를 통해 Stack Trace로 각 스택 프레임을 출력  
  
  - PC Registers  
  스레드가 시작될 때 생성  
  스레드의 명령어 실행을 기록, JVM의 명령어 주소를 가짐  
  
  - Native Method Area  
  바이트 코드가 아닌 Binary Code 실행 영역  
  JNI(Java Native Interface)를 통해 호출되는 C/C++의 코드를 실행하는 영역
  (I/O작업을 위한 C 라이브러리 함수 등)

### Execution Engine  
- Class Loader에 의해 Load 된 클래스들의 바이트 코드를 실행하는 모듈  
- Byte Code -> Binary Code  
  - Interpreter  
  명령어를 하나씩 실행하는 대화형식 컴파일  
  - JIT(Just-In-Time) Compiler  
  인터프리터의 느린 속도를 보완하기 위한 컴파일러로 정해진 시간 내에 모든 바이트 코드를 컴파일  
  
## Java Stack/Heap Space와 메모리 할당  
![image](https://www.baeldung.com/wp-content/uploads/2018/07/Stack-Memory-vs-Heap-Space-in-Java.jpg)  

자바의 경우 GC로 인하여 약간 특이한 Heap 영역을 가지고 있다.  
- Stack Space  
  - 정적 메모리 할당이 이루어지는 장소로 힙 영역에 동적 할당된 값들(Object)에 대한 참조를 얻을 수 있다.  
  - JVM의 각 스레드는 스택 프레임을 가지며 각 메소드 혹은 변수마다 스택 프레임을 추가하고, 해제시 스택 프레임은 빠지게 된다.  
  - 메모리가 가득 찰 시 java.lang.StackOverFlowError를 발생시킨다.  

- Heap Space  
  - 런타임 시간에 Object와 JRE 클래스에 대한 할당에 사용된다.  
  - 객체의 위치를 우리가 알 수 없으며 참조를 Stack에게 반환한다.  
  만약 stack 값이 null이라면 참조할 수 있는 값이 없으므로 nullPointerException이 발생한다.  
  
  > nullPointerException 참고  
  Calling the instance method of a null object.  
  (빈 객체의 메소드를 호출하는 경우)  
  Accessing or modifying the field of a null object.  
  (빈 객체의 필드에 접근하거나 수정하려는 경우)  
  Taking the length of null as if it were an array.  
  (빈 배열의 길이를 가져오려는 경우)  
  Accessing or modifying the slots of null as if it were an array.  
  (빈 배열의 인덱스를 접근하거나 수정하려는 경우)  
  Throwing null as if it were a Throwable value.  
  (예외처리를 위해 널 값을 던지는 경우)  
  
- 힙 영역 구조  
 ![image](https://mirinae312.github.io/img/jvm_gc/JVMHeap.png)  
 Eden과 Survior로 이루어진 Young Generation (젊은 세대)  
 참조가 거의 없는 Old, 프로그램 종료 시까지 살아있어야하는 메타데이터 집합인 Permanent  
 
 - Young Generation  
  - Eden  
  new를 통해 새로 생성된 객체가 위치  
  정기적인 쓰레기 수집 후 살아남은 객체들은 Survivor로 이동  
  
  - Survivor1, Survivor2  
  각 영역이 채워지게 되면, 살아남은 객체는 비워진 Survivor로 이동  
  이때 참조가 없는 객체들은 Minor GC로 수집 됨
  따라서, 항상 Survivor1과 2중 한 곳은 비어있는 상태  
  > Minor GC  
  Eden, Survivor에서 발생하는 GC

- Old  
Youn Generation에서 마지막까지 살아남은 객체가 이동  
Major GC가 이루어지며, Minor GC보다 횟수가 적음  
> Major GC  
Old, Permanent 영역에서 발생하는 GC  

- Permanent  
Class Loader에 의해 Load된 클래스들이 저장  
JDK 8부터는 Metaspace 영역으로 교체  

### JVM Life Cycle  
![image](https://mirinae312.github.io/img/jvm_memory/JVMObjectLifecycle.png)  
Youn Generation의 Minor GC, Old의 Major GC를 거치며 최종적으로 모든 쓰레기가 수집  

## GC와 메모리 관리  

## Java Call Stack  

### 참조  
[Process Memory Layout](https://gabrieletolomei.wordpress.com/miscellanea/operating-systems/in-memory-layout/)  
[메모리 영역, 정적/동적 메모리 할당](https://m.blog.naver.com/PostView.nhn?blogId=parkjy76&logNo=220925369874&proxyReferer=https%3A%2F%2Fwww.google.com%2F)  
[자바 메모리 관리](https://postitforhooney.tistory.com/entry/JavaStackHeap-JAVA%EC%9D%98-Stack%EA%B3%BC-Heap%EC%9D%98-%EC%9D%B4%ED%95%B4%EB%A5%BC-%ED%86%B5%ED%95%B4-Java%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC)  
[Java Call Stack](http://javavikings.blogspot.com/2011/03/3-memory-segmentscodeheapstack.html)  
[JVM 메모리 구조](https://88240.tistory.com/435)  
[java-stack-heap](https://www.baeldung.com/java-stack-heap)  
[JVM 메모리 구조](https://mirinae312.github.io/develop/2018/06/04/jvm_memory.html)  

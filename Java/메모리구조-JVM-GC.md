# 자바에서 메모리를 알아본다.  
- JVM (Java Virtual Machine)과 메모리 할당  
- Java Stack/Heap Space과 메모리 할당  
- GC (Garbage Collection)와 메모리 관리    

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
런타임 시에 JVM이 동작하며 Minor GC와 Major GC가 동작하여 쓰레기를 수집하는 것은 알았다.  
그렇다면 쓰레기 수집은 GC에서 실제로 어떻게 일어날까?  

그 전에 알아야하는 JVM의 'stop-the-world'  

![image](https://steamuserimages-a.akamaihd.net/ugc/832455393925671329/DB16E192134A631D935269E6D7F164282CD1E310/?imw=637&imh=358&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=true)  
  DIO의 'the world'가 아니다.  
  
- stop the world  
stop-the-world는 GC를 실행하기 위하여 JVM이 모든 애플리케이션의 실행을 멈추는 것이다.  
stop-the-world가 발생하면 GC를 실행하는 GC 쓰레드 이외에 모든 쓰레드는 말 그대로 'stop'한다.  GC 작업이 완료되면 'stop'은 풀리고, 어떤 GC 알고리즘을 사용하더라도 stop-the-world는 일어난다.  
이러한 stop-the-world의 시간을 줄이는 것이 바로 GC 튜닝이다.  

- GC가 존재할 수 있는 이유  
메모리의 동적 할당은 두가지 방법으로 해제가 가능하다.  
  1. 프로그램 코드에서의 명시적 해제  
  2. 쓰레기 수집기의 동작으로 인한 할당 해제  
자바는 대부분 메모리의 동적 할당을 프로그래머가 명시적으로 해제하지 않고, GC에 맡기는 편이다.  
물론 객체를 null로 지우거나 하는 경우가 있으나, 이는 메모리의 해제 목적보다는 코드의 명시적 사용을 목적으로 하는 경우가 많다.  

따라서 GC가 쓰레기 수집(객체 지우기) 작업을 하는데 이는 두 가지의 전제 조건하에서 동작을 시작했다.  
  1. 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.  
  2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.  
  
이 가설에 따라 JVM의 메모리 공간은 매우 많은 객체가 생성되는 Young Generation과   
Minor GC가 발생하고도 살아남은 객체가 모이는  Old로 나뉘었고, 각 Minor GC와 Major GC가 발생한다.  

- Old영역이 Young Generation을 참조하는 경우  
Young Generation 일명 Young 영역은 수많은 객체가 생성되고 그만큼 GC가 자주 발생한다.  
그런데 만약 Old 영역의 객체가 Young 영역의 객체를 참조해야 한다면, GC의 대상이 되지 말아야할 것이다.  
그 구분을 어떻게 하는가?  
![iamge](https://d2.naver.com/content/images/2015/06/helloworld-1329-2.png)  
  - Card table  
  Old 영역에서 Young 영역의 객체를 참조하기 위한 테이블로 512byte의 Chunk로 이루어져 있다.  
  Young 영역의 GC가 발생 시 GC는 Old 영역의 카드 테이블을 참고하여 GC의 대상을 식별한다.  
  카드 테이블은 write-barrier를 통해 관리 되는데 이는 [write barrier](https://en.wikipedia.org/wiki/Write_barrier)를 참고  

- Young Generation GC 동작   
앞서 봤듯이 Young 영역은 Eden과 Survivor1/2로 구성되어 총 3개의 영역을 가진다.  
![image](https://d2.naver.com/content/images/2015/06/helloworld-1329-3.png) 사진
이 사진과 같이 쓰이지 않는 객체는 Eden과 Survivor에서 Minor GC에 의해 처리되고, 살아남은 객체는 각 Eden -> Survivor -> Old로의 이동을 거치게 된다.  

이러한 GC 과정에는 TLABs와 bump-the-pointer 기술이 사용된다는데 아직 그정도까진 도달하지 않아도 될 것 같다.  

- Old 영역의 GC 동작  
Old 영역은 Young과 달리 기본적으로 데이터가 가득 차면 Major GC를 실행한다.  
JDK 7을 기준으로 5가지의 Major GC 방식이 존재하는데  

  - Serial GC  
  - Parallel GC  
  - Parallel Old GC(Parallel Compacting GC)  
  - Concurrent Mark & Sweep GC (CMS 또는 Mark & Sweep)  
  - G1(Garbage First) GC  

그런데 이 중에서 운영 서버에서 Serial GC는 절대 사용하면 안된다고 한다.  
단일 코어 방식을 위해 만든 방식이라 프로세스 파워의 낭비가 심하다고 한다.  

#### Serial GC  
Serial GC는 적은 메모리, 적은 코어에 적합한 방식  
Young 영역은 bump-the-pointer와 TALBs 방식을 사용  

![image](https://mirinae312.github.io/img/jvm_gc/MarkSweepCompaction.png)  
Old 영역은 Mark & Sweep Compact 알고리즘을 사용한다.  
  1. Old 영역의 살아있는 객체를 Mark  
  2. Heap의 앞 부분부터 확인하여 살아 있는 것을 남기는 Sweep  
  3. 각 객체들이 연속적으로 힙을 채워 힙에 객체가 존재하는 부분과 존재하지 않는 부분으로 나누는 Compaction  
  
#### Parallel GC  
Serial GC와 기본적인 알고리즘은 같으나, Parallel GC는 이름답게 멀티 쓰레드를 사용한다.  
![image](https://d2.naver.com/content/images/2015/06/helloworld-1329-4.png)  
따라서 Parallel GC는 메모리 크기가 클 때와 다중 코어에서 유리하다.  
이는 CMS GC를 사용해서 얻는 latency(지연) 단축에 대한 것보다 throughput(성능)이 더 중요할 때 사용 된다.  

#### Parallel Old GC  
Parallel GC는 Old 영역의 GC 알고리즘이 Mark-Summary-Compaction을 거친다.  
Summary 단계는 Sweep과 달리 GC를 수행한 영역에 대한 객체 식별을 거친다.  

#### Concurrent Mark & Sweep GC  
![image](https://d2.naver.com/content/images/2015/06/helloworld-1329-5.png)  
사진과 같이 CMS GC는 일반적인 GC보다 복잡하다.  
  - Initial Mark  
  Class Loader에서 가장 가까운 객체 중 살아 있는 객체를 찾는다.
  가장 짧은 'stop-the-world' 시간을 가진다.  
  - Concurrent Mark  
  Initial Mark를 통해 살아있다고 확인한 객체에서 참조하는 객체를 확인한다.  
  이 과정은 어플리케이션 스레드가 실행 중인 상태에서 동시에 진행된다.(멀티 스레딩)  
  - Remark  
  Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다.  
  - Concurrent Sweep  
 어플리케이션 스레드가 실행 중인 상태에서 쓰레기를 수집한다.  
  
각 단계가 각 한가지의 일만을 실행하기에 'stop-the-world'로 인해 JVM의 실행이 멈추는 시간은 매우 짧고, 응답 속도가 매우 중요한 작업에서 많이 사용한다.  
그러나 CMS GC는 다음과 같은 단점을 가지는데  
  - 다른 GC 방식보다 더 많은 메모리와 CPU를 사용한다.
  Parallel GC 기준 약 20%의 heap memory  
  - Old 영역에 대한 Compaction 단계가 기본적으로 제공되지 않는다. 
  Memory fragmentation(메모리 파편화)가 발생한다.  
따라서, CMS GC는 메모리 파편화에 따른  Compaction 작업이 자주 일어나게 되는지를 검토하여 적용해야 한다.  
그렇지 않으면 Compaction 과정으로 인한 'stop-the-world'가 다른 GC보다 길게 이어질 것이다.  

#### G1(Garbage First) GC  
이 GC 방식은 조금 특이한데 Young Generation과 Old 영역이 존재하나, 고정된 크기와 위치로 존재하지 않는다.  

![image](https://mirinae312.github.io/img/jvm_gc/G1Heap.png)  
G1 GC는 CMS GC를 대체하기 위해서 만들어진 GC로    
Heap Space를 Region이라는 일정 크기로 나누어 각 Region에 Young, Old를 동적으로 부여한다.  
JVM의 Heap은 2048개의 Region으로 나뉘어 질 수 있으며, 각 Region의 크기는 1 ~ 32MB까지 지정 가능하다.  
  > JVM이 가질 수 있는 Heap 크기를 조절 가능  
  Max Heap Size는 64Bit 8GB 메모리 기준 최대 1/4  
  2048MB까지 지정 가능, 2048개의 1MB Region으로 나눌 수 있음  
  
G1 GC는 다음과 같은 특징들을 가진다.  
  - GC는 Region 단위로 발생한다.  
  - Region을 공유하기에 Old Region이 GC로 해제 된다면 Young Region이 그 곳을 사용할 수 있다.   - Young 영역은 Parallel GC를 사용한다.  
  - Old Region의 GC가 일어날 때 Young Region의 GC도 발생할 수 있다.  
  - Old Region의 GC는 'stop-the-world'와 멀티 스레딩 방식 둘다 사용 가능하다.  
  - Old Region의 GC는 Heap 사용량이 Threshold를 넘어갈 시에 발생한다.  
  - 살아있는 객체들은 Young/Old의 GC 동안 Virtual Memory Address로 이동하기 때문에  
  Memory Fragmentation(메모리 파편화)가 발생하지 않는다.  
  - GC가 Region 단위로 발생하기에 Heap 영역의 사이즈가 클수록 유리하다.  
  
G1 GC의 Major(Full) GC 과정 
![image](https://mirinae312.github.io/img/jvm_gc/G1FullGC.png)  
  - Initial Mark  
  Old Region에 존재하는 객체들이 참조하는 Survivor Region을 Mark
  'stop-the-world' 발생  
  - Root Region Scan  
  Initial Mark를 통해 찾은 Survivor Region에 대한 GC 대상을 스캔  
  - Concurrent Mark  
  전체 힙 Region을 스캔, GC 대상이 스캔되지 않는 Region은 제외시킨다.  
  - Remark  
  'stop-the-world'를 발생시키고, 최종 GC 대상을 Remark 한다.  
  -CleanUp  
  'stop-the-world'를 발생시키고, 살아남은 객체가 가장 적은 Region에 대한 GC를 수행한다.  
  'stop-the-world' 종료 시 완전히 비워진 Region을 FreeList에 추가하여 재사용하기 위해 준비한다.  
  - Copy  
  GC가 발생한 Region이지만 CleanUp 과정에서 완전히 비워지지 않은 Region들을 새 Region으로 복사 하여 Compaction 작업을 수행한다.  
  
  
이러한 특징들로 인해 G1 GC는 JDK8부터 기본 GC 알고리즘으로 사용되고 있다.  

## 참조  
[Process Memory Layout](https://gabrieletolomei.wordpress.com/miscellanea/operating-systems/in-memory-layout/)  
[메모리 영역, 정적/동적 메모리 할당](https://m.blog.naver.com/PostView.nhn?blogId=parkjy76&logNo=220925369874&proxyReferer=https%3A%2F%2Fwww.google.com%2F)  
[자바 메모리 관리](https://postitforhooney.tistory.com/entry/JavaStackHeap-JAVA%EC%9D%98-Stack%EA%B3%BC-Heap%EC%9D%98-%EC%9D%B4%ED%95%B4%EB%A5%BC-%ED%86%B5%ED%95%B4-Java%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC)  
[Java Call Stack](http://javavikings.blogspot.com/2011/03/3-memory-segmentscodeheapstack.html)  
[JVM 메모리 구조](https://88240.tistory.com/435)  
[java-stack-heap](https://www.baeldung.com/java-stack-heap)  
[Java GC](https://d2.naver.com/helloworld/1329)  
[JVM 메모리 구조](https://mirinae312.github.io/develop/2018/06/04/jvm_memory.html)  
[Java의 GC는 어떻게 동작하나?](https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html)  
[GC와 Mark & Sweep](https://imasoftwareengineer.tistory.com/103)   

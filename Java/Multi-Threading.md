# Java Multi Threading  
[Concurrency vs. Prallelism](https://github.com/Agugu95/Today-I-Learned/blob/master/OS/Concurrence-vs-Parallel.md)와 [Porcess-Thread](https://github.com/Agugu95/Today-I-Learned/blob/master/OS/Process-Thread.md)를 봤다면 둘이 뭐가 다른지는 알았을테니 그럼 자바에서는 도대체 어떻게 멀티 스레딩에서 Concurrency와 Prallelism을 구현하는 것일까?  

## 멀티 스레딩 구현
자바에서 멀티 스레딩의 구현은 [Thread 클래스](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html)를 이용해 상속하는 법과 [Runnable Interface](https://docs.oracle.com/javase/9/docs/api/java/lang/Runnable.html)를 구현하는 방법이 있다.  
근데 일반적으로 자바는 Multiple-Extends가 불가능하기 때문에, Runnable Interface를 통한 구현으로써 많이 사용한다.  

```
Thread Class 상속
class TIL extedns Thread
{
  public void run() {
    try
    {
    } 
    catch (Exception e) 
    {
    }
  }
 }
 
 public class TILRUN
 {
  pbulic static void main(String[] args) {
    TIL til = new TIL();
    til.start();
    }
  }
```  
```
Runnable Interface Implement
class TIL implements Runnable
{
 public void run() {
  {
   try
   {
   }
   catch (Exception e)
   {
   }
  }
 }
 
 public class TILRUN
 {
  public static void main(String[] args) {
   Thread thread = new Thread(new TIL());
   thread.start();
   }
  }
```  


## [스레드 풀을 구현하는 Concurrent Package](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/concurrent/package-summary.html)  
근데 우리가 한가지 간과한 사실은 멀티 스레딩 작업에서 스레드를 사용하기 위한 객체를 생성하는 것은 상상이상으로 비용이 큰 작업이다.  
JVM의 Default Thread 할당 메모리는 256kb 만약 Thread 요청이 1000개가 들어온다면 순식간에 256,000KB(256MB)의 메모리가 소모되고   
이 1000개의 Thread를 스케쥴링 하기 위한 시간과 작업 종료 시 제거해야하는 1000개의 Thread  
다시 1000개의 요청이 들어온다면 소모 될 256,000KB의 메모리... 끔찍하다.  
때문에 똑똑한 개발자들이 Thread Pool이라는 것을 생각해냈고, 대부분의 멀티 스레딩 작업은 이러한 스레드 풀을 구현해서 이루어지고 있다.  
- [Thread Pool](https://www.geeksforgeeks.org/thread-pools-java/)  
쓰레드 풀링을 한다면 먼저 쓰레드 풀로써 생성될 수 있는 쓰레드의 갯수를 제한하고, 각 작업은 Queue에 들어와 스레드에 의해서 순차적으로 처리된다.  
![image](https://user-images.githubusercontent.com/38939634/64905010-0a5e2700-d70d-11e9-8c94-918497858db4.png)




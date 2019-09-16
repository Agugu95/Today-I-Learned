# Synchronous, Asynchronous, Blocking, Non-Blocking    
- Synchronous/Asynchronous는 무엇이 다른가?  
- Blocking/Non-Blocking은 무엇이 다른가?
- (Sync/Async != Blocking/Non-Blocking) == true?  

- 내가 알고 있던 것  
Sync/Async와 Blocking/Non-Blocking은 다르다.  
Synchronous는 프로그램이 진행과 함수의 완료 시점을 누가 관리하는가?   
Blocking 함수의 처리 결과를 누가 처리하는가?  
프로그램이 진행함에 있어 호출 된 함수가 완료되는 시점까지 기다린다면 동기  
호출 된 함수의 처리 결과를 그 함수가 처리한다면 블로킹  
sync와 blocking은 엄연히 다르고 같이 사용될 수 있다.  
Blcoking은 I/O의 사용 때문에 벌어지는 일이다.  

- 우리가 계속 보게 될 사진  
![image](https://user-images.githubusercontent.com/38939634/64935162-89c53500-d88a-11e9-9470-00a9fb3be8df.png)  
[뒤태지존의 끄적거림](https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)이라는 깃허브 블로그에서 가져온건데 이걸 보고서 이해되면 당신은 이미 모든 걸 이해하고 있다.  

## Blcoking vs. Synchronus  
일단 Blocking과 Non-Blocking은 low-level에서밖에 다룰 수 없는 I/O를 위한 것이다는걸 알아두자.  
- Blocking I/O  
실제로 Blocking과 Non-Blocking은 User-level에서 다룰 수 없는 I/O를 다루기 위한 것으로 우리가 사용할 때 직접 I/O를 다루는 것 같지만  
실제로는 OS에게 요청을 보내고, OS가 작업을 처리해주는 것이다.  
내가 데이터를 입력 중일 때 스레드에서 다른 작업이 벌어진다면 나는 데이터를 끝까지 입력할 수 없을 거다.  
이런 유쾌하지 않은 상황을 방지하는 것이 Blocking으로 I/O작업이 일어나는 중간에 스레드는 I/O작업이 종료될 때까지 Blocking 되버린다.  

- Synchronus  
동기는 프로그램에서 스레드 진행과 프로그램의 처리 흐름이 맞아야 된다.  
만약 내가 데이터를 입력 중임에도 프로그램이 진행되버린다면, 내가 입력을 끝마치기 전에 프로그램은 종료되버릴 수도 있을거다.  
그래서 Synchronus는 스레드에 의해 처리되는 작업이 완료 될 때까지 프로그램의 진행을 멈추고  
결과에 따라 처리하는 것이다.  

- So What?  
정리하자면 I/O작업은 유저 영역에서 처리 할 수가 없기에 OS에게 작업을 맡겨야한다.  
I/O작업이 일어나는 도중에 프로그램이 종료되는 일이 발생하지 않기 위해 Synchronous를 구현했고  
I/O작업이 종료되는 결과에 따라 프로그램을 처리하기 위해 Blocking을 사용한다.  
작업을 호출한 스레드는 작업이 처리되기까지 진행을 할 수 없고, 작업의 처리 결과 반환은 작업이 완료되기까지 정지되니 결국 Synchronous(User)-Blocking(OS)가 완성되는 것이다.  
물론 Sync-Blocking말고도 Sync-Non-Blocking도 존재한다.

## Non-Blocking vs. Asynchronous    
그런데 문제가 한가지 있다.  
Blocking I/O 작업 방식은 사용자가 아닌 OS 수준의 작업이 이루어지고, 사용자는 CPU 자원을 거의 사용하지 않는다.  
게다가 작업이 완료되기까지 아무것도 할 수 없다니 이 얼마나 비효율적인가?  
- Non-Blocking I/O  
그래서 나온 것이 Non-Blocking I/O다.  
사용자 스레드에서는 그대로 OS에게 I/O작업을 요청하지만 OS는 작업 결과를 바로 반환한다.  
사용자 스레드는 프로그램의 진행을 멈추지 않을 수 있고 최종적으로는 완전한 작업 결과를 받을 수 있다.  
그런데 문제는 사용자 스레드에서 작업 결과를 확인하지 않는다면 우리는 I/O작업이 완전하게 종료되었는지 안되었는지 알 수가 없다.  
한마디로 사용자 스레드에서 작업 결과를 확인하기 위해서는 계속해서 OS에게 요청을 보내야하고  
이는 별로 좋지 않은 생각인 것 같다.  
![image](https://user-images.githubusercontent.com/38939634/64936256-f55dd100-d88f-11e9-8a3a-96b5a4bf1f7a.png)  
Synchronous-Non Blocking은 사용자는 요청하고, OS는 반환한다.  
언제까지? 사용자가 원하는 결과가 반환되는 작업 완료 시 까지  

- Asynchronus(비동기)  
Synchronous와 함께 Non-Blocking을 사용한 것 까지는 좋았는데 생각보다 효율적이지 못하다.  
사용자가 매번 확인해야 한다니.. 그냥 프로그램이 진행되는 도중에 완료되면 처리하는게 좋지 않을까?  
이러한 소망을 구현한 것이 Asynchronous Event-Driven 방식이다.  
이벤트 드리븐 방식은 호출시킨 함수에 [CallBack](https://ko.wikipedia.org/wiki/%EC%BD%9C%EB%B0%B1)을 통해 해당되는 이벤트가 발생할 시에 처리하게 된다.  
![image](https://user-images.githubusercontent.com/38939634/64936690-1cb59d80-d892-11e9-823d-08cd36ad94c6.png)  
사진은 Asynchronous-Non Blocking I/O 방식으로 사용자는 작업을 요청하고 OS는 바로 반환한다.  
사용자는 작업의 완료를 신경쓰지 않고 OS가 작업이 완료됨을 알려주면 Handler에 의해 처리한다.  

## (Sync/Async == Blocking/Non-Blocking) == true? false?  
그럼 다시 처음의 사진으로 돌아와보자.  
![image](https://user-images.githubusercontent.com/38939634/64935162-89c53500-d88a-11e9-9470-00a9fb3be8df.png)  
Sync/Async와 Blocking/Non-Blocking은 분명 비슷한 것 같지만 다르고 Sync는 Blocking이 될 수 있다.  
정리하자면  
- Synchronous/Asynchronous는 호출한 작업을 어떻게 처리할 것인가?  
결과가 반환되길 기다릴 것인가? 기다리지 않을 것인가?  
- Blocking/Non-Blocking은 작업의 처리 결과를 어떻게 할 것인가?    
완료되는 시점까지 멈출 것인가? 바로 반환할 것인가?  


> 사진 출처 및 참고  
https://www.slipp.net/wiki/pages/viewpage.action?pageId=19530144  
https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/  https://stackoverflow.com/questions/2625493/asynchronous-vs-non-blocking  

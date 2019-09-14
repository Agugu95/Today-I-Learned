# Concurrency vs. Parallelism 
Concurrency(병행성)과 Parallelism(병렬성)은 정확히 무엇이 다른가?  
내가 공부를 하면서 느끼는 것은 이러한 단어를 번역하려고 하면 뜻이 더욱 모호해진다는 것이다.  
Concurrency는 Concurrency로, Parallelism은 Parallelism으로 생각하자.  
 
- Concurrency  
특정 순서없이 겹치는 기간에 시작, 실행 및 완료되는 여러 작업을 의미한다.  

- Parallelism  
여러 작업 또는 하나의 작업을 여러 부분으로 나누어 동시에 실행될 때를 의미한다.  

마치 둘이 비슷한 작업으로 보이지만 사실 둘은 완전히 별개의 작업이라고 한다.  

- 내가 알고 있는 Concurrency와 Parallelism  
내가 받은 OS수업과 기타 지식에 따르면 Concurrency는 보통 1코어 멀티 스레딩의 작업을 의미한다.  
하나의 작업을 시분할 시스템에 의하여 각 스레드를 분할함으로써 마치 동시에 실행되는 듯하게 보여주는
그런 작업으로 알고 있다.  
Concurrency에 따른 각 스레드의 메모리 문제라던가 스레드 풀 그런건 잠시 접어두도록 하자.  
그리고 Parallelism은 N:N 멀티 스레딩 작업 n개의 코어, n개의 스레드로 '진짜' 멀티 스레딩을 의미한다.  
이건 내가 구현해본 적은 없지만 물리적인 단위로 구현되는 멀티 스레딩으로 알고있다.  
  
그럼 진짜 Concurrency와 Parallelism이 무엇이 다른지 다시 공부해보도록 하자.  

## Concurrency  
Concurrency는 두 가지 이상의 작업에 대해서 동시에 실행 될 때, 우리 눈에는 이것이 '완벽하게' 동시에 실행되는 것 처럼 보이지만  
실제로 내부는 그렇게 구현되어 있지 않다.  
OS의 [시간 분할(시분할)시스템](https://ko.wikipedia.org/wiki/%EC%8B%9C%EB%B6%84%ED%95%A0_%EC%8B%9C%EC%8A%A4%ED%85%9C)에 의해 마치 우리는 모든 작업을 동시에 수행하는 듯한 착각에 빠질 수 있는 것이다.  
이러한 시분할 시스템은 CPU의 연산 시스템은 정말 정말 빠른데, 사용자의 작업 속도가 따라가지 못해 가능한 것으로 알고 있다.  

## Parallelism
Parallelism은 두 가지의 작업이 필요없다.  
단순히 작업을 물리적 단위로 분할하거나, 여러 작업을 동시에 n개의 코어로 작업하는 것이 Parallelism이다.  
1개의 코어 CPU는 Concurrency는 소유할 수 있지만, Parallelism은 가질 수 없다.  

## Concurrency vs. Parallelism  
그렇다면 진짜 차이는 뭘까?  

- 작업의 처리  
Concurrency는 어떤 한 흐름 내에서 두 가지 이상의 작업을 나누어 실행하는 것  
한마디로 N개의 코어가 있다면, N개의 코어 각각 내부에서 일어나는 작업의 분할 혹은 프로세스의 독립 실행이 바로 Concurrency다.  
Parallelism은 N개의 코어가 각각 흐름을 소유하고, 이 흐름이 동시에 일어나는 것  

결론적으로 Concurrency는 Parallelism이 아니지만, Concurrency와 Parallelism은 같이 존재할 수 있다.  
자바는 멀티 스레딩에서 OS에게 제어권을 주는 방법과 사용자가 제어할 수 있는 두가지 방법을 전부 제공한다.  
자세한 건 자바 [멀티스레딩 구현]()에서 보도록하자.

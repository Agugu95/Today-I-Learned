# Complexity  
Complexity를 번역하면 복잡도 또는 복잡성이라고 한다.  
그런데 복잡도 또는 복잡성이란 무엇일까?  
쓸데없을 수도 있지만 나는 여기서부터 "왜?"라는 생각이 들었다.  
사전에서 뜻을 찾아보니  
- 복잡성 : 갈피를 잡기 어려울 만큼 여러 가지가 얽혀 있거나 어수선한 성질  
- 복잡도 : 다이어그램에서 사전식 순서를 채용한 순서쌍을 이르는 말  
음.. 잘 와닿지 않는데 그럼 번역하지 말고 순수한 Time Complexity를 보도록 하자.  

## Time Complexity
위키피디아의 Time Complexity 원문을 봤다.  
> In computer science, the time complexity is the computational complexity 
  that describes the amount of time it takes to run an algorithm. 
  Time complexity is commonly estimated by counting the number of elementary operations performed by the algorithm, 
  supposing that each elementary operation takes a fixed amount of time to perform. 
  Thus, the amount of time taken and the number of elementary operations performed 
  by the algorithm are taken to differ by at most a constant factor.  
  
중요한 맥락만 보자면 Time Complexity란 알고리즘에따라 각 기본 연산이 수행되는 횟수를 계산하여 추정하는 것으로  
각 기본연산이 고정 된 시간을 가진다고 했을 때, 알고리즘에 의해 수행되는 시간과 계산량은 들어오는 최대 상수 인자에 따라 달라진다.  

이걸 보니 알고리즘은 어떤 하나의 정해진 방법이 아닌 Problem Solving(문제 해결)을 위한 방법들이라고 한게 생각난다.    
그렇기에 어떤 알고리즘(방법)들을 적용하면서 수행되는 시간을 추정하여 계산하는 것이 Time Complexity인 것 같다.  

결국 Complexity란 하나의 시스템에서 문제를 해결하는 것은 여러가지 알고리즘을 적용하는 것  
Time Complexity란 시스템에서 알고리즘을 적용하였을 때 걸리는 시간을 추정하는 것  

### 알고리즘에서 왜 최악의 시간만 가정하는거야?  
위키피디아 원문
  Since an algorithm's running time may vary among different inputs of the same size,  
  one commonly considers the worst-case time complexity  

알고리즘의 실행 시간은 들어오는 입력에 따라 다르기 때문에, 상상할 수 있는 최악의 경우를 상정하여 추정하는 것이라고 할 수 있겠다.  

## 알고리즘의 Running Time vs. Time Complexity  
그런데 알고리즘의 수행시간과 수행시간을 추정하는 것이 뭐가 다른걸까..?  
놀랍게도 Stack overflow에는 나와 같은 고민을 한 사람들이 있었고 그 답변들을 가져와봤다.  
[알고리즘에서 시간복잡도와 수행시간, 실행시간, 컴파일시간은 뭐가 다릅니까?](https://stackoverflow.com/questions/38926189/what-is-the-difference-between-running-time-complexity-compile-time-and-executi)  
[알고리즘에서 시간복잡도와 수행시간은 뭐가 다릅니까?](https://stackoverflow.com/questions/4915842/difference-between-time-complexity-and-running-time)    

이걸 보고 내가 얻은 결론은  
- Running Time은 말 그대로 알고리즘이 프로그램에 적용되어 수행되는 시간  
- Time Complexity는 알고리즘을 적용할 때 조건이 변화한다면 걸릴 수 있는 시간을 계산하는 것.  즉 알고리즘의 효율성을 따지는 것이다.  

## 결론  
다시 한번 되새기면 알고리즘이란 문제를 해결하기 위한 어떤 방법을 적용하는 Problem Solving이다.  
번역보다는 원문을 잘 참고하도록 하자.  
- Time Complexity는 어떤 알고리즘을 적용하였을 때 변화하는 조건에 따라 그 알고리즘이 얼마나 효율적인가를 계산하는 것  
- Algorithm Running Time은 Time Complexity에 따라 추정한 값을 통해 프로그램을 수행했을 때 실제로 걸리는 수행시간  

나름 시간을 쓴게 아까우면서 재밌었던 공부 주제였다.

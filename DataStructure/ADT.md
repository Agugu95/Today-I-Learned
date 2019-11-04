# Abstraction Data Type (추상 데이터 타입)  
추상데이터타입 또는 추상 자료형이라는 말을 참 많이 들었었다.  
시작은 객체지향의 추상화였고, Data Structure로 넘어오니 ADT라는 것이 존재했다.  
그런데 추상화라는게 정확하게 무엇일까?  
"누가 나한테 추상 자료형이 뭐야?"라고 묻는다면  
"어 그거 자료형인데 추상적인건데 어.. 구현법이 없는?"이라고 대답할 것이다.  

그러던 중 KOCW에서 건국대학교 강의를 보던 중 교수님께서 여러분은 Abstraction 이 추상화라는 것에 대해서 먼저 확실히 알아야 한다고 하셨다.  
그래서 강의를 듣고 정리 된 ADT란  
## ADT  
- 컴퓨터사이언스에서 Abstraction != 추상화  
- Abstraction의 사전적 정의는 'A statement summarizing the important points', 'to take away; remove something to not importatnt'  
중요한 내용만을 요약하거나, 중요하지 않은 것을 제외하는 것 즉 특정 관점에 따라 빼고 더하는 것이다.  
- 결국 ADT는 'from the point of view of a user of the data'  
사용자의 관점에서 바라본 데이터라는 것이다.  

사용자는 토스트를 구울 때 '식빵을 꺼내서, 토스터기를 키고, 식빵을 넣고, 알람이 울리면 꺼낸다.'라는 것만 알면 된다.  
안의 배치를 어떻게 하고, 어떻게 알람이 울리게 할 지는 Implemnter(구현자)의 일이라는 것이다.  

## 결론은?  
- ADT != 추상자료형  
관점에 따라 데이터를 다르게 해석한 것이 ADT다.  
- ADT의 사용은?  
알고리즘을 풀 때 상위레벨에서부터 시작하는 것이다.  
상위레벨(사용자)의 관점에서 아키텍쳐를 설계하고, 점점 내려오며 구현을 생각하는 것  

## 왜 OOP(Object Oriented Programing)에서 ADT인가?  
애초에 Data Structure가 나온 이유는 현실의 Thing을 컴퓨터로 가져오기 위함  
현실의 Thing은 데이터와 연산이라는 관계로 묶여있음  
C를 기준으로 본다면 C는 Structe에 데이터를 담을 수는 있지만 연산을 합칠 수는 없음  
C++/Java와 같은 OOP는 클래스라는 컨테이너 안에 데이터와 연산을 함께 담을 수 있음  
현실의 Thing을 관계로 더욱 쉽게 나타나냈기에 OOP를 사용하는 것  

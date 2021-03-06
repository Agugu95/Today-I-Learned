# List  
[권오흠 자료구조](https://www.youtube.com/watch?v=eTwYE-ercNM&list=PL52K_8WQO5oXIATx2vcTvqwxXxoGxxsIz&index=13)  

List는 데이터는 순서를 정하여 나열한 자료구조  
Set도 데이터를 나열한 자료구조지만 말 그대로 데이터의 집합이기에 순서가 없다.  

List는 이러한 순서를 이용하여 삽입, 삭제, 검색, 확인 등의 연산을 할 수 있고 Array와 Linked List로 구분된다.  

## Array
- 가장 기본적인 List 구현  
- 고정된 크기(정적 할당)  
- 인덱스(순서)를 통한 랜덤 액세스 가능
- Elements의 삽입과 삭제 연산 시 인덱스의 이동이 필요한 단점      
- 인덱스를 통해 모든 연산은 N만큼의 시간을 소요하는 장점  

## Linked List  
- 랜덤 액세스를 희생하고, 가변 크기를 얻은 List  
- 랜덤 액세스 불가, 순차적인 접근만 가능  
- 인덱스의 이동이 필요없는 삭제와 삽입 연산의 장점  
- 반드시 Head 노드의 정보를 가지고 있는 최초의 노드가 필요함, 랜덤 액세스가 불가능 하기에 최초 노드를 모를 시 접근 불가능  

### Array 구현  
- C Primitve Type Array   
```
int main() {
  int arr[10] = { 0, };
}
``` 
0의 초기값을 가지는 index 10의 int 타입 배열  
배열 포인터 arr -> int 타입 배열 0번 주소  
컴파일 타임에 배열의 크기를 알 수 있는 정적 메모리 할당이 이루어짐  

- Java Primitve Type Array  
```
public static void main(String args[]){
  int[] arr = new int[10];
}
```
자바는 기본적으로 배열을 객체로 취급  
int 타입의 배열을 가질 수 있는 변수 arr(stack) ->  int 타입 10의 배열 0번 주소(heap)  
즉 new를 통한 배열 값을 변수가 가지고 있지 않다면, 변수는 아무것도 가지지 않은 null 상태이므로
nullPointerException 발생  

### Singly Linked List 구현

- C Linked List  
```
typedef struct node {
  char *data; // 데이터가 문자열이라 가정
  struct node *next; // 동일한 구조체를 포인터로 가지는 자기 참조형 구조체
}Node;

Node *head = NULL; // 연결리스트 시작 노드를 저장할 헤드 포인터

int main() {
  Node *head = (Node *)malloc(sizeof(Node)); // 노드 포인터
  head->data = "Tuseday"; // 포인터 화살표  연산자 
  head->next = NULL;
  
  // 1번
  Node *q = (Node *)malloc(sizeof(Node)); // 노드 포인터
  q->data = "Monday";
  q->next = NULL;
  head->next = q;
  
  // 2번
  q = (Node *)malloc(sizeof(Node));
  q->data = "Friday";
  q->next = head;
  head = q;
  
  // 3번 
  Node *p = head;
  while(p!=NULL) {
    printf("%s\n", p->data);
    p = p->next;
  }
}
```
실제로 노드를 수작업으로 생성하지는 않음, 반복문을 이용

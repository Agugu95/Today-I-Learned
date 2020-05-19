# Stack  
- Stack의 개념을 알아본다.  
- Stack의 ADT 구현을 알아본다.  
- Stack을 Array와 Linked List로 구현 해본다.  

## Stack이란?  
![image](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b4/Lifo_stack.png/350px-Lifo_stack.png)  
스택이란 어떠한 데이터들을 순차적으로 저장할 수 있으며 입구와 출구가 하나밖에 존재하지 않는 자료구조다.  

스택의 특징  
  - LIFO(Last-In-First-Out)  
  입구와 출구가 한 곳밖에 없기에 마지막으로 들어간 자료가 가장 먼저 나온다.  
  ![image](https://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Tallrik_-_Ystad-2018.jpg/220px-Tallrik_-_Ystad-2018.jpg)  
  접시를 한 곳에 쌓아두었을 때 맨위에 접시부터 꺼낼 수 박에 없는 것을 생각하면 된다.  
  - 데이터의 위치를 따로 지정할 수 없기에 데이터의 연산은 push(넣기)와 pop(꺼내기)밖에 존재하지 않는다.  
  - top을 이용하여 스택의 위치를 파악한다.  

## Stack ADT  
스택은 다음과 같은 연산을 가지고 있다.  
  - Push  
  스택에 데이터를 넣는 push 연산  
  - Pop  
  스택에서 데이터를 꺼내는 pop 연산  
  - Empty  
  스택이 비어있는지 확인하는 Empty 연산  
  - Peek  
  스택의 위치를 파악하고 데이터를 확인할 수 있는 Peek 연산  
  - Full 
  스택이 가득 찼는지 확인하는 Full 연산  
  
## Stack 구현  
스택 역시 하나의 리스트를 가지고 만들어진 자료구조이기에 Array와 Linked List를 통해 구현할 수 있다.  
```
public class implementStack {
    static class Stack<T> {
        static class Node<T> {
            private T data;
            private Node<T> next;

            public Node(T data) {
                this.data = data;
            }
        }

        private Node<T> top;

        public void push(T data) {
            Node<T> node = new Node<>(data);
            node.next = top;
            top = node;
        }

        public T pop() {
            if (top == null) {
                throw new EmptyStackException();
            }
            T data = top.data;
            top = top.next;
            return data;
        }

        public T peek(){
            if (top == null) {
                throw new EmptyStackException();
            }
            return top.data;
        }

        boolean isEmpty(){
            return top == null;
        }
    }
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(1);
        stack.push(2);
        System.out.println(stack.peek());
        System.out.println(stack.pop());
        System.out.println(stack.peek());
        System.out.println(stack.pop());
        System.out.println(stack.isEmpty());
    }
}
```
  
## 참고  
[Wikipedia Stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))  
[권오흠 자료구조](https://www.youtube.com/watch?v=PQHnuPnfqgU&list=PL52K_8WQO5oXIATx2vcTvqwxXxoGxxsIz&index=34)  

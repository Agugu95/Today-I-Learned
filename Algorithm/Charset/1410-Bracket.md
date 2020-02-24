# Bracket  
괄호의 짝을 맞추기  

## 짝이 맞는 괄호만 출력하기  
자바 Collection인 Stack을 사용  
```
public static void main(String[] args) {
        String in = "";
        Scanner sc = new Scanner(System.in);
        Stack<Character> stack = new Stack<>();
        StringBuffer sbf = new StringBuffer();
        in = sc.nextLine();
        for (int i = 0; i < in.length(); i++) {
            if (in.charAt(i) == '(') {
                stack.push(in.charAt(i));
            } else {
                sbf.append(stack.pop());
                sbf.append(in.charAt(i));
            }
        }
        System.out.println(sbf);
    }
```

1. 괄호 문자열에서 charAt()을 통해 가져온 문자가 여는 괄호면 스택에 넣는다.
2. 여는 괄호가 아니라면 스택의 이전 데이터를 pop()하고, 현재 문자를 넣는다.  
생각나는대로 단순하게 풀어봤다.  

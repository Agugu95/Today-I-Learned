# Recursion  
재귀 알고리즘  
자신이 자신을 호출하는 연쇄함수  
Base Case가 없다면 무한 루프에 빠진다.  

## Towers of Hanoi  
오늘 4시간? 5시간? 정도 Recursion이랑 Hanoi만 공부했는데 솔직히 제대로 공부가 됐는지 모르겠다.  
정리를 할 정도의 상태는 아닌 것 같아 공부했다는 기록만 남겨둔다.  

```
import java.util.scanner;

public class Hanoi{
  static void moveDisk(int num, int start, int mid, int end) {
    if(num == 0) {
      return;
      }
      
      moveDisk(num - 1, start, end, mid);
      moveDisk(num - 1, mid, start, end);
   }
   
   public satatic void main(String args[]){
    Scanner sc = new Scanner(System.in);
    int num = sc.nextInt();
    moveDisk(num, 1, 2, 3);
   }
}
```

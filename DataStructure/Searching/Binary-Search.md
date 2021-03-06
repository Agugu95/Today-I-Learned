# Binary Search(이진 검색)  
이진 검색은 데이터가 이미 DESC(내림차순) 또는 ASC(오름차순)으로 정렬 된 상태에서 적용할 수 있는 검색 알고리즘이다.  
선형 검색보다 빠르며, 매 연산 시 n/2로 줄어든다.  

## Binary Search Time Complexity  
이진검색의 시간복잡도는 매 시행마다 n/2으로 줄어든다.  
3번 시행 시 1/2 * 1/2 * n/2, k번의 시행은 결국 (1/2)k * N이 된다.  
Worst case를 가정하였을 때 마지막 시행까지하고 남은 자료의 수는 1개 = 검색할 필요가 없다.  
고로 (1/2)k * N == 1 라고 할 수 있겠다. (연산이 계속되면 결국 1개의 자료가 남는다.)  
양변에 2k를 곱해주고, logN을 취하면 k == log2N  
k는 시행횟수였으니 결국 시행 횟수는 log2N, 즉 N개의 데이터에 대한 수행 횟수는 log2N이다.  
따라서 시간복잡도는 Big O 표기법으로 표기한다면 O(logN)이 되겠다.  

*수식으로 표현하고 싶은데...*

## Binary Search 구현 
- API를 사용하지 않은 구현  
```
  public class bSearch {
    public static void main(String[] args) {
      int arr[] = new int[]{1, 2, 3, 4, 5, 6, 7};
      int key, result;
      Scanner sc = new Scanner(System.in);
      do {
        System.out.print("찾을 값을 입력하세요: ");
        key = sc.nextInt();
        result = binarySearch(key, arr);
        if (result == -1) {
          System.out.println("해당하는 값이 없습니다.");
        } while (key == result);
    }
    
    public static int binarySearch(int key, int[] arr){
      int mid;
      int start, end;
      start = 0;
      end = arr.length -1;
      while (end >= start) {
      mid = (start + end) / 2;
      if (key == arr[mid]) {
        return mid;
        } 
      if (key < arr[mid]) {
        end = mid - 1;
        } else {
          start = mid + 1;
         }
      }
      return -1;
    }
}
```

- java.util.Arrays.binarySearch() 사용
```
public class apiBsearch {
  public static void main(String[] args) {
    int arr[] = new int[]{1, 2, 3, 4, 5, 6, 7};
    int key, result;
    Scanner sc = new Scanner(System.in);
    do {
      System.out.print("찾을 값을 입력하세요: ");
      key = sc.nextInt();
      result = Arrays.binarySearch(arr, key);
      if (result == -1) {
        System.out.println("찾는 값이 없습니다.");
       }
     } while (key == result);
  }
 }
```
   
   

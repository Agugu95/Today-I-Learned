# 자바에서의 정렬  
자바에서의 정렬은 Comparator<T>와 Comparable<T> 인터페이스를 구현하는 방법이 있다.  
데이터의 정렬을 위해서는 데이터를 가지고 있는 자료구조 내의 데이터 간 크기를 비교할 수 있어야 한다.  
즉, 자바에선 비교를 위한 오브젝트들의 수치화가 가능해야 한다.  
Comparable<T> Interface가 받을 수 있는 파라미터는 
> T - the type of objects that this object may be compared to  
객체와 객체 간 비교가 가능해야한다.  
  
  - Comparable Interface    
  - Comparator Interface  
  - 정렬의 실제 구현은 어떻게 되어 있나?  
  - Comparator vs. Comparable  

## Comparable Interface  
Comparable Interface를 좀 더 자세히 살펴보자.  
Comparable Interface는 Natural Order(자연 정렬)을 구현하고 있으며 
  - Natural Order(자연 정렬)  
  데이터 정렬 시 숫자의 순서를 자연스럽게 맞추어 주는 것    
  a1, a10, a2, a21은 자연스럽지 않음    
  a1, a2, a10, a21은 자연스러운 정렬    


Comparable<T> Interface는 CompareTo(T o)만을 inteface 메소드로 정의하고 있는데 ComparaTo(T o)는  
  ```
  Compares this object with the specified object for order.  
  Returns a negative integer, zero, or a positive integer as this object is less than, equal to, or greater than the specified object.  
  ```
this가 o보다 큰 경우 1 반환 this > o return 1;    
this가 o보다 작은 경우 -1 반환 this < o return -1;    
this가 o와 같은 경우 0을 반환 this == o return 0;    

## Comparator Interface  
Comparator는 java.util.Comparator 인터페이스로 존재한다.  
java doc의 설명에 따르면 기본적인 정렬이 아닌 각 개체 콜렉션에 순서를 비교할 수 있고 이는 Arrays.sort / Collections.sort에 전달할 수 있다.  
즉 내가 지정한 객체의 컬렉션을 가지고 비교를 할 수 있다.  
Comparator는 Compare(T o1, T o2)를 메소드로 정의하고 있으며  
```
  int compare​(T o1, T o2)  
  a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.
```
첫번째 객체 > 두번째 객체 return 1;
첫번째 객체 < 두번째 객체 return -1;
첫번째 객체 == 두번째 객체 return 0;

## 클래스 라이브러리에서의 정렬 비교
- java.util.Arrays.sort()  
  배열 라이브러리에서 제공해주는 메소드  
  ASC(Ascending 오름차순) 자연정렬을 해준다.  
  
- java.util.Collections.sort()  
  Collection 프레임워크를 다룰 수 있는 Collections 라이브러리에서 제공해주는 메소드  
  역시 ASC (오름차순) 자연정렬을 해준다.  

각 Comparable Interface를 구현하고 있는 클래스끼리는 인스턴스 비교가 가능하고  
Natural Order가 된다.  
  
## 각 라이브러리 정렬의 구현  
- Arrays.sort()  
```
static final class NaturalOrder implements Comparator<Object> {
        @SuppressWarnings("unchecked")
        public int compare(Object first, Object second) {
            return ((Comparable<Object>)first).compareTo(second);
        }
        static final NaturalOrder INSTANCE = new NaturalOrder();
    }
```
  Comparator<T> 를 통해 compare(O o1, O o2)를 구현하고 있다.  
  
- Collections.sort()  
  ArrayList, List, Vector등이 sort()를 가지고 있는데, 내부적으로 Arrays.sort를 사용한다.

결론적으로 Array나 List같은 경우는 Comparable Inteface를 구현한 클래스들을 사용하여 Natural Order를 할 수 있다.   

## Comparator vs. Comparable  
- Comparable Interface  
기본적으로 자바는 Arrays.sort, Collections.sort에서 자료구조에 대한 정렬을 제공해준다.  
이 정렬들은 실제로 Comparable Interface를 구현하고 있고, 순수한 상태의 Comparable 구현은 Natural Order(자연정렬)과 문자열 정렬을 제공한다.  
```
public static void main(String[] args) {
        String[] name = new String[]{"ab", "cd", "ef", "ge", "hr", "zx", "wqw", "xcv"};

        Arrays.sort(name);
        for (int i = 0; i < name.length; i++) {
            System.out.print(name[i] + " "); 
        }
        System.out.println();
        Arrays.sort(name, Collections.reverseOrder());
        for (int i = 0; i < name.length; i++) {
            System.out.print(name[i] + " ");
        }
    }
```
기본적인 Arrays.sort Collections.sort API를 사용한 문자열 정렬이다.
```
ab cd ef ge hr wqw xcv zx // Arrays.sort(name)
zx xcv wqw hr ge ef cd ab // Arrays.sort(name, Collections.reverseOrder())
```


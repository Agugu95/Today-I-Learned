# What's the Diffrence Integer.valueOf() and Integer.parseInt()  
valueOf를 사용하던 도중 ParseInt와 뭐가 다른지 궁금해져서 찾아봄.  
- JDK Document 
```
static int parseInt​(String s)	
Parses the string argument as a signed decimal integer
문자열을 Decimal int로 Parser한다.
```
```
static Integer valueOf​(int i)	
Returns an Integer instance representing the specified int value.
지정된 int값을 가지는 Integer instance를 반환.
```
그런데 문제는 valueOf와 parseInt의 반환값이 동일하다는 것.  
분명 valueOf는 Integer instance를 반환해준다고 하는데?  
그래서 까보니  
```
public static int parseInt(String s) throws NumberFormatException {
        return parseInt(s,10);
    }

 public static Integer valueOf(String s) throws NumberFormatException {
        return Integer.valueOf(parseInt(s, 10));
    }
```
결국 valueOf도 내부에서 parseInt를 사용하는 거시여따...


#Cache
- 캐쉬의 사본 판별  
조건부 헤더 IMS(If-Modified-Sence)를 Request Header에 추가한다.  
True라면 캐쉬는 Origin Server로 가서 신선한 상태의 파일 사본을 받아온다.  
False라면 캐쉬는 304 Not Modified를 반환한다.  

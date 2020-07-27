# Algorithm  

###  특정 문자 위치 찾기
~~~
.index()
~~~
-  특정 문자나 문자열을 앞에서부터 처음 발견되는 인덱스를 반환하고 만약 찾지 못했으면 '-1'을 반환함  

###  숫자 자릿수 반환  
~~~
(int)(Math.log10(변수)+1)
~~~

###  Char형 int형 변환  
-  유니코드
~~~
char - '0'
~~~  
-  래퍼클래스
~~~
Character.getNumericValue(변수.CharAt(i))
~~~   

###  앞뒤 공백 제거  
~~~
.trim()
~~~  

###  isEmpty vs null  
-  null : 객체가 생성되지 않음  
-  isEmpty : "" 빈 객체가 생성됨  

###  바꾸고싶은 문자열 치환하기
~~~
str = str.replaceAll(기존문자, 바꿀문자)
~~~




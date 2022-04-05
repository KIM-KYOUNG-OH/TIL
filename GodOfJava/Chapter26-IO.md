# 26장 IO

# Input
- 파일을 읽음

# Output
- 파일을 쓰거나 외부로 전송할 때

# 스트림이란?
- 끊기지 않고 연속적인 데이터

# NIO
- JDK 1.4에 등장
- 빠른 IO처리를 위해 스트림이 아닌 Buffer와 Channel 기반으로 동작

# NIO2
- java 7에 등장

# File
- 파일 자체와 파일 경로를 포함
- 이제 거의 안쓰지만 알아두고 넘어가자

# Files
- NIO2와 함께 사용
- 인스턴스를 생성해야하는 File 클래스와 달리 모든 메소드가 static으로 선언되어 있어서 객체 생성없이 메소드 사용 가능

# File 클래스로 파일 처리하기
```java
public class FileManageClass {
    public static void main(String[] args) {
        FileManageClass sample = new FileManageClass();
        String pathName = File.separator + "GodOfJava" +
                          File.separator + "text";
        String fileName = "test.txt";
        sample.checkFile(pathName, fileName);
    }
    
    public void checkFile(String pathName, String fileName) {
        File file = new File(pathName, fileName);
        try {
            System.out.println("create result =" + file.createNewFile());
        } catch(IOException e) {
            e.printStackTrace();
        }
    }
}
```

# InputStream
- abstract 클래스
- 데이터를 읽을 때 InputStream의 자식 클래스 사용
- implements Closeable

# OutputStream
- abstract 클래스
- 데이터를 쓸 때 OutputStream의 자식 클래스 사용
- implements Closeable, Flushable

# Closeable
- Closeable을 구현한 클래스는 리소스 사용 후 close()를 호출해야 한다

# Flushable
- flush() 메소드만 선언되어 있다

# flush()
- 파일 저장시 버퍼를 사용하여 데이터를 쌓아두었다가 한 번에 저장하는데 flush()는 '현재 버퍼의 데이터들을 flush()를 호출한 시점에 전부 저장하라'는 메소드이다

# read()
- 유일한 추상 메소드
- 스트림에서 다음 바이트를 읽음

# 자주 사용하는 InputStream을 확장한 클래스
- FileInputStream
- FilterInputStream
- ObjectInputStream

# Reader와 Writer
- Stream은 byte를 다루기 위한 것이라면 Reader와 Writer는 char 기반의 문자열을 처리하기 위한 클래스이다

# Reader
- implements Readable, Closeable
- InputStream에 선언되어 있는 메소드와 많이 중복됨

# Writer
- implements Appendable, Closeable, Flushable
- OutputStream에 선언된 메소드와 대부분 동일

# Appendable?
- append() 추상 메소드를 가짐
- StringBuilder나 StringBuffer를 처리시 사용

# FileWriter vs BufferedWriter
- FileWriter
  - write()나 append() 호출시마다 파일에 쓰기 때문에 비효율적
- BufferedWriter
  - 버퍼에 저장할 데이터를 보관해두었다가 버퍼가 다차면 데이터를 전부 저장

# IOException은 언제 발생할까?
- 매개변수로 넘어온 파일 이름이 파일이 아닌 경로를 의미할 경우
- 권한 문제
- 파일이 존재하지만 
ㅇㅕ러

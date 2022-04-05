# 27장 Serializable과 NIO

# Serializable이란?
- 클래스가 파일을 읽거나 쓸 수 있도록 다른 서버로 보내거나 받을 수 있도록 하려면, 반드시 Serializable을 implements 해야함
- Serializable을 구현하면 JVM에서 해당 객체는 저장하거나 다른 서버로 전송할 수 있도록 해준다
- Serializable을 구현한 후 SerialVersionUID를 지정하길 권장하며 별도로 지정하지 않으면 자동으로 컴파일됨
```java
static final long serialVersionUID = 1L;
```

# SerialVersionUID를 지정하는 이유?
- 해당 객체의 버전을 명시하는데 사용
- A 서버에서 B 서버로 객체를 전송할 때 해당 객체가 같은지 다른지 SerialVersionUID로 인식한다
- UID가 같더라도 변수의 개수나 타입 등이 다르면 다른 클래스로 인식한다
- Serializable 클래스의 형태가 변경되면 컴파일시 SerialVersionUID가 다시 생성된다
  - SerialVersionUID를 명시해놓은 상황에선 변수가 변경되더라도 예외가 발생하진 않지만 올바른 방법이 아님
    - 데이터가 바뀌면 SerialVersionUID도 변경하는 습관을 가져야함

# transient 예약어
- transient를 사용하여 선언한 변수는 Serializable 대상에서 제외되어 해당 타입의 기본값이 됨

# transient를 사용하는 이유?
- transient 변수는 사용하는데 전혀 문제가 없지만 파일 전송이나 저장하는 작업에는 사용할 수 없도록 한다
- 비밀번호 같은 보안상 전송하거나 저장할 필요가 없는 변수를 transient로 선언한다

# NIO란?
- JDK 1.4에 추가
- stream에 사용하는 IO와 다르게 Channel과 Buffer를 사용한다

```java
public void writeFile(String fileName, String data) throws Exception {
    FileChannel channel = new FileOutputStream(fileName).getChannel();
    byte[] byteData = data.getBytes();
    ByteBuffer buffer = ByteBuffer.wrap(byteData);
    channel.write(buffer);
    channel.close();
}
```
- getChannel()로 FileChannel 객체 생성
- wrap()로 ByteBuffer 객체 생성
- write()에 buffer 객체를 넘겨주면 파일 쓰기 작업

```java
public void readFile(String fileName) throws Exception {
    FileChannel channel = new FileInputStream(fileName).getChannel();
    ByteBuffer buffer = ByteBuffer.allocate(1024);
    channel.read(buffer);
    buffer.flip();
    while(buffer.hasRemaining()) {
        System.out.println((char) buffer.get());
    }
    channel.close();
}
```

- getChannel() 메소드로 FileChannel 객체 생성
- allocate()로 ByteBuffer 객체 생성
  - allocate() 매개변수 만큼의 크기로 생성
- read() 메소드에 buffer 객체를 넘겨줌으로써 buffer에 데이터를 담으라고 지정
- flip()으로 buffer의 맨 앞으로 이동
- get()으로 한 바이트씩 데이터를 읽음

# NIO의 Buffer 클래스
- Buffer 클래스에 선언된 메소드
  - int capacity()
    - 버퍼에 담을 수 있는 크기 리턴
  - int limit()
    - 버퍼에서 읽거나 쓸 수 없는 첫 위치 리턴
    - 초기 limit 값은 capacity(버퍼 크기)와 같음
  - int position()
    - 현재 버퍼의 위치 리턴
  - Buffer flip()
    - 현재 position을 limit 값으로 지정 후 position을 0으로 이동
  - Buffer mark()
    - 현재 position을 mark
  - Buffer reset()
    - 버퍼의 position을 mark한 곳으로 이동
  - Buffer rewind()
    - 현재 버퍼의 position을 0으로 이동
  - int remaining()
    - (limit - position) 계산 결과 리턴
  - boolean hashRemaining()
    - position과 limit 값에 차이가 있을 경우 true
  - Buffer clear()
    - 버퍼를 지우고 현재 position을 0으로 이동하여 limit값을 버퍼 크기로 변경

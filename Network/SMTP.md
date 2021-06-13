# SMTP란?
- 간이 우편 전송 프로토콜(Simple Mail Transfer Protocol)  
- 인터넷에서 이메일을 보내기 위해 이용되는 프로토콜  
- TCP 포트번호: 25  
- 메일 서버간 송수신 뿐만 아니라 메일 클라이언트에서 메일 서버로 메일을 보낼때에도 사용된다.  
- 우리가 메일을 보낼때는 바로 상대방 컴퓨터로 메일을 송신하는 것이 아니라, 중간에 메일 서버를 거치게 되며, 메일 서버에서는 메일이 보관되고 다른 메일 서버로 보내는 우체국 같은 역할을 한다.
- SMTP외에도 POP3/IMAP는 유저가 메일 서버에서 메일을 받기 위한 프로토콜이다.

# POP3란?  
- Post Office Protocol  
- 원격 서버로부터 이메일을 받기 위한 프로토콜  
- TCP 포트번호: 110  
- 원격 서버로부터 이메일을 가져온 후 서버에서 이메일을 삭제한다.  
- 전자메일을 다운로드한 후 동일한 컴퓨터를 사용하여야만 액세스할 수 있다.  

# IMAP란?  
- Internet Message Access Protocol  
- 원격 서버로부터 이메일을 받기 위한 프로토콜  
- TCP 포트번호: 143  
- 온라인 모드와 오프라인 모드를 모두 지원하므로 POP3와 달리 이메일 메시지를 서버에 남겨두었다가 나중에 지울 수 있다.  
- 어디서나 모든 장치에서 전자 메일에 엑세스할 수 있다.  
- POP3에 비해 IMAP는 메일 서버와의 통신 트래픽이 높은 단점을 가지고 있다.  

# Spring Boot + JavaMail  
## 의존성 주입  
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```  
## Java mail관련 Interface or Class  
### MailSender interface   
- 최상위 인터페이스  
### JavaMailSender interface    
- MailSender 하위 interface  
- MimeMessageHelper class와 함께 사용되며 MimeMessage를 만들기 위해 사용된다  
### JavaMailSenderImpl class  
- JavaMailSender interface의 구현 클래스  
### SimpleMailMessage class  
- 보낸 사람, 받는 사람, 참조, 제목 및 텍스트 필드를 포함하는 간단한 메일 메시지를 만드는 데 사용된다  
### MimeMessagePreparator interface  
- MIME 메시지 준비를위한 콜백 인터페이스를 제공  
### MimeMessageHelper class  
- MIME 메시지 생성을 위한 helper 클래스. HTML 레이아웃의 이미지, 일반적인 메일 첨부 파일 및 텍스트 콘텐츠에 대한 지원을 제공  

## SMTP 서버 정보 등록하기  
```properties
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=<login user to smtp server>
spring.mail.password=<login password to smtp server>
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```
- spring.mail.properties.mail.smtp.starttls.enable : TLS connection 사용
- password는 일반 비밀번호가 아닌 앱 비밀번호를 사용해야 한다  
[앱 비밀번호 사용하기](https://support.google.com/accounts/answer/185833)  

## simple email message 보내기
- JavaMailSender  
```java
@Component
public class EmailServiceImpl implements EmailService{

    private JavaMailSender emailSender;

    public EmailServiceImpl(JavaMailSender emailSender) {
        this.emailSender = emailSender;
    }

    public void sendSimpleMessage(String to, String subject, String text) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setFrom("test@mail.com");
        message.setTo(to);
        message.setSubject(subject);
        message.setText(text);
        emailSender.send(message);
    }
}
```

## 첨부파일 붙여서 메일 보내기  
- 파일이 첨부된 메일은 SimpleMailMessage 대신 JavaMail 라이브러리의 MIME multipart message를 사용한다  
- org.springframework.mail.javamail.MimeMessageHelper 클래스를 사용  


## Ref.  
- [위키백과 - SMTP](https://ko.wikipedia.org/wiki/%EA%B0%84%EC%9D%B4_%EC%9A%B0%ED%8E%B8_%EC%A0%84%EC%86%A1_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)  
- [위키백과 - POP](https://ko.wikipedia.org/wiki/%ED%8F%AC%EC%8A%A4%ED%8A%B8_%EC%98%A4%ED%94%BC%EC%8A%A4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)  
- [위키백과 - IMAP](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%EB%A9%94%EC%8B%9C%EC%A7%80_%EC%A0%91%EC%86%8D_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)  
- https://cheershennah.tistory.com/104  
- [Guide to Spring Email](https://www.baeldung.com/spring-email)  

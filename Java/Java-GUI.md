# Java-GUI-Programming  
## 1. 기본 자바 로직
``` java
import java.awt.Color;
import javax.swing.JFrame;

public class SwingGibon extends JFrame{

  public SwingGibon(String title) {
    super(title);
    this.setBounds(700, 100, 300, 300); // 시작위치 x, y, 크기 w, h
    // this.getContentPane().setBackground(Color.DARK_GRAY); // 프레임 위에 있는 패널의 색상변경
    this.getContentPane().setBackground(new Color(211, 225, 208)); // 프레임 위에 있는 패널의 색상변경
    this.setDesign(); // 디자인 코드
    this.setVisible(true); // 보이게 하기
    this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 프로그램을 종료해주는 메서드
  }

  public void setDesign() {
    // 레이아웃 없애기
    this.setLayout(null);
  
  }

  public static void main(String[] args) {
    new SwingGibon("기본창");
  }
}
```

## 2. 자바 라이브러리
  ### 2.1 javax.swing 객체  
  #### - JFrame  
    프레임 생성
  #### - JLabel
    패널에 문자열 생성
  #### - JTextField
    패널에 문자를 입력할 수 있는 빈칸 생성
  #### - JComboBox
    패널에 특정 선택지중 한가지를 선택할 수 있는 박스 생성  
  #### - JButton  
    패널에 퍼튼 생성
  #### - JTable
    테이블 생성
  #### - DefaultTableModel  
    테이블을 행단위로 쓰거나 삭제(JTable 자체를 다시 생성하지 않아도됨)
  #### - JScrollPane  
    테이블영역 밖의 데이터도 스크롤하여 읽을 수 있도록 함  
    
  ### 2.2 java.awt 객체  
  #### - Image / ImageIcon
  
  #### - Canvas
  
### 2.3 이벤트
   **방법1) ActionListner 인터페이스를 implements**  
   
    -> actionPerformed(ActionEvent e) 오버라이딩  
    -> Object 참조변수에 getSource를 이용해서 이벤트 소스를 저장...조건문을 이용해서 이벤트 소스마다 수행될 로직을 선언  
    -> UI디자인 객체마다 addActionListener 메서드를 사용해서 actionPerformed 메서드 호출   
   
   **방법2) 익명내부클래스 선언  
  
  #### - JOptionPane.showMessageDialog()
    조건에 해당하는 이벤트 발생시 경고창 출력
  #### - FileDialog

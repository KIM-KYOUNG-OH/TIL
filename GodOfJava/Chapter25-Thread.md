# 25장 스레드

# 프로세스
- JVM이 시작되면 자바 프로세스가 시작

# 스레드
- 프로세스안에 여러 작업들의 단위
- main(), GC도 스레드에 해당

# 왜 스레드를 사용할까?
- 하나의 작업을 동시에 작업하려고할 때 여러 프로세스를 띄우게 되면 각각 메모리를 할당해주어야 하므로 메모리 낭비가 발생할 수 있다
- JVM은 32~64MB의 메모리를 점유하고 스레드는 1MB 이내 메모리를 사용
- 대부분의 작업을 단일 스레드보다 멀티 스레드로 작업하는게 더 빠름

# 스레드를 생성하는 방법
1. Runnable 인터페이스 사용
  - implements로 다중 상속
2. Thread 스레드 사용
  - extends로 단일 상속

### run()
- 스레드가 시작하면(start()호출) 동작하는 시작점

### start()
- 스레드를 시작하는 메서드

# 스레드의 특징
- 스레드1 이후에 스레드2가 시작되도 스레드가 종료되길 기다리지 않기 때문에 누가 먼저 종료될지 알 수 없다

```java
public class RunnableSample implements Runnable {
    public void run() {
        System.out.println("This is RunnableSample's run() method");
    }
}
```

```java
public class ThreadSample extends Thread {
    public void run() {
        System.out.println("This is ThreadSample's run() method");
    }
}
```

```java
public class RunMultiThread {
    public void static main(String args[]) {
        RunMultiThread sample = new RunMultiThread();
        sample.runMultiThread();
    }
    
    public void runMultiThread() {
        RunnableSample[] runnable = new RunnableSample[5];
        ThreadSample[] thread = new ThreadSample[5];
        for(int i = 0; i < 5; i++) {
            runnable[i] = new RunnableSample();
            thread[i] = new ThreadSample();
            new Thread(runnable[i]).start();
            Thread[i].start();
        }
        System.out.println("RunMultiThread.runMultiThread() method is end");
    }
}
```

```console
This is RunnableSample's run() method
This is RunnableSample's run() method
This is RunnableSample's run() method
This is ThreadSample's run() method
RunMultiThread.runMultiThread() method is end
This is ThreadSample's run() method
This is ThreadSample's run() method
This is RunnableSample's run() method
This is ThreadSample's run() method
This is RunnableSample's run() method
This is ThreadSample's run() method
```

- 결과는 PC 사양에 따라 다르고 메서드를 수행할 때마다 달라짐
- run() 메서드가 완료되면 스레드는 종료됨

# Thread 생성자
- 스레드 이름을 지정하지 않으면 "Thread-n"을 가짐

# sleep()
- static 메서드
- 매개변수 시간만큼 대기함. 단, DaemonThread는 제외
- try-catch로 감싸주어야 함

# Daemon Thread
- 데몬 스레드는 다른 실행중인 일반 스레드가 없을 경우 종료됨
- sleep()의 영향을 받지 않음

# 왜 데몬 스레드를 사용할까?
- 모니터링하는 스레드처럼 주요 스레드가 종료되면 더이상 의미가 없는 부가적인 스레드를 데몬스레드로 지정해서 주요 스레드가 종료하면 자동으로 프로세스가 종료되도록 한다

# Synchronized를 사용하는 방법
- 메소드 자체를 synchronized로 선언하는 방법
  - 락의 범위가 커서 대기시간이 길어지므로 비추
- 메소드내 특정 문장만 synchronized로 감싸는 방법

```java
public synchronized void plus(int value) {
    anount += value;
}
```

- synchronized를 사용하면 스레드가 종료될 때까지 다른 스레드의 접근을 막는다

# synchronized의 단점
- synchronized를 남용하면 불필요한 대기 시간을 야기한다
- 동기화를 적용할 부분만 synchronized 블록 처리하여 대기 시간을 최소화하자

```java
Object lock = new Object();  // lock은 문지기 같은 역할
public void plus(int value) {
    synchronized(lock) {
        amount += value;
    }
}
```

- 하나의 lock 객체만을 사용하면 lock의 범위가 커지므로 lock 객체를 여러 개 사용하여 lock의 범위를 분산한다

```java
private int amount;
private int interest;
pirvate Object interestLock = new Object();
pirvate Object amountLock = new Object();
public void addInterest(int value){
    synchronized(interestLock) {
        interest += value;
    }
}

public void plus(int value){
    synchronized(amountLock) {
        amount += value;
    }
}
```

# synchronized 사용시 주의점
- 동기화는 하나의 자원을 여러 스레드가 공유할 때 사용하는 것이지 그렇지 않은 경우에는 사용하지 않는 것이 좋다
- StringBuffer는 동기화 지원, StringBuilder는 동기화 미지원
  - 즉, StringBuffer는 하나의 문자열 객체를 여러 스레드에서 공유해야 하는 경우에만 사용하자

```java
CommonCalculate calc1 = new CommonCalculate();
ModifyAmountThread thread1 = new ModifyAmountThread(calc1, true);

CommonCalculate calc2 = new CommonCalculate();
ModifyAmountThread thread2 = new ModifyAmountThread(calc2, true);

// 이 경우는 각 스레드가 별도의 객체를 참조하므로 동기화가 필요없다
```

```java
CommonCalculate calc = new CommonCalculate();
ModifyAmountThread thread1 = new ModifyAmountThread(calc1, true);
ModifyAmountThread thread2 = new ModifyAmountThread(calc1, true);

// 이 경우는 두 스레드가 하나의 자원을 공유하므로 동기화 필요성이 있다
```

# 스레드를 통제하는 메서드
- getState()
  - 스레드 상태 확인
- join()
  - 수행중인 스레드가 중지할 때까지 대기
- interrupt()
  - 수행중인 스레드에 중지 요청

# 스레드 상태 상수
- NEW
- RUNNABLE
- BLOCKED
- WAITING
- TIMED_WAITING
- TERMINATED

```java
public void checkJoin() {
    SleepThread thread = new SleepThread(2000);
    try {
      thread.start();
      thread.join(500);
      thread.interrupt();
      System.out.println("thread state(after join)=" + thread.getState());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

- join()의 파라미터가 500인 경우
  - InterruptedException 발생
  - 발생 시점에 thread 상태 = TIMED_WAITING
  - start() 실행시 결과를 받을 때까지 기다리지 않고 다음 줄의 코드가 실행됨
- join()의 파라미터가 2000보다 큰 경우
  - join()으로 통제 대기가 끝나기 전에 스레드가 종료되므로 interrupt가 동작하지 않음
  - 정상 TERMINATED 됨

# 그 외 스레드 통제 static 메서드
- void checkAccess()
  - 스레드 수정 권한 확인
- boolean isAlive()
- boolean isInterrupted()
  - 타 스레드의 종료 여부 확인
- boolean interrupted()
  - 스레드 본인의 종료 여부 확인
- int activeCount()
- Thread currentThread()
- void dumpStack()

# Object 내부에 선언된 스레드 관련 메서드
- void wait()
  - 다른 스레드가 해당 object에 대한 notify()나 notifyAll()을 호출할 때까지 현재 스레드를 대기함
  - join()처럼 일정시간 만큼 대기할 수도 있다
- notify()
  - object 객체 모니터에 대기하고있는 단일 스레드를 깨움
- notifyAll()
  - object 객체 모니터에 대기하고있는 모든 스레드를 깨움

# ThreadGroup
- 스레드 관리를 용이하게 하기 위한 클래스
- tree 구조를 가짐
- 여러 스레드를 스레드 그룹으로 묶어 관리를 용이하게 해주는 static 메서드가 존재

# 심화
- ThreadLocal
- volatile

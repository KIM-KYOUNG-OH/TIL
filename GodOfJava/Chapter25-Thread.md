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

# ThreadGroup

# sleep()

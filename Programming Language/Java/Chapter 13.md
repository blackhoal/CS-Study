# 1. 프로세스와 쓰레드
## 1-1. 프로세스 VS 쓰레드
- 프로세스
    - 사용자가 작성한 프로그램이 운영체제에 의해 메모리 공간을 할당받아 실행 중인 프로그램
    - 프로그램에 사용되는 데이터와 메모리 등의 자원(RESOURCE)과 쓰레드로 구성 → 공장
- 쓰레드
    - 프로세스(process) 내에서 실제로 작업을 수행하는 주체
    - 모든 프로세스에는 최소 한 개 이상의 쓰레드가 존재하여 작업을 수행 → 노동자
    - 싱글 쓰레드 프로세스 = 공장(resource) + 1명의 노동자
    - 멀티 쓰레드 프로세스 = 공장(resource) + N명의 노동자
## 1-2. 공장 2 + 노동자 1 VS 공장 1 + 노동자 2?
- 하나의 새로운 프로세스를 생성하는 것보다 하나의 새로운 쓰레드를 생성하는 것이 효율성 우수
- 같은 프로세스 내의 쓰레드들은 서로 자원을 공유

## 1-3. 멀티 쓰레드의 장단점
|장점|단점|
|:------:|:------:|
|시스템 자원을 보다 효율적으로 사용 가능|동기화(synchronization)에 주의 필요|
|사용자에 대한 응답성(responseness)이 향상|교착상태(dead-lock) 발생에 주의 필요|
|작업이 분리되어 코드의 간결성 증대|각 쓰레드가 효율적으로 고르게 실행될 수 있도록 조절 필요|
- 멀티 쓰레딩 : 하나의 프로세스 내에서 여러 쓰레드가 동시에 작업을 수행하는 것
- 서버 프로그램의 경우 멀티 쓰레드로 작성하는 것은 필수
    - 하나의 서버 프로세스가 여러 개의 쓰레드를 생성하여 쓰레드와 사용자의 요청이 일대일로 처리되도록 프로그래밍 필요
- 여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하므로 발생할 수 있는 동기화, 교착상태 등의 문제를 고려하여 프로그래밍 필요

<br>

# 2. 쓰레드의 구현과 실행
```java
class ThreadWithClass extends Thread {
    // 쓰레드를 통해 작업하고 싶은 내용을 run() 메소드에 작성
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(getName()); // 현재 실행 중인 쓰레드의 이름을 반환
            try {
                Thread.sleep(10);          // 0.01초간 쓰레드를 정지
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class ThreadWithRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()); // 현재 실행 중인 쓰레드의 이름을 반환
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Thread01 {
    public static void main(String[] args){
        ThreadWithClass thread1 = new ThreadWithClass();       // Thread 클래스를 상속받는 방법
        Thread thread2 = new Thread(new ThreadWithRunnable()); // Runnable 인터페이스를 구현하는 방법

        thread1.start(); // 쓰레드의 실행
        thread2.start(); // 쓰레드의 실행
    }
}
/*
    [Output]
    Thread-0

    Thread-1

    Thread-0

    Thread-1

    Thread-0

    Thread-1

    Thread-0

    Thread-1

    Thread-0

    Thread-1
*/
```
- 쓰레드 생성 방법
    - Runnable 인터페이스를 통해 구현
        - Runnable 인터페이스 : 몸체가 없는 메소드인 run() 메소드 단 하나만을 가지는 간단한 인터페이스
        - Runnable을 구현하면 Thread 클래스의 static 메소드인 currentThread( )를 호출하여 쓰레드에 대한 참조를 얻어야만 호출이 가능
    - Thread 클래스를 상속받아 구현
        - Thread 클래스를 상속받으면 자손 클래스에서 조상인 Thread 클래스의 메소드를 직접 호출 가능
- Thread 클래스를 상속받으면 다른 클래스의 상속이 불가
    - 일반적으로 Runnable 인터페이스를 구현하는 방법을 통해 쓰레드를 생성
- 모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택을 필요
    - 새로운 쓰레드를 생성하고 실행시킬 때마다 새로운 호출스택이 생성되고 쓰레드가 종료되면 작업에 사용된 호출스택은 소멸
- 스케줄러는 실행대기중인 쓰레드들의 우선순위를 고려하여 실행순서와 실행시간을 결정
    - 각 쓰레드는 작성된 스케줄에 따라 자신의 순서가 되면 지정된 시간동안 작업을 수행
    - 실행중인 사용자 쓰레드가 하나도 없을 때 프로그램은 종료
- 쓰레드에서 예외가 발생하여 종료되어도 다른 쓰레드의 실행에는 영향 X

<br>

# 3. start()와 run()
```java
class ThreadTest
{
	public static void main(String args[])
    {
    	MyThread t1 = new MyThread();
        t1.start();
    }
}

class MyThread extends Thread
{
	public void run(){}
}
```
- 쓰레드를 생성 후 start()를 호출해야 쓰레드 시작
- OS의 스케줄러에 의해 순서가 결정
    - 먼저 입력된 것이 먼저 실행 X
- 실행 메커니즘
    - start()를 호출하면 새로운 콜스택이 생성
    - 새롭게 생성된 콜스택에 run()이 생성
    - start()는 종료되고 run()이 실행(main과 새로운 콜스택의 실행은 독립적)

<br>

# 4. 싱글 쓰레드와 멀티 쓰레드
```java
// 싱글 쓰레드 
class TestThread{
  public static void main(String[] args){

    for(int i = 0; i<50; i++){
      System.out.print("-");
    }

    for(int i = 0; i<50; i++){
      System.out.print("|");
    }
  }
}

// 멀티 쓰레드
class TestThread{
  public static void main(String[] args){

    ThreadEx1 th1 = new ThreadEx1();
    ThreadEx2 th2 = new ThreadEx2();

    th1.start();
    th2.start();

  }
}

class ThreadEx1 extends Thread{
  public void run(){
    for(int i=0; i<300; i++){
      System.out.print(new String("-"));
    }
  }
}

class ThreadEx2 implements Thread{
  public void run(){
    for(int i=0; i<300; i++){

      System.out.println(new String("|"));
    }
  }
}
```
- 쓰레드 간의 작업전환에 시간이 소요되므로 두 개의 쓰레드로 작업한 시간이 싱글 쓰레드로 작업한 시간보다 더 많은 시간 소요
    - 싱글 코어에서 단순히 CPU만을 사용하는 계산작업이라면 멀티 쓰레드보다 싱글 쓰레드로 프로그래밍하는 것이 더 효율적
- 싱글 코어의 경우 멀티 쓰레드여도 하나의 코어가 번갈아가며 작업을 수행하는 것이므로 두 작업은 겹치지 않고 수행
    - 멀티 코어에서는 멀티 쓰레드로 작업을 수행하면 동시에 두 쓰레드가 수행 가능
    - 두 쓰레드가 서로 다른 자원을 사용하는 작업의 경우 멀티 쓰레드 프로세스가 더 효율적
- 각 프로세스의 실행 시간과 실행 순서는 OS의 프로세스 스케줄러의 영향을 받으며 쓰레드는 JVM의 쓰레드 스케줄러에 의해 각 쓰레드의 실행시간과 실행순서가 결정
    - 상황에 따라 프로세스와 쓰레드에 할당되는 시간은 불확실

<br>

# 5. 쓰레드의 우선순위
```java
void setPriority(int newPrioroty)	// 스레드의 우선순위를 지정한 값으로 변경
int getPriority()					// 스레드의 우선순위를 반환

public static final int MAX_PRIORITY = 10;	// 최대우선순위
public static final int MIN_PRIORITY = 1;	// 최소우선순위
public static final int NORM_PRIORITY = 5;	// 보통우선순위
```
- 쓰레드는 우선순위(priority)라는 속성(멤버변수)을 보유
    - 작업의 중요도에 따라 쓰레드의 우선순위를 다르게 지정하여 특정 쓰레드에 더 많은 작업시간을 배치 가능
    - 우선순위의 값에 따라 쓰레드가 얻는 실행시간이 상이
- 쓰레드의 우선순위 범위는 1~10이며 숫자가 낮을수록 우선순위 ↑
    - Windows OS의 경우 32까지 존재
- 쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속
    - main 메소드를 수행하는 쓰레드는 우선순위 : 5
    - main 메소드 내에서 생성하는 쓰레드의 우선순위 : 5
- 쓰레드를 실행하기 전에만 우선순위를 변경 가능
- 멀티 코어에서는 쓰레드의 우선순위 효과 X
    - 쓰레드에 대한 우선순위 대신 작업에 우선순위를 두어 PriorityQueue를 사용하는 방식이 더 효율적

<br>

# 6. 쓰레드 그룹(thread group)
|생성자 / 메소드|설명|
|:------:|:------:|
|ThreadGroup(String name)|지정된 이름의 새로운 스레드 그룹을 생성|
|ThreadGroup(ThreadGroup parent, String name)|지정된 스레드 그룹에 포함되는 새로운 스레드 그룹을 생성|
|int activeCount()|스레드 그룹에 포함된 활성상태에 있는 스레드의 수를 반환|
|int activeGroupCount()|스레드 그룹에 포함된 활성상태에 있는 스레드 그룹의 수를 반환|
|void checkAccess()|현재 실행중인 스레드가 스레드 그룹을 변경할 권한이 있는지 체크|
|void destroy()|스레드 그룹과 하위 스레드 그룹까지 모두 삭제|
|int enumerate(Thread[] list) int enumerate(Thread[] list, boolean recurse) int enumerate(ThreadGroup[] list) int enumerate(ThreadGroup[] list, boolean recurse)|스레드 그룹에 속한 스레드 또는 하위 스레드 그룹의 목록을 지정된 배열에 담고 그 개수를 반환, 두 번째 매개변수인 recurse의 값을 true로 하면 스레드 그룹에 속한 하위 스레드 그룹에 스레드 또는 스레드 그룹까지 배열에 담는다.|
|int getMaxPrioroty()|스레드 그룹의 최대우선순위를 반환|
|String getName()|스레드 그룹의 이름을 반환|
|ThreadGroup getParent()|스레드 그룹의 상위 스레드그룹을 반환|
|void interrupt()|스레드 그룹에 속한 모든 스레드그룹을 반환|
|boolean isDaemon()|스레드 그룹이 데몬 스레드그룹인지 확인|
|boolean isDestroyed()|스레드 그룹이 삭제되었는지 확인|
|void list()|스레드 그룹에 속한 스레드와 하위 스레드그룹에 대한 정보를 출력|
|voboolean parentOf(ThreadGroup g)|지정된 스레드 그룹의 상위 스레드 그룹인지 확인|
|void setDaemon(boolean daemon)|스레드 그룹을 데몬 스레드그룹으로 설정/해제|
|void setMaxPriority(int pri)|스레드 그룹의 최대우선순위를 설정|

- 서로 관련된 쓰레드를 그룹으로 묶어서 다루기 위한 것
- 쓰레드 그룹에 다른 쓰레드 그룹을 포함 가능
    - 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경이 가능하지만 다른 쓰레드 그룹의 쓰레드는 변경 불가
- 모든 쓰레드는 반드시 하나의 쓰레드 그룹에 포함
    - 쓰레드 그룹을 지정하지 않고 생성한 쓰레드는 main 쓰레드 그룹에 소속
- 자신을 생성한 쓰레드(부모 쓰레드)의 그룹과 우선순위를 상속
- JVM은 자바 어플리케이션이 실행되면 main과 system이라는 쓰레드 그룹을 생성하고 JVM 운영에 필요한 쓰레드를 생성하여 쓰레드 그룹에 포함

<br>

# 7. 데몬 쓰레드(daemon thread)
```java
// 스레드가 데몬 스레드인지 확인 -> 데몬스레드일 경우 true를 반환
boolean isDaemon()

// 스레드를 데몬 스레드로 또는 사용자 스레드로 변경 -> 매개변수 on을 true로 지정하면 데몬 스레드로 설정
void setDaemon(boolean on)
```
- 일반 쓰레드(non-daemon-thread)의 작업을 돕는 보조적인 역할을 하는 쓰레드
    - 일반 쓰레드가 모두 종료되면 데몬 쓰레드는 강제적으로 자동 종료
- 일반 쓰레드의 작성방법과 실행방법이 같으며 쓰레드를 생성한 후 start( )를 호출하기 전에 setDaemon(true)를 호출하여 생성
    - setDaemon()을 실행하지 않으면 IllegalThreadStateException 발생
- 가비지 컬렉터, 자동 저장, 화면 자동갱신 등에 사용

<br>

# 8. 쓰레드의 실행제어
## 8-1. 쓰레드의 실행제어
|메소드|설명|
|:------:|:------:|
|static void sleep(long millis)|지정된 시간동안 스레드를 일시정지 / 지정한 시간 후 자동적으로 다시 실행대기상태로 변경|
|static void sleep(long millis, int nanos)|"|
|void join()|지정된 시간동안 스레드 실행 / 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 스레드로 다시 돌아와 계속 실행|
|void join(long millis)|"|
|void join(long millis, int nanos)|"|
|void interrupt()|sleep()이나 join()에 의해 일시정지상태인 스레드를 깨워 실행대기상태로 변경 / 해당 스레드에서 interruptedException이 발생하여 일시정지 상태를 탈출|
|void stop()|스레드를 즉시 종료|
|void suspend()|스레드를 일시정지 / resume()을 호출하면 다시 실행대기상태로 변경|
|static void yield()|실행 중에 자신에게 주어진 실행시간을 다른 스레드에게 양보(yield)하고 자신은 실행대기상태로 변경|

## 8-2. 쓰레드의 상태
|상태|설명|
|:------:|:------:|
|NEW|스레드가 생성되고 아직 start()가 호출되지 않은 상태|
|RUNNABLE|실행 중 또는 실행 가능한 상태|
|BLOCKED|동기화 블럭에 의해 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태)|
|WAITING|스레드의 작업이 종료하지 않았지만 실행이 불가능한(unrunnable) 일시정지 상태 / TIMED_WAITING : 일시정지시간이 지정된 경우|
|TIMED_WAITING|"|
|TERMINATED|스레드의 작업이 종료된 상태|

<br>

# 9. 쓰레드의 동기화
## 9-1. synchronized를 이용한 동기화
## 9-2. wait()과 notify()
## 9-3. Lock과 Condition을 이용한 동기화
## 9-4. volatile
## 9-5. fork & join 프레임웍

# Reference
[참조 1](https://orangebluestyle.tistory.com/45)  
[참조 2](https://jungdami-ing.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%9D%98-%EC%A0%95%EC%84%9D-%EC%9A%94%EC%95%BD%EC%A7%91-13-%EC%8A%A4%EB%A0%88%EB%93%9Cthread)  
[참조 3](https://haeng-on.tistory.com/40)  
[참조 4](http://www.tcpschool.com/java/java_thread_concept)  
[참조 5](https://recoderr.tistory.com/44?category=887628)  

# 1. java.lang 패키지
## 1-1. Object 클래스
|메소드|설명|
|------|---|
|protected Object clone()|객체 자신의 복사본을 반환|
|public boolean equals(Object obj)|객체 자신과 주어진 객체(obj)를 비교하여 같으면 true, 다르면 false 반환|
|protected void finalize()|객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출|
|public Class getClass()|객체 자신의 클래스 정보를 담고 있는 Class 인스턴스를 반환|
|public int hashCode()|객체 자신의 해시코드를 반환|
|public String toString()|객체 자신의 정보를 문자열로 반환|
|public void notify()|객체 자신을 사용하려고 기다리는 쓰레드를 하나만 실행|
|public void notifyAll()|객체 자신을 사용하려고 기다리는 모든 쓰레드를 실행|
|public void wait()|다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout, nanos) 동안 대기|
|public void wait(long timeout)||
|public void wait(long timeout, int nanos)||
- 모든 클래스의 최고 조상 클래스이므로 Object 클래스의 멤버는 모든 클래스에서 바로 사용이 가능

### equals(Object obj)
- 매개변수로 객체의 참조변수를 받아 객체 자신과 비교하여 결과를 boolean값으로 반환
- Object 클래스로부터 상속받은 equals 메소드는 두 참조변수에 저장된 값(주소값)이 같은지를 판단하는 기능만 가능
- 인스턴스가 가진 값을 통해 비교하려면 equals()를 오버라이딩하여 사용
- String 클래스 또한 Object 클래스의 equals 메소드를 오버라이딩하여 문자열 값을 비교

### hashCode()
- 객체의 주소값을 이용하여 해시코드를 만들어 반환하므로 서로 다른 두 객체는 같은 해시코드를 가지는 것이 불가능
- 클래스의 인스턴스변수 값으로 객체의 일치를 판단하려면 equals()처럼 오버라이딩하여 사용

### toString()
- 인스턴스에 대한 정보를 문자열로 제공할 목적으로 정의한 메소드
- Object 클래스에서 toString()의 접근 제어자가 public이므로, 이를 오버라이딩하는 클래스에서도 toString()의 접근 제어자를 public으로만 정의 가능

### clone()
- 자신을 복제하여 새로운 인스턴스를 생성
- Object 클래스의 clone 메소드는 단순히 인스턴스변수의 값만을 복사하므로 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 성립 X
- 변수는 주소를 복제하기 때문에 원래의 인스턴스에 영향을 미친다. 이런경우 clone메서드를 오버라이딩해야 한다.
- clone()을 사용하려면 먼저 복제할 클래스가 Clonable 인터페이스를 구현하며, clone()를 오버라이딩하여 접근 제어자를 protected에서 public으로 변경해야 상속관계가 없는 다른 클래스에서 clone() 호출 가능

## 1-2. String 클래스

## 1-3. StringBuffer 클래스와 StringBuilder 클래스

## 1-4. Math 클래스

## 1-5. Wrapper 클래스

# 2. 유용한 클래스

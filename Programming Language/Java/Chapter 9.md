# java.lang 패키지
## 1. Object 클래스
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

### getClass()
```java
// 생성된 객체로부터 얻는 방법
Class cObj = new Card().getClass(); 
// 클래스 리터럴(*.class)로부터 얻는 방법
Class cObj = Card.class; 
// 클래스 이름으로부터 얻는 방법
Class cObj = Class.forName("Card"); 

// new 연산자를 이용한 객체 생성
Card c = new Card(); 
// Class 객체를 이용한 객체 생성
Card c= Card.class.newInstance(); 
```
- 자신이 속한 클래스의 Class객체를 반환
    - Class 객체는 클래스의 모든 정보를 담고 있으며, 클래스 당 한 개만 존재
    - 클래스 파일이 '클래스 로더'에 의해서 메모리에 올라갈 때 자동으로 생성
- Class 객체를 이용하면 클래스에 대한 모든 정보를 얻을 수 있으므로 Class 객체를 통해 객체를 생성하고 메소드를 호출하는 등 보다 동적인 코드를 작성 가능

### 공변 반환 타입(covariant return type)
```java
// (1) 반환 타입을 Object에서 Point로 변경
public Point clone(){ 
    Object obj = null;
    try{
    	obj = super.clone();
    }catch(CloneNotSupportedException e){}
    // (2) Point 타입으로 형 변환
    return (Point) obj; 
}
```
- 오버라이딩 시 조상 메소드의 반환 타입을 자손 클래스 타입으로 변경할 수 있는 것
- 공변 변환 타입을 사용 시 실제로 반환되는 자손 객체의 타입으로 반환 가능하므로 번거로운 형 변환이 감소한다는 장점 존재

### 얕은 복사와 깊은 복사
- 얕은 복사
    - 객체를 복사할 때, 해당 객체만 복사하여 새 객체를 생성하는 방식
    - 객체에 저장된 값만 그대로 복제하며 객체가 참조하고 있는 객체까지는 복제 X
    - 복사된 객체의 인스턴스 변수는 원본 객체의 인스턴스 변수와 같은 메모리 주소를 참조
    - 해당 메모리 주소의 값이 변경되면 원본 객체 및 복사 객체의 인스턴스 변수 값은 동일하게 변경
- 깊은 복사
    - 객체를 복사할 때, 해당 객체와 인스턴스 변수까지 복사하는 방식
    - 원본이 참조하고 있는 객체까지 복제
    - 전체를 복사하여 새 주소에 담으므로 참조를 공유 X


## 2. String 클래스
-  문자열을 저장하고 이를 다루는데 필요한 메소드를 함께 제공
- String 인스턴스의 내용은 변경 불가하므로 문자열 간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 String 클래스 대신 StringBuffer 클래스 사용을 권장

### 문자열의 비교
- 문자열 생성 방식
    - 문자열 리터럴을 지정하는 방식
    - String 클래스의 생성자를 이용하여 생성하는 방식
- String 클래스의 생성자를 이용한 경우에는 new 연산자에 의해서 메모리 할당이 이루어지므로 항상 새로운 String 인스턴스가 생성
- 문자열 리터럴은 이미 존재하는 것을 재사용하는 것

### 문자열 리터럴
```java
// "AAA"라는 문자열을 담고 있는 String 인스턴스가 하나 생성된 후 참조변수 s1, s2, s3는 모두 이 String 인스턴스를 참조
class StringEx2 {
	public static void main(String args[]) {
		String s1 = "AAA";
		String s2 = "AAA";
		String s3 = "AAA";
		String s4 = "BBB";
	}
}
```
- 자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시 클래스 파일에 저장
- 같은 내용의 문자열 리터럴은 한번만 저장

### 빈 문자열
```java
// 길이가 0인 char 배열
char[] chArr = new char[0]; 
// 길이가 0인 int 배열
int[] iArr = {}; 

// 초기화
String s = "";
char c = ' ';
```
- 내용이 없는 문자열
- 크기가 0인 배열을 생성하는 것은 모든 타입이 가능
- `String str=“”;`은 가능하지만 `char c = ‘’;`은 불가능 
    - char형 변수에는 반드시 하나의 문자를 지정 필요
- String은 참조형의 기본값인 null보다 빈 문자열로 초기화하고 char형은 기본값인 ‘\u0000’보다 공백으로 초기화

### join과 StringJoiner
```java
/* 예제 - join */
String animals = "dog,cat,bear";
// 문자열을 ','를 구분자로 나눠서 배열에 저장 [dog, cat, bear]
String[] arr = animals.split(","); 
// 배열의 문자열을 '-'로 구분해서 결합
String str = String.join("-", arr); 
// 출력 : dog-cat-bear
System.out.println(str);

/* 예제 - StringJoiner */
StringJoiner sj = new StringJoiner(",", "[", "]");
String[] strArr = {"aaa", "bbb", "ccc"};

for(String s : strArr)
	sj.add(s.toUpperCase());

// 출력 : AAA,BBB,CCC
System.out.println(sj.toString()); 
```

### format
```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3,5,3+5);
// 출력 : 3 더하기 5는 8입니다.
System.out.println(str); 
```

### 기본형 <-> String 변환
```java
/* 기본형 -> String */
int i = 100;
// 방법 1
String str1 = i + ""; 
// 방법 2
String str2 = String.valueOf(i);

/* String -> 기본형 */
// 방법 1
int i = Integer.parseInt("100"); 
// 방법 2
int i2 = Integer.valueOf("100"); 
```

## 3. StringBuffer 클래스와 StringBuilder 클래스
### StringBuffer
```java
StringBuffer sb = new StringBuffer("abc");
// sb에 문자열 "123"을 추가하면 append()는 반환타입이 StringBuffer인데 자신의 주소를 반환
sb.append("123");  
                  
StringBuffer sb2 = sb.append("ZZ"); // sb의 내용 뒤에 "ZZ"를 추가한다.
System.out.println(sb); // abc123ZZ
System.out.println(sb2); // abc123ZZ

/* StringBuffer 인스턴스의 문자열 비교 */
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

String s = sb.toString();
String s2 = sb2.toString();
System.out.println(s.equals(s2)); // true
```
- String과 같이 문자형 배열(char[])을 내부적으로 보유
- String과 달리 내용을 변경 가능
- 인스턴스를 생성할 때 버퍼(배열)의 크기를 넉넉하게 지정하는 것이 유리
    - 버퍼가 작으면 작업 중에 더 큰 배열의 생성이 필요하므로 성능 저하
- equals()는 오버라이딩 X, toString()는 오버라이딩 O
    - StringBuffer 인스턴스의 문자열을 비교하려면 StringBuffer 인스턴스의 toString()를 호출하여 String 인스턴스를 얻은 후 equals()를 사용하여 비교

### StringBuilder
- StringBuffer에서 쓰레드의 동기화만 제거한 클래스이며 그 외에는 기능이 완전하게 동일
- StringBuffer는 멀티 쓰레드에 안전(thread safe)하도록 동기화
    - 멀티 쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 저하
- StringBuffer도 충분히 성능이 좋으므로 성능 향상이 반드시 필요한 경우에만 StringBuilder를 사용

## 4. 래퍼(Wrapper) 클래스
```java
public final class Integer extends Number implements Comparable{
    	...    
	private int value;
    	...
}
```
- 기본 자료형의 값을 객체로 변환할 때 사용
- 내부적으로 기본형(primitive type) 변수룰 보유
- 래퍼 클래스는 equals()가 오버라이딩이 되어 있어 주소 값이 아닌 객체가 가진 값을 비교
- 비교 연산자는 사용이 불가하며 대신 compareTo()를 사용
    - compareTo() : 같으면 0, 왼쪽 값이 크면 양수, 왼쪽 값이 작으면 음수이며 정렬에 사용
- toString()도 오버라이딩이 되어 있어 객체가 가지고 있는 값을 문자열로 변환하여 반환
- 래퍼 클래스는 MAX_VALUE, MIN_VALUE, SIZE, BYTES, TYPE 등의 static 상수를 공통적으로 보유

### Number 클래스
```java
public abstract class Number implements java.io.Serializable {
  public abstract int intValue();
  public abstract long longValue();
  public abstract float floatValue();
  public abstract double doubleValue();

  public byte byteValue() {
    return (byte)intValue();
  }

  public short shortValue() {
    return (short)intValue();
  }
}
```
- 추상 클래스이며 내부적으로 숫자를 멤버 변수로 갖는 래퍼 클래스의 조상 클래스
- Number 클래스의 자손으로 래퍼 클래스, BigInteger, BigDecimal 등이 존재
    - BigInteger: long으로도 다룰 수 없는 큰 범위의 정수를 처리하기 위한 클래스
    - BigDecimal: double로도 다룰 수 없는 큰 범위의 부동 소수점 수를 처리하기 위한 클래스
- 객체가 가진 값을 숫자와 관련된 기본형으로 변환하여 반환하는 메소드를 정의

### 문자열을 숫자로 변환
|문자열 → 기본형|문자열 → 래퍼 클레스|
|------|---|
|byte b = Byte.parseByte("100");|Byte b = Byte.valueOf("100");|
|short s = Short.parseShort("100");|Short s = Short.valueOf("100");|
|int i = Integer.parseInt("100");|Integer i = Integer.valueOf("100");|
|long l = Long.parseLong("100");|Long l = Long.valueOf("100);|
|float f = Float.parseFloat("3.14");|Float f = Float.valueOf("3.14");|
|double d = Double.parseDouble("3.14");|Double d = Double.valueOf("3.14");|

- 문자열이 10진수가 아닌 다른 진법일 때 변환하는 메소드도 존재
```java
/* 형식 */
static int parseInt(String s, int radix);
static Integer valueOf(String s, int radix);

// 100(2) -> 4
Integer.parseInt("100", 2); 
// 100(8) -> 64
Integer.parseInt("100", 8); 
// 100(16) -> 256
Integer.parseInt("100", 16); 
// FF(16) -> 255
Integer.parseInt("FF", 16); 
// NumberFormatException 발생
Integer.parseInt("FF"); 
```


### 오토박싱 & 언박싱
```java
ArrayList<Integer> list = new ArrayList<Integer>();

// 오토박싱: 10 -> new Integer(10)
list.add(10); 
// 언박싱: new Integer(10) -> 10
int value = list.get(0); 
```
- JDK 1.5 이전에는 기본형과 참조형 간의 연산이 불가능했기에 래퍼 클래스로 기본형을 객체로 생성하여 연산
    - 오토박싱 & 언박싱으로 인해 기본형과 참조형 간의 연산이 가능
- 오토박싱(autoboxing): 기본 자료형의 값을 래퍼 클래스 객체로 자동 변환하는 것 
- 언박싱(unboxing): 래퍼 클래스 객체를 기본 자료형의 값으로 자동 변환하는 것

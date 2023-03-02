# 1. 제네릭스(Generics)
## 1-1 개요
- 컴파일 시 타입을 체크하는 기능(compile-tile type check)
- 타입을 파라미터화하여 컴파일 시 구체적인 타입이 결정되도록 하는 것
- 데이터의 타입을 일반화하는 것
- 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시 미리 지정하는 방법
- 장점
    - 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성 상승
        - 의도하지 않은 타입의 객체가 저장되는 것을 방지하고 저장된 객체를 꺼낼 때 원래와 다른 타입으로 형변환하는 경우 발생되는 오류 감소
    - 반환값에 대한 타입 변환 및 타입 검사에 소모되는 노력 감소
- JDK 1.5 이전 여러 타입을 사용하는 대부분의 클래스나 메소드에서 인수나 반환값으로 Object 타입을 사용
    - 해당 경우 반환된 Object 객체를 다시 원하는 타입으로 타입 변환이 필요하며 변환 시 오류 가능성 존재
    - JDK 1.5부터 도입된 제네릭을 통해 컴파일 시 미리 타입이 정해지므로 타입 검사나 타입 변환과 같은 번거로운 작업을 생략 가능

## 1-2 제네릭 클래스의 선언
```java
// 제네릭 클래스 변경 전
public class ArrayList extends AbstractList{
    private transient Object[] elementData;
    public boolean add(Object o) { /* 내용 생략 */ }
    public Object add(int index) { /* 내용 생략 */ }
    ...
}

// 제네릭 클래스 변경 후
public class ArrayList<E> extends AbstractList<E>{
    private transient E[] elementData;
    public boolean add(E o) { /* 내용 생략 */ }
    public E add(int index) { /* 내용 생략 */ }
    ...
}

// 타입 매개변수 E 대신에 실제 타입 Tv를 대입
ArrayList<Tv> tvList = new ArrayList<Tv>();

tvList.add(new Tv());
Tv t = tvList.get(0); // 형 변환 생략 가능
```
- 제네릭 클래스를 작성 시 Object 타입 대신 타입 매개변수(E)를 선언하여 사용
    - 타입 매개변수 : 임의의 참조형 타입
    - 'E'가 아닌 다른 문자도 사용 가능 
    - 여러 개의 타입 매개변수는 쉼표로 구분하여 명시 가능
    - 타입 매개변수는 클래스뿐만 아니라 메소드의 매개변수나 반환값으로도 사용 가능
- 제네릭 클래스의 객체를 생성 시 타입 매개변수(E) 대신에 실제 타입(String) 지정이 필요
    - 타입 매개변수 대신 실제 타입을 지정 시 형 변환 생략 가능
    - 실제 타입을 지정 시 기본 타입을 바로 사용 불가하고 Integer와 같은 래퍼(wrapper) 클래스를 사용

## 1-3 제네릭 타입의 다형성
```java
/*
    1. 참조변수와 생성자의 대입된 타입은 일치
*/
ArrayList<Tv> tvList = new ArrayList<Tv>();           // 일치
ArrayList<Product> productList = new ArrayList<Tv>(); // 불일치이므로 에러 발생

/*
    2. 제네릭 클래스 간의 다형성은 성립(대입된 타입 일치 필요)
*/
List<Tv> list1 = new ArrayList<Tv>();
List<Tv> list2 = new LinkedList<Tv>();

/*
    3. 매개변수의 다형성 성립
*/
ArrayList<Product> list = new ArrayList<Product>();
list.add(new Product());
list.add(new Tv());     // OK
list.add(new Audio());  // OK

boolean add(E e){ ... } // E에는 Product가 대입되므로 Product 및 자손 객체가 가능

/*
    4. JDK 1.7부터 타입 추론이 가능한 경우 생성자의 타입을 생략 가능
*/
ArrayList<Product> list = new ArrayList<>(); // 참조변수의 타입으로 ArrayList가 Product 타입의 객체만 저장한다는 것을 알 수 있으므로 생략 가능
```

## 1-4 제네릭 타입의 형 변환
```java
/*
    1. 제네릭 타입과 원시 타입 간의 형 변환은 가능하지만 권장 X
*/
Box box = null;            // 원시 타입
Box<Object> objBox = null; // 제네릭 타입

box = (Box)objBox;         // 제네릭 타입 -> 원시 타입 (경고 발생)
objBox = (Box<Object>)box; // 원시 타입 -> 제네릭 타입 (경고 발생)

/*
    2. 대입된 타입이 다른 제네릭 타입 간의 형 변환은 불가
*/
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>)strBox; // 에러 발생 Box<String> -> Box<Object>
strBox = (Box<String>)objBox; // 에러 발생 Box<Object> -> Box<String>

/*
    3. 와일드 카드가 사용된 제네릭 타입으로 형 변환 가능
*/
// FruitBox<Apple> → FruitBox<? extends Fruit>
Box<Object> objBox = (Box<Object>) new Box<<String>();                  // 에러 발생
Box<? extends Object> wBox = (Box<? extends Object>) new Box<String>(); // OK
Box<? extends Object> wBox2 = new Box<String>();                        // 위 문장과 동일

// FruitBox<? extends Fruit> → FruitBox<Apple>
FruitBox<? extends Fruit> box = null;
FruitBox<Apple> appleBox = (FruitBox<Apple>) box; // OK(타입이 확인 불가하여 형 변환 경고 발생)
```

## 1-5 제네릭 타입의 제거
```java
/*
    1. 제네릭 타입의 경계(bound)를 제거
*/
// 변경 전
class Box<T extends Fruit> {
    void add(T t) {
        ...
    }
}

// 변경 후
class Box {
    void add(Fruit t) {
        ...
    }
}
/*
    2. 제네릭 타입을 제거한 후 타입이 일치하지 않으면 형 변환을 추가
*/
// 변경 전
T get(int i) {
    return list.get(i);
}

// 변경 후
Fruit get(int i) {
    return (Fruit)list.get(i);
}

/*
    3. 와일드 카드가 포함된 경우 적절한 타입으로의 형 변환이 추가
*/
// 변경 전
static Juice makeJuice(FruitBox<? extends Fruit> box) {
    String tmp = "";
    for (Fruit f : box.getList()){
        tmp += f + " ";
    }
    return new Juice(tmp);
}

// 변경 후
static Juice makeJuice(FruitBox box) {
    String tmp = "";
    Iterator it = box.getList().iterator();
    while(it.hasNext()) {
        tmp += (Fruit)it.next() + " ";
    }
    return new Juice(tmp);
}
```
- 컴파일러는 제네릭 타입을 제거 후 필요한 영역에 형 변환을 추가
- 자바 코드에서 선언되고 사용된 제네릭 타입은 컴파일 시 컴파일러에 의해 자동으로 검사되어 타입 변환
- 코드 내의 모든 제네릭 타입은 제거되어 컴파일된 class 파일에는 어떠한 제네릭 타입도 포함 X
    - 제네릭을 사용하지 않는 코드와의 호환성을 유지하기 위한 목적

## 1-6 와일드 카드
```
<?> 
- 타입 변수에 모든 타입을 사용 가능
- <? extends Object>와 동일

<? extends T>
- 와일드 카드의 상한 제한
- T 타입과 T 타입을 상속받는 자손 클래스 타입만을 사용 가능

<? super T>
- 와일드 카드의 하한 제한
- T 타입과 T 타입이 상속받은 조상 클래스 타입만을 사용 가능
```
```java
/*
    1. 메소드의 매개변수에 와일드 카드를 사용 가능
*/ 
static Juice makeJuice(FruitBox<? extends Fruit> box) {
	String tmp = "";
	for(Fruit f: box.getList()) tmp += f + " ";
	return new Juice(tmp);
}

/*
    2. 메소드의 매개변수로 FruitBox<Fruit> 외에 FruitBox<Apple>와 FruitBox<Grape>도 가능
*/
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new FruitBox<Apple>();
. . .
System.out.println(Juicer.makeJuice(fruitBox)); // OK. FruitBox<Fruit>
System.out.println(Juicer.makeJuice(appleBox)); // OK. FruitBox<Apple>
```
- 이름에 제한을 두지 않는 것을 표현할 때 사용되는 기호
- 와일드 카드를 사용 시 하나의 참조 변수로 대입된 타입이 다른 객체를 참조 가능
- 와일드카드는 기호 '?'로 표현하며 와일드 카드는 어떠한 타입도 되는 것이 가능
- 와일드 카드에는 "&"를 사용 불가
    - `<? extends T & E>` -> 사용 불가

<br>

# 2. 열거형(enums)
## 2-1 개요
```java
/*
    enum 문법이 지원되지 않을 때 여러 상수를 정의하여 사용하려면 public static final을 통해 전역변수로 상수를 설정하여 사용
    -> 상수명이 중복되는 문제 발생
*/
//여러 상수를 정의하기 위한 예전 방식
public static final int SPRING = 1;
public static final int SUMMER = 2;

public static final int DJANGO = 1;
public static final int SPRING = 2; //계절의 SPRING과 중복 발생

/*
    인터페이스를 사용하여 상수를 구분함으로써 상수명이 중복되는 문제 해결
    -> 타입 안정성 문제 발생
*/
interface Seasons {
	int SPRING = 1, SUMMER = 2;
}

interface Frameworks {
	int DJANGO = 1, SPRING = 2;
}
// 의미적으로 다르지만 비교했을 때 에러가 발생하지 않으므로 타입 안전성이 감소
if(Seasons.SPRING == Framworks.SPRING) 

/*
    객체를 생성하면 상수명 중복과 타입 안정성 문제를 모두 해결
    -> 코드가 길어지고 사용자 정의 타입이므로 switch문을 사용하면 (java : incompatible types: Seasons cannot be converted to int) 에러 발생
*/
class Seasons {
	public static final Seasons SPRING = new Seasons();
    public static final Seasons SUMMER = new Seasons()'
}

class Frameworks {
	public static final Seasons DJANGO = new Framworks();
    public static final Seasons SPRING = new Framworks()'
}
```
- 서로 연관된 상수들의 집합
- 몇 가지로 한정된 변하지 않는 데이터를 다룰 때 사용
- 장점
    - 여러 상수를 편리하게 선언하고 관리 가능
    - 상수명의 중복 회피 및 타입에 대한 안정성 보장
    - 간결성 및 가독성 증대

## 2-2 열거형의 정의와 사용
```java
enum Direction { EAST, WEST, SOUTH, NORTH }

class Unit{
    int x, y;	    // 유닛의 초기화
    Direction dir;	// 열거형을 인스턴스 변수로 선언
    
    void init() {
    	dir = Direction.EAST; // 유닛의 방향을 EAST로 초기화
                              // 열거형 변수의 값은 열거형 상수 값(EAST, WEST ..) 중 하나
    }
}

if( dir == Direction.EAST ){
	x++;
} else if (dir > Direction.WEST){ // 에러 발생. 열거형 상수에 비교 연산자 사용 불가 
	...
} else if (dir.compareTo(Direction.WEST) > 0){ // compareTo()는 가능
}
```
- 각각의 상수에 따로 값을 지정하지 않아도 자동적으로 0부터 시작하는 정수값이 할당
- 상수명은 대문자로 작성
- 열거형 상수의 비교에 ==와 compareTo()를 사용
    - compareTo() : 열거형 상수가 정의된 순서의 차이를 반환

## 2-3 Enum 메소드
|메소드|설명|
|---|------|
|static E values()|해당 열거체의 모든 상수를 저장한 배열을 생성하여 반환|
|static E valueOf(String name)|전달된 문자열과 일치하는 해당 열거체의 상수를 반환|
|protected void finalize()|해당 Enum 클래스가 final 메소드를 가지는 것 불가|
|String name()|해당 열거체 상수의 이름을 반환|
|int ordinal()|해당 열거체 상수가 열거체 정의에서 정의된 순서(0부터 시작)를 반환|

<br>

# 3. 애너테이션(annotation)
## 3-1 개요
- 소스코드에 붙여서 특별한 의미를 부여하는 기능
- 컴파일러에게 문법 에러를 체크하도록 정보 제공
- 프로그램을 빌드할 때 코드를 자동으로 생성할 수 있도록 정보 제공
- 런타임에 특정 기능을 실행하도록 정보 제공
- 다른 프로그램들에 영향 X

## 3-2 애너테이션 종류
1. 표준 애너테이션
|표기|설명|
|---|------|
|@Override|컴파일러에게 메소드 오버라이딩 여부를 알릴 때 사용|
|@Deprecated|앞으로 사용하지 않을 대상을 알릴 때 사용|
|@Functionallnterface|함수형 인터페이스라는 것을 알릴 때 사용|
|@SuppressWarning|컴파일러가 경고메세지를 표시 X|
- 자바에서 기본적으로 제공하는 애너테이션

2. 메타 애너테이션
|표기|설명|
|---|------|
|@Target|애너테이션을 정의할 때 적용 대상을 지정하는데 사용|
|@Documented|애너테이션 정보를 javadoc으로 작성된 문서에 포함|
|@Inherited|애너테이션이 하위 클래스에 상속|
|@Retention|애너테이션이 유지되는 기간을 정하는데 사용|
|@Repeatable|애너테이션을 반복하여 적용 가능|
- 애너테이션에 붙이는 애너테이션
- 애너테이션을 정의할 때 사용

3. 사용자 정의 애너테이션
- 사용자가 직접 정의하는 애너테이션

## 3-3 표준 애너테이션
```
@Override
- 메서드 앞에만 붙일 수 있는 애너테이션
- 선언한 메서드가 상위 클래스의 메서드를 오버라이딩하는 메서드라는 것을 컴파일러에게 알려주는 역할
- 자바 컴파일러에게 어떤 정보를 제공하기 위한 역할
    - 상위 클래스(또는 인터페이스)에서 @Override가 붙어있는 메서드명과 동일한 이름의 메서드를 찾을 수 없다면 컴파일 에러 발생

@Deprecated
- 기존 메서드를 하위 버전 호환성 문제로 삭제하기 곤란해 남겨두어야만 하지만 더 이상 사용하는 것을 권장하지 않을 때 사용

@SuppressWarnings
- 컴파일 경고 메시지가 나타나지 않도록 설정
    - 경우에 따라서 경고가 발생할 것이 충분히 예상됨에도 묵인해야 할 때 주로 사용

@SuppressWarnings() 
- 괄호 안에 억제하고자 하는 경고메세지를 지정
    - @SuppressWarnings("all") : 모든 경고를 억제
- 둘 이상의 경고를 한번에 묵인 가능
    - @SuppressWarnings({"deprecation","unused","null"})

@Functionallnterface
- 코드 작성과정에서 실수를 방지하기 위한 확인용 애너테이션
- 함수형 인터페이스를 선언할 때, 컴파일러가 함수형 인터페이스의 선언이 바르게 선언되었는지 확인
    - 바르게 선언되지 않은 경우 에러 발생
    - 함수형 인터페이스는 단 하나의 추상 메서드만을 가져야하는 제약이 존재
```

## 3-4 메타 애너테이션
```
@Target
- 애너테이션을 적용할 대상을 지정하는 데 사용

@Documented
- 애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 하는 애너테이션 설정
- 표준 애너테이션과 메타 애너테이션 중 @Override와 @SuppressWarnings를 제외하고는 모두 @Documented가 적용

@Inherited
- 하위 클래스가 애너테이션을 상속받도록 하는데 사용
- 상위클래스에 붙이면 하위 클래스도 상위 클래스에 붙은 애너테이션들이 동일하게 적용

@Retention
- 특정 애너테이션의 지속 시간을 결정하는 데 사용
- 애니테이션과 관련한 유지 정책(retention policy) : 애너테이션이 유지되는 기간을 지정하는 속성
    - SOURCE : 소스 파일에 존재, 클래스 파일에는 존재 X
    - CLASS : 클래스 파일에 존재, 실행 시 사용 불가, 기본값
    - RUNTIME : 클래스 파일에 존재, 실행 시 사용 가능

@Repeatable
- 애너테이션을 여러 번 붙일 수 있도록 허용
```

## 3-5 사용자 정의 애너테이션
```java
// 인터페이스 앞에 @ 기호를 붙이면 애너테이션을 정의 가능
@interface 애너테이션명{
    // 애너테이션 요소 선언  
	타입 요소명(); 
 }
```

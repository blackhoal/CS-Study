# 1. 객체지향언어
## 1-1. 객체지향이란?
- 프로그램을 단순히 데이터와 처리 방법으로 나누는 것이 아니라, 프로그램을 수많은 '객체(object)'라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식의 프로그램 설계방법론
- 실제 세계는 사물(객체)로 이루어져 있으며, 발생하는 모든 사건은 사물간의 상호작용

## 1-2. 장점
- 코드의 재사용성이 우수
    - 새로운 코드를 작성 시 기존의 코드를 이용하여 쉽게 작성 가능
- 코드의 관리가 용이
    - 코드 간의 관계를 통해 적은 노력으로도 쉽게 코드 변경 가능
- 신뢰성 우수
    - 제어자와 메소드를 통해 데이터를 보호하고 올바른 값을 유지하며 코드의 중복을 제거하여 코드의 불일치로 인한 오동작 방지

## 1-3. 특징
- 캡슐화
    - 클래스의 내부 변수와 메소드를 하나로 묶는 것
    - 객체의 세부 내용이 외부에 드러나지 않아 외부에서 데이터를 직접 접근하는 것을 방지
- 상속
    - 자식 클래스가 부모 클래스의 특징과 기능을 물려받는 것
    - 클래스를 상속 받아 수정하여 사용하므로 중복 코드를 줄이는 것이 가능
    - 부모 클래스를 수정 시 자식 클래스도 수정
- 추상화
    - 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능을 간추려 내는 것
    - 불필요한 부분을 숨기는 것이 가능
    - 인터페이스와 구현을 분리
- 다형성
    - 프로그램 언어의 각 요소가 다양한 자료형에 속하는 것이 허가되는 성질
    - 오버라이딩 : 부모 클래스의 메소드를 자식 클래스에서 재정의하는 것

# 2. 클래스와 객체
- 클래스
    - 객체를 생성하기 위한 설계도
- 객체 
    - 실제로 존재하는 것
    - 인스턴스화 : 특정 클래스로부터 실제로 객체를 생성하는 과정
- 객체의 구성
    - 속성(property) : 멤버변수(member variable), 특성(attribute), 필드(field), 상태(state)
    - 기능(function) : 메소드(method), 함수(function), 행위(behavior)

# 3. 변수와 메소드
```java
class Variables{
    // 클래스 영역
    int iv; // 인스턴스 변수
    static int cv; // 클래스 변수

    // 메소드 영역
    void method() {
        int lv = 0; // 지역변수
    }
}
```
|변수의 종류|선언위치|생성시기|
|------|---|---|
|클래스 변수(class variable)|클래스 영역|클래스가 메모리에 올라갈 때|
|인스턴스 변수(instance variable)|클래스 영역|인스턴스가 생성되었을 때|
|지역변수(local variable)|클래스 영역 이외의 영역|변수 선언문이 수행되었을 때|

- 인스턴스 변수(instance variable)
    - 클래스 영역에 선언
    - 클래스의 인스턴스를 생성할 때 만들어지는 변수 
    - 인스턴스 변수를 사용하기 위해 인스턴스 생성이 전제조건으로 수행
    - 인스턴스마다 고유한 상태를 유지해야하는 속성의 경우, 인스턴스 변수로 선언

- 클래스 변수(class variable)
    - 인스턴스 변수 앞에 static을 함께 선언. 
    - 독립적인 저장공간을 갖는 인스턴스 변수와는 달리 모든 인스턴스가 공통된 저장공간(변수)을 공유
    - 한 클래스의 모든 인스턴스가 공통적인 값을 유지해야 할 때 클래스 변수로 선언
    - 인스턴스를 생성하지 않아도 사용하는 것이 가능하며 '클래스이름.클래스변수'와 같은 방식으로 사용한다.

- 지역 변수(local variable)
    - 메소드 내에 선언되어 메소드 내에서만 사용 가능
    - 메소드가 종료 시 소멸되어 사용 불가
    - for문 or while문과 같은 반복문에 선언된 지역변수는 지역변수가 선언된 블럭 내에서만 사용 가능하며, 블럭을 벗어나면 사용 불가

- 클래스 변수와 인스턴스 변수
    - 클래스 변수 : 모든 인스턴스가 항상 같은 값을 공유
    - 인스턴스 변수 : 각기 다른 값을 유지 가능 

- 메소드
    - 특정 작업을 수행하는 일련의 문장을 하나로 묶은 것

- 메소드를 사용하는 이유
    - 높은 재사용성
    - 중복된 코드의 제거
    - 프로그램의 구조화

- 메소드의 종류
    - 클래스 메소드 : 인스턴스의 생성 없이 호출 가능
    - 인스턴스 메소드 : 인스턴스를 생성해야 호출 가능

- JVM의 메모리 구조
```
1. 메소드 영역
- 클래스가 사용되면 해당 클래스의 클래스 파일을 읽고 분석하여 클래스 데이터를 저장
- 클래스 변수도 이 영역에 함께 생성

2. 힙
- 인스턴스 변수들이 생성되는 공간
- 프로그램 실행 중 생성되는 인스턴스는 모두 힙에 저장

3. 호출스택
- 메소드의 작업에 필요한 메모리 공간을 제공
- 메소드를 위한 메모리가 할당되며 지역변수들과 연산의 중간 결과 등을 저장하는데 사용
- 메소드가 작업을 마치면 해당 메모리 공간은 반환되어 제거
```

# 4. 오버로딩
```java
// 오버로딩
void display(int num1){
    System.out.println(num1)
}              

void display(int num1, int num2){
    System.out.println(num1+num2)
}

void display(int num1, double num2){
    System.out.println(num1+num2)
}

display(10);       // display(int num1) 메소드 호출 -> 10
display(10, 20);   // display(int num1, int num2) 메소드 호출 -> 200
display(10, 3.14); // display(int num1, double num2) 메소드 호출 -> 13.14

// 오버라이딩
class Parent {
    void display() { 
        System.out.println("부모 클래스의 display() 메소드입니다.");
    }
}

class Child extends Parent {
    void display() { 
        System.out.println("자식 클래스의 display() 메소드입니다.");
    }
}

Parent pa = new Parent();
pa.display(); // 부모 클래스의 display() 메소드입니다.

Child ch = new Child();
ch.display(); // 자식 클래스의 display() 메소드입니다.
```
- 오버로딩이란?
    - 한 클래스에서 메소드 이름은 동일하지만 파라미터 개수나 자료형을 다르게 함으로써 서로 다르게 동작하도록 정의하는 것
    - 성립 조건을 만족하지 못하게 정의할 경우 중복 정의로 컴파일 에러 발생
    - 중복을 줄이며, 메소드의 이름을 획일화하는 것이 가능

- 오버로딩의 조건
    - 메소드 이름이 동일
    - 매개변수의 갯수 또는 타입이 상이
    - 매개변수는 같고 리턴타입이 다를 경우는 성립 X

# 5. 생성자
- 개요
    - 인스턴스가 생성될 때 호출되는 인스턴스 초기화 메소드
    - 클래스에 정의된 생성자가 하나도 없을 시 기본 생성자가 컴파일러에 의해 추가

- 생성자의 조건
    - 생성자의 이름은 클래스의 이름과 동일
    - 생성자는 리턴값이 존재 X

- 생성자에서 다른 생성자 호출(this, this())
```java
// this
class Car {
    private String modelName;
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;

    Car(String modelName, int modelYear, String color, int maxSpeed) {
        this.modelName = modelName;
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }
    ...
}

// this()
class Car {
    private String modelName;
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;

    Car(String modelName, int modelYear, String color, int maxSpeed) {
        this.modelName = modelName;
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }

    Car() {
        this("소나타", 2012, "검정색", 160); // 다른 생성자를 호출함.
    }
 
    public String getModel() {
        return this.modelYear + "년식 " + this.modelName + " " + this.color;
    }
}

public class Method05 {
    public static void main(String[] args) {
        Car tcpCar = new Car(); System.out.println(tcpCar.getModel());
    }
}
```
```
1. this
- 클래스의 인스턴스 자신을 참조하는 데 사용하는 참조변수
- 메소드 및 생성자에 입력된 매개변수와 지역변수를 구분

2. this()
- 같은 클래스의 다른 생성자를 호출할 때 사용하는 메소드
- 생성자 내부에서만 사용 가능
- this() 메소드에 인수를 전달하면 생성자 중 메소드 시그니처가 일치하는 다른 생성자를 찾아 호출
```


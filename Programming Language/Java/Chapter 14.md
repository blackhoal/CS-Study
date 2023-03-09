# 1. 람다식
## 1-1 개요
```java
// 일반적인 코드
int[] arr = new int[5];
Arrays.setAll(arr, (i) ->(int)(Math.random()*5)+1);

// 람다식
int method(){
    return (int) (Math.random()*5)+1;
}
```
- 함수(메소드)를 간단한 '식(Expression)'으로 표현하는 방법
    - 함수를 간략하면서도 명확한 식으로 표현 가능
- 메소드를 람다식으로 표현하면 메소드의 이름과 반환값이 없어지므로 람다식을 익명함수라고도 지칭
- 메소드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통 { } 사이에 ->를 추가
- 반환 값이 있는 경우 return문 대신 식의 연산결과가 자동적으로 반환값이 되도록 하는 것이 가능
-  람다식에 선언된 매개변수의 타입은 추론이 가능한 경우 생략 가능 
    - 반환타입이 없는 이유도 추론이 가능하므로 
    - 매개변수 중 어느 하나의 타입만 생략하는 것은 허용 X
- 선언된 매개변수가 하나뿐인 경우에는 괄호를 생략 가능 
    - 매개변수의 타입이 존재하면 괄호를 생략 불가

## 1-2 람다식 작성
```java
/*
    1. 메소드의 이름과 반환타입을 제거하고 '->' 를 블록{} 앞에 추가
*/
// Before
int max(int a, int b) { return a > b ? a : b;}
// After
(int a, int b) -> { return a > b ? a : b;}

/*
    2. 반환값이 있는 경우 식이나 값만 작성하며 return문은 생략 가능 (끝에 ';' 제거)
*/
// Before
(int a, int b) -> { return a > b ? a : b;}
// After
(int a, int b) -> a > b ? a : b

/*
    3. 매개변수의 타입이 추론 가능하면 생략 가능(대부분의 경우 생략 가능)
*/
// Before
(int a, int b) -> a > b ? a : b
// After
(a, b) -> a > b ? a : b
```
## 1-3 함수형 인터페이스(Functional Interface)
- 람다식은 익명 함수가 아닌 익명 객체
    - 익명 클래스의 객체와 동등
- 함수형 인터페이스 : 람다식을 다루기 위한 인터페이스
- 오직 하나의 추상 메소드만 정의
    - 람다식과 인터페이스의 메소드가 1:1로 연결
    - static 메소드와 default 메소드에는 제약 X
- 메소드의 매개변수가 함수형 인터페이스라면 메소드를 호출할 때 람다식을 참조하는 참조변수를 매개변수로 지정 필요 
    - 참조변수 없이 직접 람다식을 매개변수로 지정하는 것도 가능
- 메소드의 반환타입이 함수형 인터페이스타입이라면 함수형 인터페이스의 추상 메소드와 동등한 람다식을 가리키는 참조변수를 반환하거나 람다식을 직접 반환 가능
- 익명 객체이므로 타입이 존재 X 
    - 대입 연산자의 양변의 타입을 일치시키기 위해 형변환이 필요
- 인터페이스를 구현한 클래스 객체와 완전히 동일하므로 동일한 인터페이스로의 형변환을 허용
    - 이러한 형변환은 생략 가능
- 객체이지만 Object 타입으로 형변환이 불가 
    - 오직 함수형 인터페이스로만 형변환이 가능
- 람다식 내에서 참조하는 지역변수는 final이 붙지 않았어도 상수로 간주
    - 클래스 인스턴스 변수는 상수로 간주되지 않아 값을 변경 가능
    - 외부 지역변수와 같은 이름의 람다식 매개변수는 허용 X

## 1-4 java.util.function 패키지
|함수형 인터페이스|메소드|설명|
|:------:|:------:|:------:|
|java.lang.Runnable|void run( )|매개변수 x, 반환값 x|
|Supplier<T>|T get( )|매개변수 x, 반환값만 존재|
|Consumer<T>|void accept(T t)|매개변수만 있고 반환값 X|
|Function<T,R>|R apply(T t)|하나의 매개변수를 받아 결과를 반환|
|Predicate<T>|boolean test(T t)|조건식을 표현하는데 사용 / 매개변수는 하나, 반환 타입은 boolean|
|BiConsumer<T,U>|void accept(T t, U u)|두 개의 매개변수만 있고 반환값 X|
|BiPredicate<T,U>|boolean test(T t, U u)|조건식을 표현하는데 사용 / 매개변수는 둘 / 반환 타입은 boolean|
|BiFunction<T,U,R>|R apply(T t, U u)|두 개의 매개변수를 받아서 하나의 결과를 반환|

## 1-5 메소드 참조
|종류|람다식|메소드 참조|
|:------:|:------:|:------:|
|static 메소드 참조|(x) -> ClassName.method(x)|ClassName::method|
|인스턴스 메소드 참조|(obj, x) -> obj.method(x)|ClassName::method|
|특정 객체 인스턴스 메소드 참조|(x) -> obj.method(x)|obj::method|
```java
// 람다식
Supplier<MyClass> s = () -> new MyClass();
// 메소드 참조
Supplier<MyClass> s = MyClass::new;

// 람다식
Function<Integer, MyClass> f = (i) -> new MyClass(i);
// 메소드 참조
Function<Integer, MyClass> f2 = MyClass::new;

// 람다식
BiFunction<Integer, String, MyClass> bf = (i,s) -> new MyClass(i,s);
// 메소드 참조
BiFunction<Integer, String, MyClass> bf2 = MyClass::new;

// 람다식
Function<Integer, int[]> f = x -> new int[x];
// 메소드 참조
Function<Integer, int[]> f2 = int[]::new;
```
- 람다식이 하나의 메소드만 호출하는 경우에는 메소드 참조로 람다식을 간략히 표현 가능
- 하나의 메소드만 호출하는 람다식은 `클래스 이름::메소드 이름` 또는 `참조변수::메소드 이름`으로 변경 가능
- 생성자를 호출하는 람다식도 메소드 참조로 변환 가능 
    - 매개변수가 있는 생성자라면 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용

<br>

# 2. 스트림(stream)
|중간 연산|설명|
|:------:|:------:|
|Stream<T> distinct( )|중복을 제거|
|Stream<T> filter(Predicate<T> predicate)|조건에 적합하지 않은 요소를 제외|
|Stream<T> limit(long maxsize)|스트림의 일부를 제거|
|Stream<T> skip(long n)|스트림의 일부를 패스|
|Stream<T> peek(Consumer<T> action)|스트림의 요소에 작업을 수행|
|Stream<T> sorted(Comparator<T> comparator)|스트림의 요소를 정렬|
|Stream<R> map(Function<T,R> mapper) / IntStream mapToInt(ToIntFunction<T> mapper)|스트림의 요소를 변환|

|최종 연산|설명|
|:------:|:------:|
|void forEach(Consumer<? super T> action)|각 요소에 지정된 작업 수행|
|long count( )|스트림의 요소의 개수 반환|
|Optional<T> max(Comparator<? super T> comparator) / Optional<T> min(Comparator<? super T> comparator)|스트림의 최대값/최소값을 반환|
|Optional<T> findAny( ) / Optional<T> findFirst( )|스트림의 요소 하나를 반환|
|boolean allMatch(Predicate<T> p) / boolean anyMatch(Predicate<T> p) / boolean noneMatch(Predicate<> p)|주어진 조건에 대한 확인을 수행|
|Object[] toArray( ) / A[] toArray(IntFunction<A[ ]> generator)|스트림의 모든 요소를 배열로 변환|
|Optional<T> reduce(BinaryOperator<T> accumulator)|스트림의 요소를 하나씩 줄여가면서 계산|
R collect(Collector<T,A,R> collector)|스트림의 요소를 수집 / 주로 요소를 그룹화하거나 분할한 결과를 컬렉션에 담아 반환하는데 사용|

- 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 라이브러리
- 데이터 소스를 추상화하고 데이터를 다룰 때 자주 사용되는 메소드를 정의함으로서 코드의 재사용성을 증대
- 배열이나 컬렉션 뿐만 아니라 파일에 저장된 데이터도 모두 같은 방식으로 다루는 것이 가능
- 데이터 소스로부터 데이터를 읽는 작업만 수행 
    - 스트림으로 작업을 수행한 결과를 컬렉션이나 배열에 담아서 반환 가능하지만 데이터 소스는 변경 불가
- 한번 사용하면 닫혀서 다시 사용하는 것이 불가하며 필요한 경우 다시 생성하여 사용
- 스트림이 제공하는 연산은 연산 결과를 스트림으로 반환하는 `중간 연산`과 스트림의 요소를 소모하면서 연산을 수행하는 `최종 연산`으로 분류
    - 중간 연산 : 연산 결과가 스트림 O / 연속해서 수행 가능
    - 최종 연산 : 연산 결과가 스트림 X / 스트림의 요소를 소모하므로 한 번만 수행 가능
- 모든 중간 연산의 결과는 스트림이지만 연산 전의 스트림과 같은 것은 X 
    - 스트림의 작업을 수행한 결과로 스트림을 반환
- 스트림 연산에서 최종 연산이 수행되기 전까지는 중간 연산은 수행 X 
    - 중간 연산이 호출하는 것은 어떤 작업이 수행되어야 하는지만 지정
    - 최종 연산이 수행되어야 스트림의 요소가 중간 연산을 거쳐 최종 연산에서 소모
- 기본적으로 Stream <T>의 형태 
    - 오토박싱과 언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림(IntStream, LongStream, DoubleStream)이 제공
    - 기본형 타입의 값으로 작업하는데 유용한 메소드가 포함
    - parallel( ) : 내부적으로 프레임워크를 이용하여 스트림이 자동적으로 연산을 병렬로 수행하도록 하는 역할
    - sequential( ) : parallel()과는 반대로 병렬로 처리하지 않도록 하는 역할
- 컬렉션은 stream( ), 배열은 Stream.of( ) 또는 Arrays.stream( ) 메소드를 사용하여 스트림을 생성 가능 
    - 기본형 스트림 클래스에도 of( ) 메소드가 정의
    - Arrays.stream( ) 메소드는 기본형 스트림 클래스에 대해 오버로딩 O
- empty( ) : 비어있는 스트림을 생성 / 스트림에 연산을 수행한 결과가 하나도 없을 때 null보다 빈 스트림을 반환하는 것을 권장
- concat( ) : 두 스트림을 하나로 연결 / 두 스트림의 요소는 같은 타입으로 일치 필요
- Optional 타입의 객체에는 모든 타입의 참조변수를 담는 것이 가능 
    - 최종 연산의 결과를 Optional 객체에 담아 반환함으로서 NullPointerException이 발생하지 않으며 보다 간결하고 안전한 코드를 작성하는 것이 가능
- collect( ) : 스트림의 요소를 수집하는 최종 연산 매개변수의 타입이 Collector이며 Collector 객체에 구현된 방법대로 스트림의 요소를 수집
    - 매개변수로 toList( ), toSet( ), toMap( ), toCollection( ), toArray( )메소드를 사용하여 요소를 컬렉션에 담아 반환 가능
- partitioningBy( ) / groupingBy( ) : 스트림의 요소를 특정 기준으로 그룹화 / 분할 
    - collect 메소드의 매개변수로 사용하여 원하는 컬렉션을 얻는 것이 가능

# Reference
[참조 1](https://haeng-on.tistory.com/41)  
[참조 2](https://recoderr.tistory.com/46?category=887628)  

# 1. 날짜와 시간
## 1-1. Calendar와 Date
```java
/* Calendar 객체 생성 */
// 에러 -> 추상클래스는 인스턴스 생성 불가
Calendar cal = new Calendar(); 
// OK
Calendar cal = Calendar.getInstance();

/* Calendar를 Date로 변환 */
Calendar cal = Calendar.getInstance();
...
Date d = new Date(cal.getTimeMillis());

/* Date를 Calendar로 변환 */
Date d = new Date();
...
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```
- Calendar는 추상 클래스이기 때문에 직접 객체를 생성할 수 없고, 메서드를 통해 완전히 구현된 클래스의 인스턴스를 얻어서 사용
    - 최소한의 변경으로 프로그램이 동작할 수 있도록 하기 위해
- Calendar를 상속받아 완전히 구현한 클래스로는 GregorianCalendar와 BuddhistCalendar가 존재 
    - getInstance()는 시스템의 국가와 지역설정을 확인하여 태국인 경우 BuddhistCalendar의 인스턴스를 반환하고, 그 외는 GregorianCalendar의 인스턴스를 반환

## 1-2. Calendar에서의 데이터 접근
- get, set을 사용하여 접근
- add(int field, int amount): 지정한 필드의 값을 원하는 만큼 증가/감소
- roll(int field, int amount): 지정한 필드의 값을 증가/감소하나 다른 필드는 고정
- 1월은 0부터 시작(1월 = 0, 12월 = 11)
- 일요일 ~ 토요일 : 0 ~ 6

# 2. 형식화 클래스
## 2-1. Decimalformat
```java
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);
```
- 숫자를 형식화할 때 사용
- 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현 가능
    - 반대로 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능

## 2-2. SimpleDateFormat
```java
/* SimpleDateFormat 예제 */
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyy-MM-dd");
String result = df.format(today);
```
|||||||
|---|------|---|------|---|------|
|y|연도|F|월의 몇번째 요일|h|시간(1~12)|
|M|월|E|요일|m|분(0~59)|
|w|년의 몇번째 주|a|오전/오후|s|초(0~59)|
|W|월의 몇번째 주|H|시간(0~23)|S|천분의 1초|
|D|년의 몇번째 일|k|시간(1~24)|z|Time zone(GMT)|
|d|월의 몇번째 일|K|시간(0~11)|||
- 날짜를 Text로 변환하는 format 변경 및 Text를 날짜로 변환하는 분석 기능을 제공
- 사용자가 원하는 형태로 생성하여(사용자 정의 패턴) String의 형태로 변환 가능

## 2-3. ChoiceFormat
```java
/* 예제 1 */
double[] limits = {60, 70, 80, 90}; 
String[] grades = {"D", "C", "B", "A"};

int[] scores = {100, 95, 88, 70, 52, 60, 70};

ChoiceFormat form = new ChoiceFormat(limits, grades);

for(int i=0; i<scores.length; i++) {
    System.out.println(scores[i] + " : "+ form.format(scores[i]));
}

/* 예제 2 */
// # : 경계값을 범위에 포함
// < : 경계값을 범위에 포함 X
String pattern = "60#D|70#C|80<B|90#A";

int[] scores = {91, 90, 80, 88, 70, 52, 60};

ChoiceFormat form = new ChoiceFormat(pattern);

for(int i=0; i < scores.length; i++) {
    System.out.println(scores[i] + " : " + form.format(scores[i]));
}
```
- 특정 범위에 속하는 값을 문자열로 변환
- 연속적이나 불연속적인 범위의 값을 처리하는 과정에서 if문이나 switch문으로 처리하기힘들 때 사용

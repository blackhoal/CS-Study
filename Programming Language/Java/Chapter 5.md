# 배열이란
```java
int[] score = new int[5];
```
- 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것

# 배열의 선언 및 생성
## 배열의 선언
```java
// 타입[] 변수명
int[ ] score;
String[ ] name;
```
- 배열을 다루기 위한 참조변수의 선언

## 배열의 생성
```java
int[ ] score;
score = new int[5];

int[ ] score = new int[5];
```
- 실제 저장공간을 생성

# 배열의 인덱스
- 각 요소(저장공간)에 자동으로 붙는 일련번호
- 인덱스의 범위 : 0 ~ 배열의길이-1

# 배열의 길이
```java
int[] arr = new int[5];
// 배열명.length : 배열의 길이
int tmp = arr.length;

// 배열의 모든 요소 출력
int[] score = new int[6];
for(int i = 0; i < score.length; i++)
	System.out.println(score[i]);
```
- 배열은 한 번 생성 시 실행하는 동안 해당 길이를 변경 불가

# 배열의 초기화
```java
int[] score = { 50, 60, 70, 80, 90 };
```
- 배열의 각 요소에 처음으로 값을 저장하는 것
- 배열은 처음 생성 시 값이 0으로 초기화

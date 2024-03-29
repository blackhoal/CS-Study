# 제어문
- 조건문 : 조건을 만족하는 경우에만 괄호 내의 작업을 수행(0 ~ 1번)
- 반복문 : 조건을 만족하는 동안 괄호 내의 작업을 수행(0 ~ n번)

# if문
```
if (조건식) {
	수행할 작업의 내용
}
```
- 조건식이 true일 때 괄호 내의 작업을 수행

# if-else문
```
if (조건식 1) {
    조건식 1의 결과가 참일 때 수행할 작업
} else if (조건식 2) {  
    조건식 2의 결과가 참일 때 수행할 작업
} else if (조건식 3) {
    조건식 3의 결과가 참일 때 수행할 작업
} else {  
    위의 어느 조건식도 만족하지 않을 때 수행할 작업
}
```

# switch문
```
switch (조건식) {
	case a :
	조건식의 결과가 a와 같을 경우 수행할 작업
  	break;

	case b :
	조건식의 결과가 b와 같을 경우 수행할 작업
	break;     

	default :
	조건식의 결과와 일치하는 값이 없을 때 수행할 작업
}
```
- 처리해야 하는 경우의 수가 많을 때 유용한 조건문
- 모든 switch문은 if문으로 변환이 가능하지만 모든 if문은 switch문으로 변환이 안되는 경우가 존재
- switch문의 조건식 결과는 정수 또는 문자열
- case문의 값은 정수 상수, 문자열만 가능하며 중복 불가

# for문
```
for(초기화;조건식;증감식) {
    수행할 작업
}
```
- 조건을 만족하는 동안 블럭 내의 작업을 반복
- 반복 횟수를 알고 있을 때 적합한 조건문

# while문
```
while(조건식) {
    조건식의 결과가 참인 동안 수행할 작업
}

do {
    최소 1회 이상 무조건 실행할 작업
} while (조건식) {
    수행할 작업
}
```
- 조건을 만족하는 동안 블럭 내의 작업을 반복
- 반복 횟수를 모를 때 적합한 조건문

# break문
```
while (true) {
    if(조건식) {
        break;
    }
}
```
- 자신이 포함된 하나의 반복문을 탈출하는 구문

# continue문
```
for(int i=0;i<=10;i++) {
    if(조건식) {
        continue;
    }
}
```
- 자신이 포함된 반복문의 끝으로 이동하여 다음 반복으로 넘어가는 구문
- 전체 반복 중 특정 조건을 만족할 때 반복을 건너기 위해 사용
- break문과 달리 반복문을 벗어나지 않음

# 이름이 붙은 반복문
```
{ Loop1 : for(int i=2;i<=9;i++) {
	for(int j=1;j<=9;j++) {
		if(j==5)
			break Loop1;
	}
}
}
```
- 반복문에 이름을 붙여 하나의 반복문을 벗어나는 것이 가능

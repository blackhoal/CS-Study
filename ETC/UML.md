# 1. UML(Unified Modeling Language)
## 1-1. 개요
- 시스템 분석, 설계, 구현 등 시스템 개발 과정에서 시스템 개발자와 고객 또는 개발자 상호 간의 원활한 의사소통이 성사되도록 표준화한 대표적인 `객체지향 모델링 언어`

## 1-2. 구성
<p align="center"><img src="https://user-images.githubusercontent.com/48504392/129287233-1766f678-4ced-4e58-8589-42c4552e7e00.png" width = "80%" height = "80%"></p>

## 1-3. 사물
- 모델을 구성하는 가장 중요한 기본 요소
- 다이어그램 내에서 관계가 형성될 수 있는 대상의 집합

|사물|내용|
|:--:|:--:|
|구조 사물<br>(Structural Things))|시스템의 개념적, 물리적 요소를 표현<br>예시 - 클래스 / 유스케이스 / 컴포넌트 / 노드|
|행동 사물<br>(Behavioral Things))|시간과 공간에 따른 요소의 행위를 표현<br>예시 - 상호작용 / 상태 머신 등|
|그룹 사물<br>(Grouping Things))|요소들을 그룹으로 묶어서 표현<br>예시 - 패키지|
|주해 사물<br>(Annotation Things))|부가적인 설명이나 제약조건 등을 표현<br>예시 - 노트|

## 1-4. 관계
|관계|표시|설명|
|:--:|:--:|:--:|
|연관<br>(Association)|실선 or 화살표|`클래스들이 서로 연결`되었음을 표현 <br>한 클래스가 다른 클래스 기능을 사용 시 표현|
|일반화<br>(Generalization)|속이 빈 화살표|객체지향에서의 `상속 관계` / IS-A 관계|
|집약<br>(Aggregation)|속이 빈 마름모|클래스들 사이의 전체 또는 부분 등의 관계<br>전체 객체와 부분 객체는 `독립적`<br>전체 객체가 소멸 시 부분 객체는 소멸 X|
|합성<br>(Composition)|속이 찬 마름모|클래스들 사이의 전체 또는 부분 등의 관계<br>전체 객체와 부분 객체는 `의존적`<br>전체 객체가 소멸 시 부분 객체도 소멸|
|의존<br>(Dependency)|점선 화살표|`연관 관계`와 유사<br> 차이점은 두 클래스의 관계가 매우 짧은 시간만 유지
|실체화<br>(Realization)|빈 삼각형, 점선|인터페이스와 클래스 사이의 관계|


① 연관 관계(Associtaion)
```java
public class Person { 
    private Car owns; 

    // getter, setter 
    public void setCar(Car car) { 
        this.owns = car; 
    } 
    public Car getCar() { 
        return this.owns; 
    }
}
```
② 의존 관계  
```java
public class Car {
    public void fillGas(GasPump p){
        p.getGas(amount); }
    }
}
```
③  집합연관 관계  
```java
public class Computer { 
    private MainBoard mb; 
    private CPU c; 

    public Computer(MainBoard mb, CPU c) { 
        this.mb = mb; 
        this.c = c; 
    }
}
```
④ 합성 관계(Composition)  
- 집
```java
```

## 1-5. 다이어그램

#
# 2. 유스케이스 다이어그램
## 2-1. 개요
## 

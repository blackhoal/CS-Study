# 1. Domain
## 1-1. IP(Internet Protocol)
- 숫자로 이루어진 인터넷 상의 컴퓨터 주소
- 각 장치가 가지는 주민등록번호 개념
- OSI 7 Layers의 L3(Network Layer)와 TCP/IP 4 Layers의 L3(Internet Layer)에 위치하는 프로토콜
- 편지 봉투에 보내는 주소와 받는 주소를 작성하고 우표를 붙여 우체통에 넣는 작업 / 우편함에 있는 편지를 꺼내 사용자에게 온 편지가 맞는지 확인하는 작업

## 1-2. Domain
- 외우거나 식별하기 어려운 IP 주소(예:240.10.20.1)를 example.com 처럼 기억하기 쉽게 만들어주는 네트워크 호스트 이름
- 일반적으로 Root DNS 서버(IANA에서 관리하는 최상위 DNS서버)에 등록된 `최상위 호스트 네임` 및 도메인 레지스트리에서 각 최상위 호스트 네임과 함께 관리하는 `하위 호스트 네임`을 지칭
- 인터넷에서 사용되는 주소이며 IP 주소를 알기 쉽게 영문으로 표현한 것

## 1-3. Domain VS Hosting
- Domain은 `사이트의 이름을 지정`하는 것이고 Hosting은 `사이트가 데이터를 저장할 공간을 대여`하는것

#
# 2. DNS(Domain Name System)
## 2-1. 개요
- 네트워크에서 도메인이나 호스트 이름을 숫자로 된 IP 주소로 해석해주는 TCP/IP 네트워크 서비스
- 인터넷이나 사설 네트워크에 연결된 컴퓨터, 서비스 또는 기타 리소스에 대한 계층적 분산 명명 시스템
- `범국제적 단위`로 웹사이트의 IP 주소와 도메인 주소를 이어주는 환경/시스템
- 호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행할 수 있도록 하기 위해 개발된 시스템
- 각 조직을 구분해주는 도메인 이름을 관리하는 DNS 서버들이 모여서 생성한 가상의 이름 공간

## 2-2. 특징
- 컴퓨터의 도메인 이름을 IP 주소로 변환하고 라우팅 정보를 제공하는 분산형 데이터베이스 시스템
- 인터넷의 전화번호부와 같은 역할을 수행
- `리소스 레코드(Resource record)`를 보유
    - A, AAAA, CNAME, NS, MX, SPF, PTR 등으로 구성
- `Forward Zone`(도메인 이름 → IP)과 `Reverse Zone`(IP → 도메인 이름)를 보유 
    - 주로 Forward Zone에는 `도메인을 구성하는 호스트에 대한 정보` / Reverse Zone에는 `DNS 서버 자기 자신에 대한 정보`를 기록
- DNS 서버에 질의 시의 응답은 `Authoritative answer`와 `Non-authoritative answer`로 분류
    - Authoritative answer : DNS 서버가 질의 받은 도메인 또는 IP 주소의 레코드를 Forward Zone, Reverse Zone 모두 가지고 있을 경우에 하는 응답
    - Non-authoritative answer : DNS 서버가 질의 받은 도메인 또는 IP 주소의 레코드를 Forward Zone, Reverse Zone 중 하나 이상 가지고 있지 않을 경우에 하는 응답
- 일반적으로 집에서 사용하는 컴퓨터는 통신사(ISP)에서 기본적으로 제공하는 DNS 주소를 사용하도록 설정
    - 학교 등의 장소에서 전용선을 통해 인터넷에 연결하는 컴퓨터는 별도의 DNS를 설정

## 2-3. 등장배경
- IP 주소를 통해 Host와 Host 간에 연결하여 서로 통신하는 것이 가능
- 그러나 IP 주소를 통해 접속할 시 외우기 어렵고 변경이 될 수 있다는 단점이 존재
- 이를 해결하기 위해 DNS(Domain Name Sysyem)의 개념이 새롭게 등장

## 2-4. 구조
![9-1](https://user-images.githubusercontent.com/48504392/128643765-db11f3eb-10c8-44b6-a85e-93869d7660a6.jpg)  
- Root DNS 서버(최상위 서버) / Top-level DNS 서버 / Second-level DNS 서버 / sub DNS 서버(최하위 서버)로 구성
- 각 서버는 바로 아래의 서버 주소를 알고 있으므로 하위로 내려가면서 도메인에 맞는 IP 주소를 찾는 과정 수행

## 2-5. Domain Name Space
![9-3](https://user-images.githubusercontent.com/48504392/128647837-a6419c94-ce08-4279-abca-a74323b0fecc.png)

## 2-6. DNS 동작과정 - Recursive Query
![9-2](https://user-images.githubusercontent.com/48504392/128647838-4fc26d94-b64d-49ac-afbc-c29f7499ef11.png)
1. 클라이언트에서 `www.naver.com`을 입력 / Local DNS 서버에게 `www.naver.com`이라는 hostname에 대한 IP 주소를 질의 
    - Local DNS에 `www.naver.com`이 캐싱이 되지 않은 경우 Root DNS 정보를 포함하여 응답  
2. Root DNS 서버에 `www.naver.com` 질의
3. Top-level DNS 서버(com 도메인을 관리)의 정보를 포함하여 응답
4. Top-level DNS 서버에 `www.naver.com` 질의
5. Second-level DNS 서버("name.com" 관리)의 정보를 포함하여 응답
6. Second-level DNS 서버에 `www.naver.com` 질의
7. Local DNS 서버에게 `www.naver.com`의 IP 주소인 "222.122.195.6"을 응답 
8. Local DNS 서버는 `www.naver.com`에 대한 IP 주소를 캐싱 후 클라이언트에게 IP 주소 정보를 전달 

#
# Reference
[참조 1](https://ithub.tistory.com/337)  
[참조 2](https://namu.wiki/w/DNS)  
[참조 3](https://hihighlinux.tistory.com/47)  

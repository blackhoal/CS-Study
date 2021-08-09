# URI(Uniform Resource Identifier)
- 통합 자원 식별자
- 인터넷에 있는 자원을 나타내는 유일한 주소
- 정보나 어떠한 자원을 하나의 뜻으로 식별하기 위한 데이터 서식을 정의한 기술표준
- 인터넷에서 요구되는 기본조건으로서 인터넷 프로토콜에 항상 포함
- 웹에서 각 자원을 식별할 수 있는 이름표의 역할
- 하위 개념으로 URL과 URN이 존재

# URL(Uniform Resource Locator)
- 네트워크상에서 자원이 어디 있는지를 알려주기 위한 규약
- 인터넷상의 파일 주소를 찾아서 자원을 가져오는 것
- 절대경로와 상대경로가 존재
    - 절대경로 : 일반적으로 쓰이는 인터넷 주소
    - 상대경로 : 파일 시스템에서 사용하는 것처럼 특정 루트 디렉토리에서의 경로를 표시
- 파일 전송(FTP), 이메일(SMTP) 등 다른 응용 프로그램에서도 사용 가능
    - ftp://example.com/download.zip (FTP)
    - user@example.com (SMTP)
- 인간이 이해할 수 있는 웹상의 주소를 나타내는 문자열
    - 따라서 더 효율적인 접근을 위해 REST API와 같은 방법론이 등장
- 해당 위치에서 어떻게 리소스를 얻어낼 것인가에 대한 정보를 포함
- 자원의 형태나 환경이 변경될 때마다 수정 가능
    - URN의 등장 배경

# URN(Uniform Resource Name)
- 리소스를 유일하게 식별하는 영구적인 이름
- 리소스가 더 이상 존재하지 않거나 사용할 수 없어도, urn:scheme를 사용하는 URI는 영속적이고 독립적으로 유지 가능
- 웹 상의 위치는 제공 X
- URL에서는 파일의 위치에 따라서 URI 문자열이 변경되지만, URN은 파일이 어디에 위치할지라도 URI의 문자열이 변경 X

# URL VS URI VS URN
![3-1](https://user-images.githubusercontent.com/48504392/127522712-672cb1f3-d818-4ee9-9e46-5fdcca194fd2.png)
- URI의 하위 개념으로 URL과 URI가 존재
- URL은 `리소스의 위치` / URN은 `리소스의 이름`을 표기한 것

# Reference
[참조 1](https://velog.io/@ss-won/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-URI-URL-URN)  
[참조 2](https://itbellstone.tistory.com/86)  

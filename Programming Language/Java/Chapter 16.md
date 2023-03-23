# 1. 네트워킹(Networking)
## 1-1. 개요
- 두 대 이상의 컴퓨터를 케이블로 연결하여 네트워크를 구성하는 것
- 다수의 컴퓨터를 서로 연결하여 데이터를 손쉽게 주고받고 자원을 공유하는 것
- java.net패키지를 사용하여 네트워크 어플리케이션의 데이터 통신 부분을 쉽게 작성 가능

## 1-2. 클라이언트 / 서버(client/sever)
- 컴퓨터 간의 관계를 역할로 구분하는 개념
    - 서버(server) : 서비스를 제공하는 컴퓨터(service provider)
    - 클라이언트(client) : 서비스를 사용하는 컴퓨터(service user)
- 서비스 : 서버가 클라이언트로부터 요청받은 작업을 처리하며 그 결과를 제공하는 것
    - 서버는 제공하는 서비스의 종류에 따라 파일 서버, 메일 서버, 어플리케이션 서버 등이 존재

### 서버 기반 모델 / P2P(Peer to Peer) 모델
|서버 기반 모델|P2P 모델|
|:------:|:------:|
|안정적인 서비스의 제공이 가능|서버 구축 및 운용 비용을 절감|
|공유 데이터의 관리와 보완이 용이|자원의 활용을 극대화|
|서버 구축 비용과 관리 비용이 발생|자원의 관리가 어려움|
|X|보안이 취약|
- 서버 기반 모델 : 네트워크를 구성할 때 전용 서버를 두는 것 
- P2P 모델 : 각 클라이언트가 서버의 역할을 동시에 수행하는 것

## 1-3. IP 주소(IP address)
- 컴퓨터를 구분하는데 사용되는 고유한 값
- 인터넷에 연결된 모든 컴퓨터는 IP주소를 보유
- 4byte(32bit)의 정수로 구성되어 있으며 1byte 단위의 정수가 마침표를 구분자로 표현
- 네트워크 주소와 호스트 주소로 분류 
    - 네트워크의 구성에 따라 네트워크 주소와 호스트 주소가 각각 몇 bit를 차지하는지 상이 
    - 서로 다른 두 호스트가 가진 IP 주소의 네트워크 주소가 같음 → 두 호스트가 같은 네트워크에 포함되어 있다는 것을 의미
- IP 주소에서 네트워크 주소가 차지하는 자릿수가 많아질수록 호스트 주소의 자릿수 감소 및 네트워크의 규모 축소
- 호스트 주소에서 0은 네트워크 자신을 나타내고, 255는 브로드캐스트 주소로 사용
    - 실제 네트워크에 포함 가능한 호스트의 수는 254개
- IP 주소와 서브넷 마스크를 & 연산하면 네트워크 주소를 얻는 것이 가능 
    - 두 호스트가 같은 네트워크 상에 존재하는지 쉽게 확인하는 것이 가능

## 1-4. InetAddress
```java
import java.net.*;
import java.util.*;

public class Hello {

    public static void main(String[] args) {
        InetAddress ip = null;
        InetAddress[] ipArr = null;

        try{
            ip = InetAddress.getByName("www.daum.net");
            System.out.println("getHostName() :" + ip.getHostName());
            System.out.println("getHostAddress() :" + ip.getHostAddress());
            System.out.println("toString() :" + ip.toString());

            byte[] ipAddr = ip.getAddress();
            System.out.println("getAddress() :" + Arrays.toString(ipAddr));

            String result = "";
            for (int i=0; i < ipAddr.length; i++) {
                result += (ipAddr[i] < 0) ? ipAddr[i] + 256 : ipAddr[i];
                result += ".";
            }
            System.out.println("getAddress() + 256 :" + result);
            System.out.println();
        }
        catch(UnknownHostException e){
            e.printStackTrace();    
        }

        try{
            ip = InetAddress.getLocalHost();
            System.out.println("getHostName() :" + ip.getHostName());
            System.out.println("getHostAddress() :" + ip.getHostAddress());
            System.out.println();
        }
        catch(UnknownHostException e){
            e.printStackTrace();    
        }

        try{
            ipArr = InetAddress.getAllByName("www.daum.net");
            for (int i=0; i < ipArr.length; i++) {
                System.out.println("ipArr["+i+"]" + ipArr[i]);
            }
        }
        catch(UnknownHostException e){
            e.printStackTrace();    
        }
    }
}
```
```
[Output]
getHostName() :www.daum.net
getHostAddress() :117.52.2.25
toString() :www.daum.net/117.52.2.25
getAddress() :[117, 52, 2, 25]
getAddress() + 256 :117.52.2.25.

getHostName() :KyleYui-MacBook-Pro.local
getHostAddress() :192.168.99.1

ipArr[0]www.daum.net/117.52.2.25
ipArr[1]www.daum.net/61.111.62.165
```
- 자바에서는 IP 주소를 다루기 위한 클래스로 InetAddress를 제공

## 1-5. URL(Uniform Resource Location)
```
https://docs.oracle.com/javase/8/docs/api/java/net/InetAddress.html?type=post#index1
                          ↓
프로토콜://호스트명:포트번호/경로명/파일명?쿼리스트링#참조

프로토콜(http, https) : 자원에 접근하기 위해 서버와 통신하는데 사용되는 통신규약
호스트명(docs.oracle.com) : 자원을 제공하는 서버의 이름
포트번호(80) : 통신에 사용되는 서버의 포트번호
경로명(/javase/8/docs/api/java/net/) : 접근하려는 자원이 저장된 서버 상의 위치
파일명(InetAddress.html) : 접근하려는 자원의 이름 
쿼리(type=post) : URL에서 ? 이후의 부분
참조(index1) : URL에서 # 이후의 부분
```

```java
import java.net.URL;

public class Main {

    public static void main(String[] args) {
        try {
            URL url = new URL("http://media.daum.net/mainnews/?type=sports#index1");

            System.out.println("url.getAuthority() :" + url.getAuthority());
            System.out.println("url.getContent() :" + url.getContent().toString());
            System.out.println("url.getDefaultPort() :" + url.getDefaultPort());
            System.out.println("url.getPort() :" + url.getPort());
            System.out.println("url.getFile() :" + url.getFile());
            System.out.println("url.getHost() :" + url.getHost());
            System.out.println("url.getPath() :" + url.getPath());
            System.out.println("url.getProtocol() :" + url.getProtocol());
            System.out.println("url.getQuery() :" + url.getQuery());
            System.out.println("url.getRef() :" + url.getRef());
            System.out.println("url.getUserInfo() :" + url.getUserInfo());
            System.out.println("url.toExternalForm() :" + url.toExternalForm());
            System.out.println("url.toURI() :" + url.toURI());


        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }
}

/*
    [Output]
    url.getAuthority() :media.daum.net
    url.getContent() :[email protected]e80c
    url.getDefaultPort() :80
    url.getPort() :-1
    url.getFile() :/mainnews/?type=sports
    url.getHost() :media.daum.net
    url.getPath() :/mainnews/
    url.getProtocol() :http
    url.getQuery() :type=sports
    url.getRef() :index1
    url.getUserInfo() :null
    url.toExternalForm() :http://media.daum.net/mainnews/?type=sports#index1
    url.toURI() :http://media.daum.net/mainnews/?type=sports#index1
*/
```
- 인터넷에 존재하는 여러 서버가 제공하는 자원에 접근할 수 있는 주소를 표현하기 위한 것
- HTTP 프로토콜에서는 80번 포트를 사용
    - URL에서 포트번호를 생략하는 경우 80으로 간주
- 각 프로토콜에 따라 통신에 사용하는 포트번호가 다르며 생략되면 각 프로토콜의 기본 포트를 사용
- 자바에서는 URL을 다루기 위한 URL 클래스를 제공

## 1-6. URLConnection
```java
// 주소의 내용을 URL 클래스를 이용하여 가져온 후 콘솔에 한 줄씩 출력
public class Ex16_4 {
	public static void main(String[] args) {
		URL url = null;
		BufferedReader input = null;
		String address = "https://www.naver.com";
		String line="";
		
		try {
			url = new URL(address);
            // url.openStream() = url.openConnection().getInputStream()
			input = new BufferedReader(new InputStreamReader(url.openStream()));
			
			while((line=input.readLine())!=null) {
				System.out.println(line);
			}
			
			input.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```
- 어플리케이션과 URL 간의 통신 연결을 나타내는 클래스의 최상위 클래스이며 추상 클래스
- URLConnection을 상속받아 구현한 클래스로 HttpURLConnection과 JarURLConnection이 존재
- URL의 프로토콜이 http 프로토콜일 경우 openConnection()은 Http URLConnection을 반환
- URLConnection을 사용하여 연결을 원하는 자원에 접근 및 읽고 쓰는 것이 가능

<br>

# 2. 소켓 프로그래밍
## 2-1. TCP와 UDP
||TCP|UDP|
|:--:|:------:|:------:|
|연결방식|연결 기반 / 1:1 통신방식|비연결 기반 / 1:1, 1:n, n:n 통신방식|
|특징|데이터의 경계를 구분 X(byte-stream) / 신뢰성 있는 데이터 전송 / 느린 전송 속도|데이터의 경계를 구분함(datagram) / 신뢰성 없는 데이터 전송 / 빠른 전송 속도|
|관련 클래스|Socket / ServerSocket|DatagramSocket / DatagramPacket / MulticastSocket|

- 소켓(socket)
    - 프로세스 간의 양방향 통신에서 사용되는 양쪽 끝단을 의미
    - TCP/IP 단에서 구분할 수 있는 하나의 포트 번호를 보유
- TCP
    - 데이터를 전송하기 전에 먼저 상대편과 연결을 한 후 데이터를 전송하며 잘 전송되었는지 확인하고 실패했다면 데이터를 재전송
    - 데이터의 전송 순서가 보장
    - 패킷 관리가 필요 X
    - 신뢰 있는 데이터의 전송이 요구되는 통신에 적합(파일 전송)
- UDP 
    - 상대방과의 연결하지 않고 데이터를 전송
    - 데이터를 전송하지만 데이터가 바르게 수신되었는지 확인하지 않으므로 데이터가 제대로 전송되었는지 확인 불가
    - 데이터의 전송 순서가 보장 X
    - 패킷의 관리 필요 O
    - 데이터가 중간에 손실이 될지라도 빠른 전송이 필요할 때 적합(게임이나 동영상 등)

## 2-2. TCP 소켓 프로그래밍
```
[서버 / 클라이언트의 통신 과정]
1. 서버 프로그램에서 ServerSocket으로 서버 컴퓨터의 특정 포트에서 클라이언트의 연결 요청을 처리할 준비 수행[서버 측]
2. 접속할 서버의 IP 주소, 포트 정보를 가지고 Socket을 생성하여 서버에 연결 요청[클라이언트 측]
3. 클라이언트의 연결 요청을 받으면 서버에 새로운 Socket을 생성하여 클라이언트 Socket과 연결[서버 측]
4. 클라이언트 Socket과 새로 생성된 서버의 Socket은 연결된 각자의 Socket을 통해 1:1 통신[서버 측 / 클라이언트 측]
```
```java
// 서버 프로그램을 실행
ServerSocket serverSocket = null;
try{
	// 1. 서버 소켓을 생성
    serverSocket = new ServerSocket(7777);
}catch(IOException e) { e.printStackTrace(); }
while(true){
    try{
    	// 2.서버 소켓이 연결 요청을 처리할 수 있도록 대기 상태로 변경
        // 클라이언트 프로그램의 연결 요청을 수신하면 새로운 소켓을 생성하여 클라이언트 프로그램의 소켓과 연결
    	Socket socket = serverSocket.accept();
        // 4. 소켓의 출력 스트림을 획득
        OutputStream out = socket.getOutputStream();
        DataOutputStream dos = new DataOutputStream(out);
        // 5. 출력 스트림을 이용하여 데이터를 전송
        dos.writeUTF("Message from Server");
        // 7. 출력 스트림 해제
       	dos.close();
        // 8. 소켓 해제
        socket.close();
    } catch(IOException e) { e.printStackTrace(); }
}

// 클라이언트 프로그램
try{
    // 3. 클라이언트 프로그램에서 소켓을 생성하여 서버 소켓에 연결
    Socket socket = new Socket("127.0.0.1", 7777);
    // 4. 소켓의 입력 스트림을 획득
    InputStream in = Socket.getInputStream();
    DataInputStream dis = new DataInputStream(in);
    // 6. 입력 스트림을 이용하여 데이터를 저장
    String result = dis.readUTF();
    // 7. 입력 스트림 해제
    dis.close();
    // 8. 소켓 해제
    socket.close();
} catch(ConnectException ce) {
    ce.printStackTrace();
} catch(IOException ie){
    ie.printStackTrace();
} catch(Exception e){
    e.printStackTrace();
}
```
- ServerSocket
    - 포트와 결합되어 포트를 통해 원격 사용자의 연결 요청을 대기하며 연결 요청을 수신하면 새로운 Socket을 생성하여 상대편 Socket과 통신할 수 있도록 연결
    - 실제 데이터의 통신은 ServerSocket과 관계 없이 Socket끼리 수행
    - 한 포트에 하나의 ServerSocket만 연결 가능
    - setSoTimeOut(int timeout)을 통해 대기시간을 지정 가능
        - timeout 값이 0인 경우 제한 시간 없이 대기
        - 지정한 대기 시간이 지나면 accept( )에서 SocketTimeoutException 발생
    - 서버에 접속하는 클라이언트의 수가 많을 때는 쓰레드를 이용하여 클라이언트의 요청을 병렬적으로 처리하는 것을 권장
- 입출력 스트림
    - Socket이 데이터를 주고받는 연결통로 
    - Socket은 입력 스트림과 출력 스트림을 보유중이며 상대편 Socket의 스트림과 교차 연결
- Socket 클래스
    - 프로세스 간 통신을 담당
    - InputStream과 OutputStream을 보유중이며 두 스트림을 통해 프로세스 간의 통신이 수행

## 2-3. UDP 소켓 프로그래밍
```java
// 서버 프로그램
try{
    // 소켓 생성
    DatagramSocket Socket = new DatagramSocket(7777);
    DatagramPacket inPacket, outPacket;
    // 데이터 공간 생성
    byte[] inMsg = new byte[10];
    byte[] outMsg;
    while(true){
        // 데이터를 수신하기 위한 패킷 생성
        inPacket = new DatagramPacket(inMsg, inMsg.length);
        // 패킷을 통해 데이터를 수신
        socket.receive(inPacket);
        // 수신한 패킷으로부터 client의 IP 주소를 저장
        InetAddress address = inPacket.getAddress();
        int port = inPacket.getPort();
        // 패킷을 생성하여 client에게 전송
        String msg = "ServerMsg";
        outMsg = msg.getBytes();
        // 해당 프로그램은 패킷을 송신한 클라이언트에게 메시지를 전송
        outPakcet = new DatagramPakcet(outMsg, outMsg.length, address, port);
        socket.send(outPacket);
    }
} catch(IOException e) { e.printStackTrace(); }

// 클라이언트 프로그램
try{
    // 소켓 생성
    DatagramSocket datagramSocket = new DatagramSocket();
    // 미리 확인한 서버의 IP 주소로 InetAddress 생성
    InetAddress serverAddress = InetAddress.getByName("127.0.0.1");
    // 데이터가 저장될 공간을 byte 배열로 생성
    byte[] msg = new byte[100];
    // DatagramPacket을 수신할 상대편의 주소를 IP 주소와 포트번호로 설정
    DatagramPacket outPacket = new DatagramPacket(msg, 1, serverAddress, 7777);
    // 소켓을 통해 패킷을 전송
    datagramSocket.send(outPacket);
    // 데이터를 수신할 DatagramPakcet 생성
    DatagramPakcet inPacket = new DatagramPacket(msg, msg.length);
    // 소켓을 통해 패킷을 수신
    datagramSocket.receive(inPacket);
    // 소켓 종료
    datagramSocket.close();
} catch(Exception e){ e.printStackTrace(); }
```
- UDP 통신에서는 연결 지향이 아니므로 ServerSocket이 아닌 DatagramSocket을 사용
    - DatagramSocket를 통해 소켓을 생성하여 DatagramPacket에 데이터를 담아 전송하는 방식
- DatagramPacket은 헤더와 데이터로 구성
    - 헤더에 DatagramPacket을 수신할 호스트의 정보(호스트의 주소, 포트)가 저장
    - DatagramPacket을 전송하면 지정된 주소의 DatagramSocket에 도착

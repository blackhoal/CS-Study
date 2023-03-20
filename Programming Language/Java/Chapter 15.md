# 1. 자바에서의 입출력
## 1-1. 스트림
- 실제의 입력이나 출력이 표현된 데이터의 이상화된 흐름
- 자바에서 입출력을 수행하기 위해 두 대상을 연결하고 데이터를 운반하는데 사용되는 연결통로 
- 먼저 보낸 데이터를 먼저 받게 되어 있으며 중간에 건너뜀 없이 연속적으로 데이터를 주고 받는 구조
- 운영체제에 의해 생성되는 가상의 연결 고리를 의미하며 중간 매개자 역할을 수행
- 바이트 단위로 데이터를 전송하며 입출력 대상에 따라 다른 입출력 스트림을 사용

## 1-2. 입출력 스트림
|InputStream|OutputStream|
|:------:|:------:|
|abstract int read( )|abstract void write(int b)|
|int read(byte[] b)|void write(byte[] b)|
|int read(byte[] b, int off, int len)|void write(byte[] b, int off, int len)|

- 스트림은 단방향 통신만 가능하므로 입력과 출력을 동시에 처리 불가
    - 따라서 사용 목적에 따라 입력 스트림과 출력 스트림으로 구분
    - 입력과 출력을 동시에 수행하려면 입력을 위한 입력 스트림과 출력을 위한 출력 스트림 2개의 스트림이 필요
- 자바에서는 java.io 패키지를 통해 InputStream과 OutputStream 클래스를 별도로 제공
    - 자바에서의 스트림 생성 : 스트림 클래스 타입의 인스턴스를 생성하는 것
    - 입출력의 대상이 달라져도 동일한 방법으로 입출력 가능
- InputStream 클래스에는 read() 메소드가, OutputStream 클래스에는 write() 메소드가 각각 추상 메소드로 포함
    - 사용자는 이 두 메소드를 상황에 맞게 적절히 구현해야만 입출력 스트림을 생성하여 사용 가능
- read() 메소드는 해당 입력 스트림에서 더 이상 읽어들일 바이트가 없으면 -1을 반환
    - 반환 타입이 byte 타입일 경우 0부터 255까지의 바이트 정보는 표현 가능하지만 -1은 표현 불가
    - 따라서 InputStream의 read() 메소드는 반환 타입을 int형으로 선언

## 1-3. 바이트 기반 스트림
|입력스트림|출력스트림|입출력 대상의 종류|
|:------:|:------:|:------:|
|FileInputStream|FileOutputStream|파일|
|ByteArrayInputStream|ByteArrayOutputStream|byte 계열 메모리|
|PipedInputStream|PipedOutputStream|프로세스|
|AudioInputStream|AudioOutputStream|오디오장치|

- 스트림은 기본적으로 바이트 단위로 데이터를 전송

## 1-4. 보조 스트림
```java
// 기반 스트림을 생성
FileInputStream fis = new FileInpustStream("test.txt");

// 기반 스트림을 이용하여 보조 스트림을 생성
BufferedInputStream bis = new BufferedInputStream(fis);
// 보조 스트림인 BufferedInputStream으로부터 데이터를 읽기
bis.read(); 
```

|입력 스트림|출력 스트림|설명|
|:------:|:------:|:------:|
|FilterInputStream|FilterOutputStream|필터를 이용한 입출력 처리|
|BufferedInputStream|BufferedOutputStream|버퍼를 이용한 입출력 성능 향상|
|DataInputStream|DataOutputStream|자바의 기본 타입으로 데이터를 처리|
|ObjectInputStream|ObjectOutputStream|데이터를 객체 단위로 읽거나, 읽어 들인 객체를 역직렬화|
|SequenceInputStream|X|두 개의 입력 스트림을 논리적으로 연결|
|PushbackInputStream|X|다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가|
|X|PrintStream|다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가|

- 실제 데이터를 주고받는 스트림이 아니므로 데이터를 입출력할 수 있는 기능은 없으나 스트림의 기능을 향상시키거나 새로운 기능을 추가 가능

## 1-5. 문자 기반 스트림
|입력 스트림|출력 스트림|입출력 대상|
|:------:|:------:|:------:|
|FileReader|FileWriter|파일|
|CharArrayReader|CharArrayWriter|메모리|
|PipedReader|PipedWriter|프로세스|
|StringReader|StringWriter|문자열|

- 스트림은 기본적으로 바이트 단위로 데이터를 전송 
    - 자바에서 가장 작은 타입인 char 형이 2바이트이므로 1바이트씩 전송되는 바이트 기반 스트림에서는 원활하게 처리하기 힘든 경우가 존재 
    - 따라서 자바에서는 바이트 기반 스트림뿐만 아니라 문자 기반의 스트림도 별도로 제공
- 기존의 바이트 기반 스트림에서 InputStream을 Reader로, OutputStream을 Writer로 변경하여 사용 가능

### 문자 기반의 보조 스트림
|입력 스트림|출력 스트림|설명|
|:------:|:------:|:------:|
|FilterReader|FilterWriter|필터를 이용한 입출력|
|BufferedReader|BufferedWriter|버퍼를 이용한 입출력|
|PushbackReader|X|다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가|
|X|PrintWriter|다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가|

<br>

# 2. 표준 입출력
|클래스 변수|입출력 스트림|설명|
|:------:|:------:|:------:|
|System.in|InputStream|콘솔로부터 데이터를 입력|
|System.out|PrintStream|콘솔로 데이터를 출력|
|System.err|PrintStream|콘솔로 데이터를 출력|

```java
try {
  InputStream input = new FileInputStream("test.txt");
  System.out.println("File opened...");
} catch (IOException e){
  System.err.println("File opening failed:");
  e.printStackTrace();
}
```

- 자바에서는 표준 입출력을 위해 System.in / System.out / System.err를 제공
- 자바 어플리케이션의 실행과 동시에 사용할 수 있게 자동적으로 생성되므로 개발자가 별도로 스트림을 생성하는 코드를 작성하지 않아도 사용이 가능


## 2-1. 표준 입출력의 대상 변경
|메소드|설명|
|:------:|:------:|
|static void setIn(InputStream in)|입력 스트림의 대상을 전달된 입력 스트림으로 변경|
|static void setOut(PrintStream out)|출력 스트림의 대상을 전달된 출력 스트림으로 변경|
|static void setErr(PrintStream err)|출력 스트림의 대상을 전달된 출력 스트림으로 변경|
- System.in / System.out / System.err의 입출력 대상은 콘솔과 같은 표준 입출력 장치
    - setIn( ), setOut( ), setErr( )를 사용하면 콘솔 이외에 다른 입출력 대상으로 변경 가능

## 2-2. RandomAccessFile 클래스
```java
// RandomAccessFile 생성
// rw : RandomAccessFile의 mode 값 / Read & Write
RandomAccessFile file = new RandomAccessFile("file.txt", "rw");

// RandomAccessFile 이동
RandomAccessFile file = new RandomAccessFile("file.txt", "rw");

// 단위 : Byte
file.seek(200);

long pointer = file.getFilePointer();

file.close();

// RandomAccessFile 읽기
RandomAccessFile file = new RandomAccessFile("file.txt", "rw");

int aByte = file.read();

file.close();

// RandomAccessFile 쓰기
RandomAccessFile file = new RandomAccessFile("file.txt", "rw");

file.write("Hello World".getBytes());

file.close();
```
- 입력과 출력을 하나의 클래스로 파일에 대한 입력 / 출력을 모두 가능하도록 설계된 클래스
- 파일의 어느 위치에나 읽기 및 쓰기가 가능
- 다양한 입출력 스트림을 이용하면 파일에 순차적으로 입출력 작업을 수행 가능
    - 순차적 접근이 아닌 임의의 지점에 접근하여 작업을 수행할 때 RandomAccessFile 클래스를 사용
- 파일만을 대상으로 하며 임의의 지점에서 입출력을 동시에 수행 가능
- RandomAccessFile 클래스의 생성자에는 인수로 파일의 이름 및 파일 모드까지 함께 전달
- getFilePointer() : 파일 포인터의 현재 위치를 확인 가능
- seek() : 파일 포인터의 위치를 변경 가능

## 2-3. File 클래스
```java
// File 초기화
File file = new File("input-file.txt");

// 존재여부 확인 - 객체 생성 시점에 실제 파일이 존재하지 않아도 예외 발생 X
boolean fileExists = file.exists();

// 디렉토리 생성 - mkdir() / mkdirs()
File file = new File("c:\\users\\javastudy\\newdir");
boolean dirCreated = file.mkdir();

// 파일 길이
long length = file.length();

// Rename
boolean success = file.renameTo(new File("c:\\data\\new-file.txt"));

// 파일 삭제
boolean success = file.delete();

// 디렉토리 확인
boolean isDirectory = file.isDirectory();

// 파일 리스트
File file = new File("c:\\data");
String[] fileNames = file.list();
File[] files = file.listFiles();
```
- 자바에서는 입출력 작업 이외의 파일과 디렉터리에 관한 작업을 File 클래스를 통해 처리하도록 지원
- File 클래스는 파일과 파일의 메타데이터의 접근하는 기능만 제공
    - 만약 파일의 내용을 읽기 및 쓰기 기능을 원한다면 file Stream을 사용
    - 만약 NIO를 사용한다면 파일에 대한 접근은 java.nio.FileChannel를 사용
- File 인스턴스는 파일과 디렉토리의 형태로 존재 가능
- File 인스턴스의 생성은 파일의 경로를 입력하여 생성
    - 주로 경로를 포함하여 지정하지만 파일 이름만 사용해도 가능하며, 해당 경우 프로그램이 실행되는 위치가 경로로 간주
- 파일명이나 디렉토리명으로 지정된 문자열이 유효하지 않아도 컴파일 에러나 예외를 발생 X
- File 인스턴스를 생성했어도 파일이나 디렉토리가 생성되는 것은 X 
    - 새로운 파일을 생성하려면 File 인스턴스를 생성한 후, 출력 스트림을 생성하거나 createNewFile( )을 호출

### File 클래스 관련 메소드
|메소드|설명|
|:------:|:------:|
|boolean canRead()|해당 파일이 읽을 수 있는 파일인지를 검사|
|boolean canWrite()|해당 파일이 쓸 수 있는 파일인지를 검사|
|boolean delete()|해당 파일 또는 디렉터리를 삭제|
|boolean exists()|해당 파일이 존재하는지를 검사|
|String getPath()|해당 파일의 경로명을 문자열로 반환|
|boolean isAbsolute()|해당 파일의 경로명이 절대 경로인지를 검사|
|boolean isDirectory()|해당 파일이 디렉터리인지를 검사|
|boolean isFile()|해당 파일이 파일인지를 검사|
|long length()|해당 파일의 크기를 반환|
|boolean mkdir()|지정된 경로에 디렉터리를 생성|
|boolean mkdirs()|지정된 경로에 디렉터리를 생성 및 필요한 모든 상위 디렉터리까지 생성|
|boolean renameTo(File dest)|해당 파일의 이름을 전달된 파일 이름으로 변경|
|boolean setExecutable(boolean executable) / boolean setReadable(boolean readable) / boolean setWritable(boolean writable) / boolean setReadOnly()|해당 파일의 속성을 변경|

<br>

# 3. 직렬화
- 객체를 데이터 스트림으로 만드는 것
- 객체에 저장된 데이터를 스트림에 사용하기 위해 연속적인(serial) 데이터로 변환
- 직렬화에는 ObjectInputStream을 사용하며 역직렬화에는 ObjectInputStream을 사용
    - 두 스트림은 각각 InputStream과 OutputStream을 직접 상속받지만 보조스트림이며 객체를 생성할 때 입출력 스트림에 대한 지정 필요
- 직렬화가 가능한 클래스를 생성하려면 java.io.Serializable 인터페이스를 구현하도록 생성 
    - Serializable 인터페이스는 빈 인터페이스이지만 직렬화를 고려하여 작성한 클래스인지 판단하는 기준이 되는 역할
- 직렬화된 객체는 정의된 멤버들의 정보를 이용하여 serialVersionUID라는 클래스의 버전을 자동 생성하여 직렬화 내용에 포함
    - 역직렬화할 때 클래스의 버전을 비교함으로써 직렬화할 때의 클래스의 버전과 일치하는지 확인 가능 
    - 만약 클래스의 내용이 변경된 경우 역직렬화는 실패하며 예외를 발생

<br>

# Reference
[참조 1](http://www.tcpschool.com/java/java_io_stream)  
[참조 2](https://haeng-on.tistory.com/42?category=949596)  
[참조 3](https://recoderr.tistory.com/49?category=887628)  

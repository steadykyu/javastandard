# 1. 자바에서의 입출력

## 1.1 입출력이란?
+ I/O란 Input과 Output의 약자로 입려고가 출력, 간단히 줄여서 입출력을 말한다. 컴퓨터의 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것을 말한다.

## 1.2 스트림
+ 이전에 배운 스트림과는 다른 개념으로, 데이터를 운반하는 데 사용되는 연결통로이다.
+ 물이 한쪽방향으로만 흐르는 것처럼 스트림은 단방향 통신만 가능하기때문에, 하나의 스트림으로 입력과 출력을 동시에 처리할 수는 없다.
+ 먼저 보낸 데이터를 먼저 받게 되어있는 큐, FIFO(First in first out) 구조로 되어있다.

## 1.3 바이트 기반 스트림
+ InputStream, OutputStream
+ 스트림은 바이트단위로 데이터를 전송하며 입출력 대상에 따라 다른 종류의 입출력 스트림이 있다.

|입력스트림|출력스트림|입출력대상의 종류|
|------|----|---|
|FileInputStream|FileOutputStream|파일
|ByteArrayInputStream|ByteArrayOutputStream|메모리(byte배열)
|PipedInputStream|PipedOutputStream|프로세스(프로세스간의 통신)
|AudioInputStream|AudioOutputStream|오디오장치

+ 이들은 모두 InputStream 또는 OutputStream의 자손들이며, 각각 읽고쓰는데 필요한 추상메서드를 자신에 맞게 구현해놓았다.
+ java.io패키지를 통해 입출력을 처리할 수 있는 **표준화된 방법**을 제공함으로써 입출력의 대상이 달려져도 동일한 방법으로 입출력이 가능하다.

InputStream|OutputStream|
|----|----|
abstract int read()|abstract void write()|
int read(byte[] b)|void write(byte[] b)|
int read(byte[] b, int off, int len)|void write(byte[] b, int off, int len)|

+ 핵심인 read()와 write()는 입출력 대상에 따라 상황에 알맞게 구현하라는 의미로 추상메서드로 되어있다.
+ 아래 메서드들도 추상메서드 read(), write() 으로 구현되어 있기때문에 입출력에 있어서 이 두 메서드는 반드시 구현되어야 한다.

## 1.4 보조 스트림
+ 데이터를 입출력할수 있는 기능은 없지만, 스트림의 기능을 향상시키거나 새로운 기능을 추가하는 보조스트림이 제공된다.
+ 이중 BufferedInputStream은 버퍼를 제공하는데, 버퍼를 사용한 입출력시 성능차이가 상당하기 때문에, 대부분 버퍼를 이용한 보조스트림을 사용한다.
+ 이 외의 보조스트림은 책을 참고하자. 모든 보조스트림은 FilterInputStream의 자손들이다.
+ 이 FilterInputStream 역시 InputStream의 자손이므로 모든 보조스트림 역시 입출력 사용방식은 동일하다.

## 1.5 문자기반 스트림
+ **Reader, Writer**
+ 바이트기반은 1byte단위로 입출력하는데, java의 char형은 2byte이기때문에 어려움이 있다.
+ 그러므로 문자데이터를 입출력할때는 Reader와 Writer를 사용한다.
+ 문자기반 스트림의 이름은 바이트기반 스트림의 이름에서 InputStream, OutputStream 부분을 Reader와 Writer로 바꿔주면된다.
+ 예 ) FileInputStream -> FileReader / FileOutputStream -> FileWriter
+ 단 ByteArrayInputStream -> CharArrayReader 형식으로 바뀐다.
+ 문자기반 보조스트림역시 바이트기반 보조스트림에서 이름만 수정해주면 된다.
+ 예 ) BufferedInputStream -> BufferedReader / BufferedOutputStream -> BufferedWriter

# 2. 바이트 기반 스트림
## 2.1 InputStream과 OutputStream
+ InputStream과 OutputStream은 모든 바이트기반의 스트림의 조상이며 여러 가지 메서드를 지닌다.
+ 책참고하기
+ ByteArrayInputStream, System.in, System.out 을 제외한 스트림들은 JVM이 자동적으로 닫아주기는 하지만, 스트림을 사용한 모든 작업을 마치고 난 후에는 close()를 호출해서
반드시 닫아주어야 한다.

## 2.2 ByteArrayInputStream, ByteArrayOutputStream
+ 메모리 즉 **바이트배열에 데이터를 입출력**하는데 사용되는 스트림이다. 주로 다른곳에 입출력하기 전에 데이터를 임시로 바이트배열에 담아서
변환 등의 작업을 하는데 사용된다.
+ 스트림의 종류가 달라고 읽고 쓰는법은 동일하므로, 아래 코드처럼 다른 스트림들도 활용해보자.
```java
class IOEx4 {
    public static void main(String[] args) {
        byte[] inSrc = {0,1,2,3,4,5,6,7,8,9};
        byte[] outSrc = null;

        byte[] temp = new byte[4];

        ByteArrayInputStream  input  = null;
        ByteArrayOutputStream output = null;

        input  = new ByteArrayInputStream(inSrc);
        output = new ByteArrayOutputStream();

        try {
            while(input.available() > 0) {
                int len = input.read(temp); // 읽은건 읽은거고, 반환값으로 읽어 온 데이터의 개수를 반환한다.
                output.write(temp, 0, len); // 바로 전의 읽어 온 개수만큼만 write한다.
            }
        } catch(IOException e) {}

        outSrc = output.toByteArray();

        System.out.println("Input Source  :" + Arrays.toString(inSrc)); //[123456789]
        System.out.println("temp          :" + Arrays.toString(temp));  // [8967] 효율을 위해 덧붙여서 읽어옴.
        System.out.println("Output Source :" + Arrays.toString(outSrc));//[123456789]
    }
}
```

## 2.3 FileInputStream과 FileOutputStream
+ **파일**에 입출력을 하기위한 스트림이다.
+ 책의 생성자와 코드의 예시를 참고하자.(FileView.java, FileCopy.java)

# 3. 바이트 기반의 보조스트림
## 3.1 FilterInputStream과 FilterOutputStream
+ FilterInputStream과 FilterOutputStream는 InputStream과 OutputStream의 자손이면서 모든 보조스트림의 **조상**이다.
> 생성자
```
protected FilterInputStream(InputStream in)
public    FilterOutputStream(OutputStream out)
```
+ FilterInputStream과 FilterOutputStream의 모든 메서드는 단순히 기반스트림의 메서드를 그대로 호출할 뿐이다.
+ 즉 상속을 통해 원하는 작업 수행하도록 읽고 쓰는 메서드를 오버라이딩 해야한다.
+ FilterInputStream은 protected 하므로 인스턴스를 생성할 수 없다.
```
FilterInputStream의 자손 : BufferedInputStream, DataInputStream, PushbackInputStream등
FilterOutputStream의 자손 : BufferedOutputStream, DataOutputStream, PrintStream 등
```
## 3.2 BufferedInputStream과 BufferedOutputStream
+ 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조 스트림이다. 한 바이트보다는 버퍼(바이트 배열)를 이용해서 입출력하는것이 훨씬 빠르므로 입출력 작업에 사용된다.
> 생성자 
```
BufferedInputStream(InputStream in, int size)       : 지정된 크기의 버퍼를 갖는 BufferedInputStream인스턴스를 생성한다.
BufferedInputStream(InputStream in)                 : 8192byte의 버퍼를 가지고 인스턴스를 생성한다.
```
> 생성자 / 메서드
```
BufferedOutputStream(OutputStream out, int size)    : 지정된 크기의 버퍼를 갖는 BufferedOutputStream인스턴스를 생성한다.
BufferedOutputStream(OutputStream out)              : 8192byte의 버퍼를 가지고 인스턴스를 생성한다.
flush()                                             : 버퍼의 모든 내용을 출력소스에 출력한 후 버퍼를 비운다.
close()                                             : flush()롤 호출한후 BufferedOutputStream인스턴스가 사용하던 모든 자원을 반환한다.
```
+ BufferedOutputStream은 버퍼가 가득 찼을때만 출력소스에 출력을 하기 때문에 close()나 flush()를 호출해서 마지막 버퍼에 있는 내용이 출력소스에 출력되도록 해야한다.
+ 보조스트림을 사용한 경우에는 기반스트림의 close()나 flush()는 호출할 필요없이 단순히 보조스트림의 close()만 호출하면된다.
+ 보조스트림의 close()안에서 오버라이딩 없이 그대로 기반스트림의 close()를 호출하기 때문이다.

## 3.3 DataInputStream과 DataOutputStream
+ 각각 DataInput인터페이스, DataOutput인터페이스를 구현하였다.
+ 데이터를 읽고 쓰는데 있어 byte단위가 아닌 8가지 기본자료형의 단위로 읽고쓸수 있다는 장점이 있다.
+ 예) readInt(), readLong(), readDouble() ....etc
+ 생성자와 메서드는 책을 참고하자.
+ 각 자료형의 크기가 다르므로 출력한 데이터를(output) 다시 읽어올때는(input) 출력했을때의 순서대로 읽어와야한다.

## 3.4 SequenceInputStream
+ 여러개의 입력스트림을 **연속적으로 연결해서** 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리할수 있도록 도와준다.
```
//참고 Vector.elements() 의 반환값 - Enumeration
SequenceInputStream(Enumeration e)                      : Enumeration에 저장된 순서대로 입력스트림을 하나의 스트림으로 연결한다.
SequenceInputStream(InputStream s1, InputStream s2)     : 두개의 입력스트림을 하나로 연결한다.
```

## 3.5 PrintStream
+ 데이터를 기반스트림에 다양한 형태로 출력할 수 있는 print, println, printf와같은 메서드를 오버로딩하여 제공한다.
+ 데이터를 적절한 문자로 출력하는 문자기반 스트림의 역할이다.
+ 사실 더 많은 언어를 처리하는 PrintWriter가 있지만 오랬동안 System.out?(PrintStream) 이 대중화되어 계속 쓰이고 있다.
+ print, println, printf 메서드들은 자주 사용되기 때문에 기반스트림에서 IOException이 발생하면 예외처리를 시키는 것이 아니라 내부에서 처리하도록 정의하였다.
+ checkError()를 통해 에러를 인지할수 있다.
+ printf() 의 여러 format(형식화된 출력)은 책을 참고하자. 또는 APi document의 Formatter 클래스를 참고하자.

# 4. 문자기반 스트림
+ 바이트기반 스트림과 문자기반스트림의 사용방법은 같으며, 문자데이터를 다루는데 사용된다는 점만 다르다.

## 4.1 Reader와 Writer
+ 바이트기반 스트림의 조상이 InputStream/OutputStream 인것과 같이 문자기반스트림에서는 Reader와 Writer가 조상이다.
+ 또한 byte배열대신 char배열을 사용한다는 점이 다르다.
+ 이외에는 다른점이 없으며, 메서드 또한 다르지 않다.( 책참고)
+ 문자기반 스트림은 2byte로 스트림을 처리하는것을 넘어서, 인코딩기능이 있다.
+ Reader는 특정 인코딩을 읽어서 유니코드(UTF-16)로 변환하고, Writer는 유니크드를 특정 인코딩으로 변환하여 저장한다.

## 4.2 FilteReader와 FileWriter
+ 책과 코드참고(FileReaderEx1,FileConversion)

## 4.3 PipedReader와 PipedWritre
+ 쓰레드 간에 데이터를 주고받을 때 사용한다. 다른 스트림과 달리 입력과 출력스트림을 하나의 스트림으로 연결해서 데이터를 주고받는다.
+ 책 or 코드참고(PipedReaderWriter)
+ 쓰레드가 시작하기 전에 연결해야한다는 것에 유의하자.

## 4.4 StringReader와 StringWriter
+ CharArrayReader/CharArrayWriter와 같이 입출력 대상이 메모리인 스트림이다.
+ StringWriter에 출력되는 데이터는 내부의 StringBuffer에 저장되며 StringWriter의 메서드를 이용해서 저장된 데이터를 얻을 수 있다.
```
StringBuffer getBuffer()        : StringWriter에 출력한 데이터가 저장된 StringBuffer를 반환한다.
String toString()               : StringWriter에 출력된  (StringBuffer에 저장된) 문자열을 반환한다.
```

# 5. 문자기반의 보조스트림
## 5.1 BufferedReader와 BufferedWriter
+ 버퍼를 이용해서 입출력의 효율을 높일 수 있다.

## 5.2 InputStreamReader와 OutputStreamWriter
+ 바이트기반 스트림을 문자기반 스트림으로 연결해주는 역할을 한다. 그리고 바이트기반 스트림의 데이터를 **지정된 인코딩**의 문자데이터로 변환하는 작업을 수행한다.
```
InputStreamReader(InputStream in)                   : OS에서 사용하는 기본 인코딩의 문자로 변환하는 InputStreamReader를 생성
InputStreamReader(InputStream in, String encoding)  : 지정된 인코딩을 사용하는 InputStreamReader를 생성
String getEncoding()                                : InputStreamReader의 인코딩을 알려준다.
```

# 6.표준입출력과 File
## 6.1 표준입출력
+ **System.in, System.out, System.err**
+ 표춘입출력은 콘솔(dos창)을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미한다.
+ 위 3가지 입출력스트림은 자바APP 실행과 동시에 사용할수 있게 자동적으로 생성되므로 별도의 생성 코드를 작성하지 않아도 된다.
```
System.in       : 콘솔로부터 데이터를 입력받는데 사용
System.out      : 콘솔로 데이터를 출력하는데 사용
System.err      : 콘솔로 데이터를 출력하는데 사용
```
> System의 소스코드
```
public final class System{
        public final static InputStream in = nullInputStream();
        public final static PrintStream out = nullPrintStream();
        public final static PrintStream err = nullPrintStream();
```
+ in, out, err은 System클래스에 선언된 클래스변수(static변수)이다.
+ 타입은 InputStream, PrintStream이지만 실제로는 BufferedInputStream, BufferedOutputStream 인스턴스를 사용한다.
+ System.in에서 우리가 콘솔창에 입력하는 상태에는 버퍼를 가지고 있기때문에 수정이 가능하며, 버퍼의 크기만큼 입력이 가능하다.
+ 콘솔창의 Enter키를 누르는 것은 "\\r"(현재라인의 첫번째 컬럼), "\\n"(줄바꿈)을 실행시키는 것과 같다.

## 6.2 표준입출력의 대상 변경
```
static void setOut(PrintStream out)             : System.out의 출력을 지정된 PrintStream으로 변경 ( 콘솔창에서 '>'으로도 가능)
static void setErr(PrintStream err)             : System.err의 출력을 지정된 PrintStream으로 변경
static void setIn(InputStream in)               : System.in의 입력을 지정된 InputStream으로 변경 ( 콘솔창에서 '<'으로도 가능)
```

## 6.3 RandomAccessFile
+ 자바에서는 입력과 출력이 각각 분리되어 별도로 작업을 하도록 설계가 되어있는데, RandomAccessFile만은 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있도록 되어있다.
+ InputStream이나 OutputStream에 상속받지 않고 DataInput과 DataOutput인터페이스를 모두 구현했기에 읽기와 쓰기가 모두 가능하다.
+ DataInputStream, DataOutputStream 처럼 기본자료형 단위로 데이터를 읽고 쓸수 있다.
+ 가장 큰 장점은 파일의 어느 위치에나 읽기/쓰기가 가능하다는 것이다.
```
내부적으로 파일포인터를 사용하며,
현재 작업중인 파일에서 파일 포인터의 위치를 알고싶을때는 getFilePointer()
파일 포인터의 위치를 옮기기 위해서는 seek(long pos)나 skipBytes(int n)를 사용한다.
```
+ 생성자/메서드는 책을 참고하자.

## 6.4 File
+ 파일은 기본적이면서 가장 많이 사용되는 입출력 대상이므로 가장 중요하다.
+ 자바에서는 File클래스를 통해서 파일과 디렉토리를 다룰 수 있도록 하고있다. 그래서 File 인스턴스는 파일 일수도 있고 디렉토리 일수도 있다.
+ File의 생성자와 경로에 관한 메서드는 책을 참고하자.
+ 경로와 관련된 File의 멤버변수도 책을 참고하자.
    - OS마다 파일의 경로, 디렉토리, 파일을 이름을 구분하는 구분자가 다르다.
    - 그러므로 OS독립적으로 프로그램을 작성하기 위해서는 구분자(,)가 아닌 경로와 관련된 멤버변수(pathSeparator ...etc)들을 사용해야한다.
+ File 인스턴스를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아니다. 그러므로 파일 명이나 디렉토리명이 유효한 문자열로 작성되지 않더라도 컴파일 에러나 예외를 발생시키지는 않는다. 새로운 파일을 생성하기 위해서는 File인스턴스 생성후 createNewFile()을 호출해야한다.
```
1. 이미 존재하는 파일을 참조할때 :
File f = new File("c:\\jdk1.8\\work\\ch15","FileEx1.java");

2. 기존에 없는 파일을 새로 생성할 때 :
File f = new File("c:\\jdk1.8\\work\\ch15","NewFile.java"); // NewFile이 없어도 컴파일에러발생 X
f.createNewFile();
```
+ File 에서 사용할수 있는 메서드는 책을 참고하자!
+ 메서드를 이용한 파일 입출력은 꼭 코드를 참고해서 돌려보고, 결과를 확인해 보도록 하자.

# 7.직렬화(Serialization)
+ 객체를 컴퓨터에 저장했다가 다음에 꺼내쓰거나, 네트웍을 통해 컴퓨터 간에 서로 객체를 주고받을 수 없을까 ? 라는 고민에서 탄생한 개념이다.
## 7.1 직렬화란?
+ 직렬화란 객체를 데이터 스트림으로 만드는것으로, 객체에 저장된 데이터를 스트림에 쓰기(Write)위해 연속적인(serial) 데이터로 변환하는 것을 말한다.
+ 반대로 스트림으로부터 데이터를 읽어서 객체를 만드는 것을 역직렬화(deserialization)라고 한다.
+ 여기서 객체는 클래스에 정의된 **인스턴스 변수의 집합**이다. 객체에는 클래스변수나 메서드가 포함되지 않는다.
+ 우리는 ObjectInputStream과 ObjectOutputStream을 사용하여 손쉽게 객체를 직렬화/역직렬화 할수 있다.

## 7.2 ObjectInputStream, ObjectOutputStream
+ 직렬화에는 ObjectOutputStream을 사용하고, 역직렬화에는 ObjectInputStream을 사용한다.
+ 이 둘은 InputStream과 OutputStream을 직접 상속받지만, 기반스트림을 필요로 하는 **보조 스트림**이다.
> 직렬화 하는 방식
```
FileOutputStream fos = new FileOutputStream("objectfile.ser"); // 직렬화는 관례적으로 .ser로 저장한다.
ObjectOutputStream out = new ObjectOutputStream(fos);           // 기반스트림을 필요로한다.
out.writeObject(new UserInfo());                                // 객체를 넣어주면 직렬화 성공!
```
+ 객체를 writeObject()로 출력하면, 객체가 파일에 직렬화 되어 저장된다.
+ 역직렬화는 같은 방식으로 FileInputStream, ObjectInputStream을 써주면 된다.
+ ObjectInputStream, ObjectOutputStream에는 readObject(), writeObject() 외의 많은 메서드를 제공한다.(책 참고!, readInt(), readUTF() 등)
+ 객체를 직렬화/역직렬화 하는 작업은 **객체의 모든 인스턴스변수가 참조하고 있는 모든 객체**를 대상으로 진행되므로 상당히 시간이 오래걸린다.
+ 만약 직렬화 작업시간을 줄이고 싶다면, readObject(), writeObject()를 메서드를 구현함으로써 가능하다.(코드참고 - UserInfo2.java)

## 7.3 직렬화가 가능한 클래스 만들기
+ **Serializable, transient**
+ 직렬화가 가능한 클래스를 만드는 방법은 직렬화 하고자하는 클래스가 java.io.Serializable interface를 구현하도록 하면 된다.
+ Serializable interface는 아무런 내용도 없는 빈 인터페이스 이지만, **직렬화를 고려하여 작성한 클래스인지를 판단하는 기준**이된다.
+ Serializable을 구현한 클래스를 상속받는다면, Serializable를 구현하지 않아도 직렬화가 가능하다.
> 직렬화 클래스를 상속받는 경우
```
public class SuperInfo implements Serializable{
        String name;
        String password;
}
public class UserInfo extends SuperInfo{        //UserInfo는 직렬화가 가능
        int age;
}               
```
> 자식만 직렬화 클래스인 경우
```
public class SuperInfo {
        String name;
        String password;
}
public class UserInfo extends SuperInfo implements Serializable{
        int age;
}               
```
+ 자손클래스를 직렬화 할때, 조상클래스에 정의된 인스턴스 변수 name과 password는 직렬화 대상에서 제외된다.
+ 직렬화 되도록 하기위해서는 조상클래스에 Serializable를 구현하도록 하거나
+ readObject(), writeObject()에 readUFT(name)/writeUFT(name)과 readUFT(password)/writeUFT(name) 추가하여 직접 직렬화 해주어야한다.
+ 세부적인 내용은 책과 코드를 참고하자.

### transient
+  직렬화 하고자 하는 객체의 클래스안에 직렬화가 안되는 객체에 대한 참조를 포함하고 있다면 제어자 transient를 붙여서 직렬화 대상에서 제외되도록 할 수 있다.
+  또한 보안상 직렬화 되면 안되는 값에 대해서도 transient를 사용할 수 있다.(예) password)
```
public class UserInfo implements Serializable{
        String name;
        transient String password;  //직렬화 대상에서 제외된다.
        int     age;
        transient Object obj = new Object(); // Object객체는 직렬화 할수없으므로, 직렬화대상에서 제외시켜주어야한다.(책참고)
}
```
+ 직렬화 후 역직렬화해보면 obj와 password에는 null이 들어가있다.

## 7.4 직렬화가능한 클래스의 버전관리
+ 직렬화된 객체를 역직렬화할 때는 직렬화 했을때와 같은 버전의 클래스를 사용해야한다.
+ serialVersionUID라는 버전은 **클래스의 정의된 멤버들(인스턴스변수)**를 통해 만든다.
+ 즉 클래스의 정의된 멤버(인스턴스변수)가 동일한 클래스를 역직렬화 과정에서 사용해야 한다는 의미이다.
+ serialVersionUID는 수동으로 정의해 줄수는 있으나, 서로 다른 클래스간에 같은 값을 갖지 않도록 serialver.exe 를 활용해 자동으로 생성된 값을 사용하는 것이 보통이다.
+ serialver.exe는 클래스에 serialVersionUID가 정의되어있으면 그 값을 출력하고, 정의되어있지않으면 자동 생성한 값을 출력한다.(이때 멤버들의 정보를 바탕으로 생성함)

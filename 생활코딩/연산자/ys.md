### 입력과 출력(1/6)

#### 입력(input) :프로그램 안으로 들어오는 정보를 말한다.

> ex) 사용자가 키보드, 마우스, 터치를 조작 등

#### 출력(output) : 프로그램이 밖으로 빠져나가는 정보를 말한다.

> ex) 모니터, 스피커 등 컴퓨터가 외부로 반환해주는 것

---

###입력과 출력(2/6) :앱이 시작할 때 데이터를 입력1

#### Strings[] args의 의미

####예제1)\`\``java package org.opentutorials.javatutorials.io; // 클래스들 위치 경로

class InputDemo{ public static void main(String[] args){ System.out.println(args.length); } }

```
> InputDemo을 실행 할 때 배열에 담을 수 있는 수를 알아내는 프로그램이다.

> String[] args의미 : 문자열을 담을수 있는 배열인데 args라는 변수명을 가지고 있다. 또한 main이라는 메소드의 parameter(입력값=매개변수)을 의미한다.

> main 앞에 void가 있으므로 출력값이 존재하지 않는다 (void는 사전적인 의미로 공허함으로써 return값이 존재하지 않는 다는 의미)

> 매개변수는 method(main)가 호출 될 때 전달된 입력값을 매소드 내부로 전달하는 역할을 하는 변수이다.

> args.length는 args라는 배열이 몇개의 값을 담을 수 있는가를 나타낸다.

####예제2)
```java
public static String member(int a, int b) {
  /* int a, inb 매개변수고
  String 이라는 데이터 타입의 return값이 존재한다*/
}
```

---

#### cmd창을 통하여 eclipse에서 만들어진 class 실행방법

####1. 경로 찾기 >Package Explorer에서 경로를 알고 싶은 Project에 마우스 오른쪽 버튼 클릭 -> Properties-> Location을 확인하면 된다.

#### 2. cmd창으로 해당 경로 이동

```
C:\Users\Administrator>c:
C:\Users\Administrator>cd C:\Users\Administrator\workspace\JavaProject
```

> 그 후 cmd창으로 이동하여 해당 경로 드라이브로 이동한다. c드라이브에 있는 경우 c: 라고 입력한다. 그리고 위에 복사하였던 project의 경로를 앞에 cd 라고 쓴 뒤 붙여준다.

####3. 자바 실행 명령어

#### 형식) java -cp bin package명.class명 parameter 변수값

> -cp bin 의미 : bin이라는 디렉토리 안에 class가 존재한다

####예제)`
java -cp bin  org.opentutorials.javatutorials.io.InputDemo
`

> 위 예제는 args에 입력된 값이 InputDemo(클래스명) 뒤에 없어서 0이 출력된다.

```
C:\Users\Administrator\workspace\JavaProject>java -cp bin  org.opentutorials.jav
atutorials.io.InputDemo 1
1

C:\Users\Administrator\workspace\JavaProject>java -cp bin  org.opentutorials.jav
atutorials.io.InputDemo 2
1

C:\Users\Administrator\workspace\JavaProject>java -cp bin  org.opentutorials.jav
atutorials.io.InputDemo 3
1

C:\Users\Administrator\workspace\JavaProject>java -cp bin  org.opentutorials.jav
atutorials.io.InputDemo one two
2
```

---

###for each문

####형식) for (배열의 데이터타입 e : 배열명 ) { }

```java
String names[] = {"AA","BB","CC"};

for (String e : names) {
  System.out.println(e);
}
```

---

###입력과 출력(3/6) :앱이 시작할 때 데이터를 입력2

예제1)\`\``java package org.opentutorials.javatutorials.io;

public class InputForeachDemo{ public static void main(String[] args){ for(String e : args){ System.out.println(e); } } }\`\`\` >String[] args 라는 매개변수로 받아서 입력한 인자의 값을 > for ecah 안의 args로 받아서 출력되는 구문이다.

#### 1. cmd 창에서 예제1) InputForeachDemo를 실행해보자

```
c:
cd C:\Users\Administrator\workspace\JavaProject
C:\Users\Administrator\workspace\JavaProject>java -cp bin org.opentutorials.java
tutorials.io.InputForeachDemo one
one
```

> one 입력 시 one을 출력한다.
>
> one은 args의 첫번째 배열에 해당하며 for문의 args배열의 e안에 담아서 Sytem.out.println(e) 안으로 들어가 출력하게된다.

---

####2. eclipse창에서 예제1) InputForeachDemo를 실행해보자

run 버튼 클릭-> Run Configurations -> 왼쪽 ㅁ+ 클릭 -> Name 에 실행할 클래스명 매개변수값 입력->Arguments->Program arguments에 입력할 인자값 입력(띄어쓰기로 구분하여 넣어준다)->Run

---

###입력과 출력(4/6) :앱이 실행할 때 데이터를 입력1

```java
 package org.opentutorials.javatutorials.io;

 import java.util.Scanner;

 public class ScannerDemo {

     public static void main(String[] args) {
         Scanner sc = new Scanner(System.in);
                               //()안에 내용부터 실행되는데 (System.in)는 사용자가 입력한 값이다.
                                //Scanner라는 객체가 사용자가 입력한 값을 알아본다.
                                //이 객체를 sc라는 변수에 담아서 sc를 통해 제어한다.
                              //즉 사용자가 입력한 값을 제어한다.

         int i = sc.nextInt(); //sc는 nextInt라는 메소드를 가지고 있는데
                                           //  sc.nextInt()가 실행되면 자바는 사용자의 입력이 있을 때까지 변수 i에 값을 할당하지 않고 대기상태이다.
                                           // Console창에 값을 입력하고
                                          //i의 값으로 들어가 아래 문장을 실행하게된다
         System.out.println(i*1000);
         sc.close();
     }

 }
```

---

###입력과 출력(5/6) :앱이 실행할 때 데이터를 입력

```java
 package org.opentutorials.javatutorials.io;

 import java.util.Scanner;

 public class Scanner2Demo {

     public static void main(String[] args) {
         Scanner sc = new Scanner(System.in);
         while(sc.hasNextInt()) {  //sc는 Scanner객체가 담긴 객체고
                                          //hasNextInt메소드를 sc가 호출한다
                                        // hasNexxtInt 역할: 자바가 실행 시키면 일단 정지시키고
                                        //반복문이 대기를 유지한다.
                                        //사용자가 입력한 값이 공백이라면 대기
                                        //숫자라면 true를 return 안에 반복문 실행
                                        //문자나 특수문자라면 false 후 종료
             System.out.println(sc.nextInt()*1000);
         }
         sc.close();
     }

 }
```

> 위 예제는

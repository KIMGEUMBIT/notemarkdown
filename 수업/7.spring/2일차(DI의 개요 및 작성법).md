2일차(DI의 개요 및 작성법)
--------------------------

### 스프링은 객체를 생성하고 연결해주는 DI컨테이너

#### pom.xml ★★★

-	**spring-context, spring-expression, spring-beans** 을 작성 시<br> 자동으로 c:\users\계정명.m2\repository에 다운받아진다

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>4.2.5.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-expression</artifactId>
  <version>4.2.5.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.2.5.RELEASE</version>
</dependency>
```

#### 다운로드 못 받을시 처리하는 방법 ★★★

1.	c:\users\계정명.m2\repository
2.	전에 받은 폴더삭제 시킨후 다시 재 다운로드 받을것
3.	pom.xml을 다시 저장시킨다

---

58p, 66p

#### \<bean> 태그 : 생성할 객체를 지정

-	XML을 이용한 스프링 설정에서 컨테이너가 생성할 객체를 지정하기 위해 사용
-	스프링 컨테이너가 생성해서 보관하는 객체를 **스프링빈(Spring Bean) 객체** <br> -> 빈 객체와 연결된 이름을 이용해서 객체를 참조

![KakaoTalk_20170822_103046592](/assets/KakaoTalk_20170822_103046592.jpg)

```

```

---

-	\<property> ,<constructor-arg> 태그를 이용해 객체필요 값 설정

---

#### 파일생성

-	한글데이터 저장 : FileWriter
-	영문데이터 저장 : FileOutputStream

```java
public void out(String message) throws IOException {
  FileWriter fw = new FileWriter(filePath);
  fw.write(message);
  fw.close();
}
```

---

### setter함수의 인자가 class인 경우 xml파일에 객체 값을 넣어주는 방법 (SpringTest2/spring2)

-	형식) ★★★★★

```java
<property name="객체 변수명 ">
 <ref bean="상대방의 참조할 bean id값">
</property>
```

##### initContext.xml

```xml
<property name="outF">
  <ref bean="outFile" />
</property>
<bean id="outFile" class="spring2.OutFileImpl">
	<property name="filePath">
		<value>c:/webtest/good.txt</value>
	</property>
</bean>
```

##### MessageBeanImplDI.java

```java
private OutFile outF; // 인터페이스형으로 받아옴

public void setOutF(OutFile outF) {
  this.outF = outF;
  System.out.println("setOutF()호출됨(outF)=>" + outF);
}
```

##### OutFileImpl.java

```java
package spring2;

import java.io.*;

public class OutFileImpl implements OutFile {

    private String filePath; //경로명 및 만들어질 파일명 저장

    public void setFilePath(String filePath) {
        this.filePath = filePath;
        System.out.println("setFilePath()호출됨!"+filePath);
    }

    @Override
    public void out(String message) throws IOException {
        // TODO Auto-generated method stub
        //영문데이터 저장 -> FileOutputStream
        //한글데이터 저장 -> FileWriter
        FileWriter fw = new FileWriter(filePath);
        fw.write(message);
        fw.close();

    }
}
```

---

### SpringTest2/spring3

#### xml파일 적용 범위

-	각 패키지 에서만 적용 할 경우 "src/main/java"
-	모든 패키지에서 적용 "src/main/resources"

#### xml c혹은 p네임스페이스(태그를 추가)

-	방법 : appicationContext.xml -> Namespaces -> p 체크 -> xmlns:p="http://www.springframework.org/schema/p" 추가됨

-	**형식) <br> p:멤버변수명=값 p:멤버변수명-ref="상대방 빈즈의 id값"**

##### 변경 전

```xml
<bean id="moniter" class="spring3.SystemMoniter">
    <property name="preiodTime">
        <value>10</value>
    </property>
    <property name="sender">
        <ref bean="smsSender" />
    </property>
</bean>
```

##### 변경 후

```xml
<bean id="moniter" class="spring3.SystemMoniter"
	p:periodTime="10" p:sender-ref="smsSender" >
```

---

#### xml 불러올 객체가 2개 이상인 경우

```xml
<bean id="userRepository" class= "net.UserRepository" >
  <property name="test">
    <list>
      <ref bean="uesr1" />
      <ref bean="user2" />
    </list>
  </propetry>
</bean>
```

### spring5

##### * Main.java에서 객체 불러오기

-	형식) <br> getBean("의존성 객체를 얻어올 id, 형변환을 할 클래스명.class")

```java
//SystemMoniter moniter = (SystemMoniter) context.getBean("moniter");
SystemMoniter moniter3 = context.getBean("moniter3", SystemMoniter.class);
```

##### xml에서 ref를 불러들이기 전에

```xml
<bean id="moniter3" class="spring5.SystemMoniter"
    p:periodTime="50" >
    <property name="sender">
        <!-- <ref bean="smsSender3" /> -->
        <bean class="spring5.SmsSender">
            <constructor-arg value="true" />
        </bean>
    </property>
</bean>
```

### ArrayList

```java
import java.util.ArrayList;

private static ArrayList<integer> mArrayList;

public static void main(String[] args) {

    // ArrayList 생성
    mArrayList = new ArrayList<integer>();

    // ArrayList 값 추가
    mArrayList.add(1);
    mArrayList.add(2);

    // ArrayList 값 확인
    for(int i = 0; i < mArrayList.size(); i++) {
        System.out.println("one index " + i + " : value " + mArrayList.get(i));
    }
    System.out.println();

    // ArrayList 특정 index 값 제거
    mArrayList.remove(0);

}

```

---

### spring6

-	List, Map, Set 타입의 콜렉션 설정
-

---

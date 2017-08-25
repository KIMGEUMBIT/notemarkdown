3일차(Spring의 JDBC Programming 작성방법)
-----------------------------------------

### spring8

#### Properties

-	(키, 값) 쌍의 목록이 설정값의 목록을 의미하는 경우 사용 (java.util.Properties)
-	다음과 같은 외부설정 파일로부터 (프로퍼티, 값) 정보를 읽어올 때 사용한다.
-	\<prop> 태그의 key속성은 Properties 객체의 프로퍼티 이름
-	\<prop> 태그의 몸체 값: 프로퍼티 값
-	string 만 사용 가능

##### spring8/BookClient.java

```java
public class BookClient {
    private Properties prop; //다양한 자바의 객체자료형에 맞게

    public void setProp(Properties prop) {
        this.prop = prop;
        System.out.println("setProp()호출=>"+prop);
    }
```

##### appicationContext.xml

```xml
<!-- spring8 new BookClient()-->

<bean name="bookClient" class="spring8.BookClient" >
    <property name="prop">
        <props>
            <prop key="server">192.168.0.57</prop>
            <prop key="vonnectionTimeout">5000</prop>
        </props>
    </property>
</bean>
```

#### Map

##### ProtocolHandlerFactory.java

```
public class ProtocolHanderFactory { //비누생산공장
   //Map객체
    private Map<String,Object> map;

    public void setMap(Map<String, Object> map) {
        this.map = map;
        System.out.println("setMap()호출(map)=>"+map);
    }
}
```

##### appicationContext.xml

```xml
<bean name="protocolHanderFactory" class="spring9.ProtocolHanderFactory">
    <property name="map">
        <map>
            <entry>
                <key><value>soap</value></key>
                <ref bean="soapHandler" />
            </entry>
            <entry>
                <key><value>rest</value></key>
                <ref bean="restHandler" />
            </entry>
        </map>
    </property>
</bean>
<bean name="soapHandler" class="spring9.SoapHandler" />
<bean name="restHandler" class="spring9.RestHandler" />
```

---

### xml객체 생성 시 정적메서드 설정

-	형식 :

```
<bean id="" class="패키지명.클래스명"
factory-method="호출할 정적메서드명">
</bean>
```

```java
class ErpClientFactory{
  private Propeties prop;
  ErpClientFactory(Properties prop){
           this.prop=prop;
   }
  public static ErpClientFactory instance(){}
}
```

```
<bean id="~" class="~" factory-method="instance">
</bean>
```

-	위 ErpClientFactory 클래스의 정적 메서드를 xml에서 객체 형식으로 정적메서드 선언 해주는 방법이다 (p97)

---

#### 빈객체의 타입(type=자료형)이나 이름을 이용해서 의존성 객체를 주입(객체를 얻어와서 가져온다)<br>=>자동으로 설정 ★★★

1.	byType-> 속성(멤버변수)의 타입과 같은 타입을 갖는 빈객체를 생성-> 가져올 때 사용
2.	byName -> 속성(멤버변수)의 이름과 같은 이름을 갖는 빈객체를 생성 -> 가져올 때 사용
3.	빈즈 클래스도 상속->자식클래스도 객체를 생성 -> 가져올때 사용

#### 1. byType

-	SystemMoniter클래스에서 PhoneCall의 타입을 자동으로 찾아서 의존성 주입해주겠다.

##### before

```xml
<bean name="systemMoniter" class="spring10.SystemMoniter">
    <property name="call">
       <ref bean="phoneCall" />
    </property>
</bean>

<bean name="phoneCall"  class="spring10.PhoneCall" />
```

<b>after(byType방법을 이용하겠다)</b>

```xml
<bean name="systemMoniter" class="spring10.SystemMoniter"
           autowire="byType">
           =============>SystemMoniter클래스에서 PhoneCall의 타입을
                                            자동으로 찾아서 의존성 주입
</bean>

<bean name="PhoneCall"  class="spring10.PhoneCall" />
```

결과

```
setCall()호출(call)=>spring10.PhoneCall@e45f292
moniter=>spring10.SystemMoniter@ff5b51f
```

#### byType 사용 시 주의할 점

```
<bean name="PhoneCall"  class="spring10.PhoneCall" />
<!-- <bean name="PhoneCall2"  class="spring10.PhoneCall" /> -->
```

```
expected single matching bean but found 2: PhoneCall,PhoneCall2; nested exception is org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [spring10.PhoneCall] is defined: expected single matching bean but found 2: PhoneCall,PhoneCall2
    at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireByType(AbstractAutowireCapableBeanFactory.java:1307)
```

-	xml에서 byType 을 사용하는 경우에는 동일한 타입의 클래스가 한개이상 설정하면 안된다.

#### 2. byName

```java
private PhoneCall phonecall;//type과 이름을 같게 준다(멤버변수명)

public void setPhonecall(PhoneCall phonecall) {
  this.phonecall = phonecall;
  System.out.println("setCall()호출(phonecall)=>"+phonecall);
}
```

```xml
<bean name="systemMoniter" class="spring10.SystemMoniter" autowire="byName">
</bean>

<bean name="phonecall"  class="spring10.PhoneCall" />
```

#### 3.빈즈 클래스도 상속 -> 자식클래스도 객체를 생성 -> 가져올때 사용

-	자바에서 **추상클래스**는 **설계목적**으로써 **객체를 생성 할 수 없다.**
-	스프링컨테이너-> 해당 빈 객체를 생성하지 마라 옵션 -> **abstract=true**
-	방법 : 부모빈즈에 abstract=true -> **parent="부모빈즈의 id부여"** -> 오버라이딩 가능

```xml

///////부모 빈즈/////////////////////////////////////
<bean id="commonMoniter" class="spring11.SystemMoniter"
    abstract="true" >
    <property name="periodTime" value="10"></property>
    <property name="sender" ref="smsSender"></property>
</bean>

/////// 자식 빈즈////////////////////////////////
<!-- (3)의 자식클래스 생성 위 멤버변수 periodTime, sender을 그대로 물러받았다-->
<bean id="doorMoniter" parent="commonMoniter" />

<bean id="lobbyMoniter" parent="commonMoniter" >
    <property name="periodTime" value="30" />
</bean>

<bean id="roomMoniter" parent="commonMoniter" >
    <property name="periodTime" value="20" />
</bean>

////// 부모빈즈의 멤버변수가 참조하고 있는 클래스///////
<bean id="smsSender" class="spring11.SmsSender" />
```

#### Main.java

```java
// 4.객체를 의존성주입

//실행부모클래스명 객체명 = context.getBean("자식클래스객체", 실행부모클래스명.class)
SystemMoniter moniter = context.getBean("doorMoniter", SystemMoniter.class);
```

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.2.5.RELEASE</version>
</dependency>

<!-- dbcp(ConnectionPool) or mysql -->
<dependency>
  <groupId>commons-dbcp</groupId> <!-- 패키지 -->
  <artifactId>commons-dbcp</artifactId> <!-- 라이브러리 -->
  <version>1.4</version><!-- 버전 -->
</dependency>
```

---

### Spring3 (DB연결)

##### Bean.xml

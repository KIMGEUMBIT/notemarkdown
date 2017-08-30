6일차(SpringMVC 프로그래밍 작성법2(게시판))
-------------------------------------------

#### 에러

```
스프링 작업->에러유발->경로 X ,파일명 오타
=>HTTP Status 404 – Not Found

경고: No mapping found for HTTP request with URI
[/Springmvc/good/index.do] in DispatcherServlet with name 'test'

/good/index.do에 관련된 정보가 없다.->처리해주는 구문X (컨트롤러에 정보X)
```

#### 268, 269p, 전체흐름 ppt참조

### 0. web.xml에 DispatcherServlet을 지정해준다

```xml
<servlet>
   <servlet-name>test</servlet-name>
   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
```

![2](/assets/2.GIF)

| 구성요소             | 설명                                                                          |
|----------------------|-------------------------------------------------------------------------------|
| DispatcherServlet    |                                                                               |
| 컨트롤러(Controller) |                                                                               |
| ModelAndView         | <b>컨트롤러가 처리한 결과과 정보 및 뷰</b> 선택에 필요한 <b>정보를 담는다</b> |
| 뷰(View)             |                                                                               |

### 1. 컨트롤러 없이 바로 페이지 이동

##### test-servlet.xml

```xml
<bean name="/good/index.do"
           class="org.springframework.web.servlet.mvc.ParameterizableViewController">   <!-- 페이지이동 클래스 (고정)-->
          <property name="viewName" value="list2"></property>
                                <!-- value="페이지 이동" -->
 </bean>
```

##### index.jsp

```html
<a href="index.do">출발</a><br>
<a href="good/index.do">출발2</a><br>
<a href="index2.do">컨트롤러호출</a><br>
```

> "/good/index.do" 에서 good는 가상경로이다 해당 클래스는 페이지 이동 시 고정클래스 index.jsp에서 a href 링크 속성 값이 good/index.do인 경우 해당 servlet으로 들어가 bean name을 찾고 이동한다

---

### 2. Controller클래스 받고 페이지 이동 방법

##### 1. test-servlet.xml

-	1.요청명령어 등록(test-servlet.xml에 등록)

-	형식) \<bean name="/~/요청명령어.do" class="패키지명....컨트롤러클래스명" />

```xml
<bean name="/index2.do" class="lee/TestActionController">
</bean>
```

##### 2. Controll인터페이스 상속받는 클래스 생성

-	org.springframework.web.servlet.mvc.Controller 상속받기

```java
public class TestActionController implements Controller {
	// public String requestPro(HttpServletRequest request, HttpServletResponseresponse) throws Throwable
	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub

		System.out.println("TestAction컨트롤러의 handleRequest()호출됨!");

		ModelAndView mav = new ModelAndView();

		// <property name="viewName" value="list2"></property>
		mav.setViewName("list3");// 경로X, 파일의 확장자X, 파일으림만

		// request.setAttribute("search", search); 서버에 메모리에 다 저장
		mav.addObject("greeting", "스프링세상");

		return mav;
	}

}

```

-	상속받은 Controller 인터페이스는 기존에 했던 CommandAction과 같다

-	ModelAndView 객체는 컨트롤러가 처리한 결과과 정보 및 뷰 선택에 필요한 정보를 담는다

-	mav.setViewName("list3"); 는 자동으로 .jsp가 붙는다

```xml
<bean id="viewResolover"
class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="viewClass"
                  value="org.springframework.web.servlet.view.JstlView" />
  <property name="prefix" value="/" />
  <property name="suffix" value=".jsp" />
</bean>
```

-	mav.addObject("greeting", "스프링세상"); //기존에 request.setAttribute("search", search);

---

Springmvc2
----------

-	SpringMVC에서 환경설정파일(위 test-servlet.xml)이 꼭 하나만 존재X=>복수개의 환경설정파일을 작업할 수 있다.
-	즉,

```
(1)
hello-servlet.xml->/hello.do=>lee.HelloActionController->hello.jsp
world-servlet.xml->/world.do=>lee.WorldActionController->world.jsp

(2). 공동 대표이사 -> DispatcherServlet -> *.do 처리
누가 제일 먼저 실행 -> 누가 *. do 처리
```

#### DispatcherServlet을 지정하는 web.xml

```xml
<servlet>
   <servlet-name>hello</servlet-name>
   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
      /WEB-INF/hello-servlet.xml
      /WEB-INF/world-servlet.xml
    </param-value>
   </init-param>
   <load-on-startup>2</load-on-startup>
</servlet>

<servlet>
   <servlet-name>world</servlet-name>
   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   <load-on-startup>1</load-on-startup>
</servlet>
```

> \<load-on-startup>1</load-on-startup> : DispatcherServlet를 메모리에 올릴 실행 순서
>
> <init-param> : 파일을 불러올 경우 초기 매개변수 값 지정 실질적으로 2번이 전부 다 한다

##### hello-servlet.xml

```xml
<!-- (4) viewResolver(위치(prefix),이동할 페이지의 확장자 를 지정) -->
<bean id="viewResolover"
class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="viewClass"
                  value="org.springframework.web.servlet.view.JstlView" />
  <property name="prefix" value="/" />
  <property name="suffix" value=".jsp" />
</bean>

<!-- (2) 컨트롤러를 알려주는 역할 -->
<bean id="defaultHandlerMapping"
class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />

<!-- 요청명령어에 따른 액션클래스(=>컨트롤러액션클래스등록) -> world.jsp -->
<bean name="/hello.do" class="lee.HelloActionController">
</bean>
```

##### HelloActionController.java

```java
package lee;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

//public class ListAction implements CommandAction 모델2
public class HelloActionController implements Controller {
	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {

		System.out.println("HelloAction컨트롤러의 handleRequest()호출됨!");

		ModelAndView mav = new ModelAndView();
		mav.setViewName("hello");//
		mav.addObject("message", "클릭 하나!");

		return mav;
	}
}
```

---

SpringBoard
-----------

```
SpringBoard
       |
        -src
       |
         -WebContent->화면에 출력할 page(~)
                 |
                    - META-INF - Context.xml
                 |
                  -WEB-INF->1.web.xml(컨트롤러 등록)
                            2.board-servlet.xml(요청명령어 등록)
                         |
                          -lib->spring 라이브러리 복사(0순위)
```

##### META-INF/Context.xml

```

****JNDI 방법->DB연결****
=============
<?xml version="1.0" encoding="UTF-8"?>
<Context>
   <Resource name="jdbc/orcl(JNDI이름)" --->외부클래스(BoardDAO)
                    auth="container" ->리소스(자원(DB)를 누가 관리할 것인가?(스프링)
                    type="javax.sql.DataSource" ->커넥션풀에 대한 DataSource의 fullName
                    username="test1"  ->접속할 계정명
                    password="t1234" ->접속할 암호
                    driverClassName="oracle.jdbc.driver.OracleDriver" ->접속할 Driver명
                    factory="org.apache.commons.dbcp.BasicDataSourceFactory"
                                     ->커넥션풀을 생성해주는 클래스 fullName
                    url="jdbc:oracle:thin:@localhost:1521:orcl" ->접속 url
                    maxActive="20" ->최대로 빌려줄 수있는 커넥션풀 갯수 지정하는 속성
                    maxIdle="10" /> ->최대로 여분이 있는 커넥션풀 갯수 지정하는 속성
</Context>
```

```
 적용예)
<?xml version="1.0" encoding="UTF-8"?>
<Context>
   <Resource name="jdbc/orcl"
                    auth="container"
                    type="javax.sql.DataSource"
                    username="test1"
                    password="t1234"
                    driverClassName="oracle.jdbc.driver.OracleDriver"
                    factory="org.apache.commons.dbcp.BasicDataSourceFactory"
                    url="jdbc:oracle:thin:@localhost:1521:orcl"
                    maxActive="20"
                    maxIdle="10" />
</Context>

```

##### BoardDAO.java

```java
```

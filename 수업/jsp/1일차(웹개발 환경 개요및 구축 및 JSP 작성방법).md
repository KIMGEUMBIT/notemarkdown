### 1일차(웹개발 환경 개요및 구축 및 JSP 작성방법)

#### 웹개발 환경 개요및 구축(JSP) ★

1.	Java가 먼저 설치(JDK 8.0) -> path설정 -> 경로 상관없이 자바실행
2.	웹서버를 설치(net, php, jsp)

3.	톰캣서버 설치(무료) -> 환경설정(무료)

4.	http://jakarta.apache.org -> http://tomcat.apache.org/

5.	가장 최신버전의 바로 밑의 버전을 설치할 것

6.	설치방법 : <br>(1) zip으로 다운받아서 압출풀기 <br> --> 환경설정은 나중에 해야한다. <br> --> 오라클서버와 충돌을 방지하기위하여 8080 포트를 변경해야한다 (2) <br>•Core: ◦ zip (pgp, md5, sha1) <br>◦ tar.gz (pgp, md5, sha1) <br>◦ 32-bit Windows zip (pgp, md5, sha1) <br>◦ 64-bit Windows zip (pgp, md5, sha1) <br>◦ <b>32-bit/64-bit Windows Service Installer (pgp, md5, sha1) </b><br> <br> -> Ex체크 -> HTTP/1.1 Connector Port : 8090<br> ->관리자계정(Tomcat Administrator Login -> User Name, Password)-> admin/1234 <br> -> Java Virtual Machine : 자바가 설치된 상태라면 설치경로가 나온다(톰켓서버->자바연결) "C:\Program Files\Java\jre1.8.0_131"<br> -> C:\Program Files\Apache Software Foundation\Tomcat 8.5를 C:\Tomcat 8.5 변경

---

#### Tomcat 8.5 (위 설치 폴더)

1.	bin (서버 가동)

2.	Tomcat8.exe (권장) 디버깅창 역할

3.	Tomcat8w.exe (대화상자 형태로 제공)

4.	lib

5.	servlet-api (서블릿에 관련된 라이브러리 파일명) ★★

---

#### Tomcat 제대로 설치 됐나 확인방법

1.	톰캣서버 가동 후 (Tomcat8.exe (권장) 디버깅창 역할)
2.	웹브라우저에 http://localhost:8090 =>고양이그림홈페이지

혹은

1.	개발편집도구 -> 이클립스를 사용하여

```


```

---

### 이클립스의 웹프로그래밍을 위한 환경설정 ★

1.	작업영역 변경

파일->switch workspace->변경=>C:\webtest\4.jsp\sou other

1.	웹프로젝트를 생성->JspStudy

2.	서버를 등록=>유료서버(회사)->Resin,WebLogic,,,맞춰서 등록

3.	이클립스->서버(톰캣서버의 위치를 지정)->자바의 설치위치(서블릿때문에) ->그림참조

4.	Dynamic Web Project

5.	priject name : jspStudy

6.	Target runtime : New Runtime -> 해당서버에 맞춰서 등록.. <br> Apache Tomcat v8.5 선택, Create a new local server 체크

7.	Apache Tomcat v8.5 <Br> C:\Tomcat 8.5 <br> JRE : jdk1.8<br>

-> Context root : jspStudy(프로젝트 이름) <br> -> Content directory:WebContent : WebContent(작업영역 -> html, css, js, jsp) <br> -> Generate web.xml deployment descriptor -> <br> xml파일을 가지고 환경설정

---

#### 웹프로그럄의 구조(=웹프로젝트=웹컨텍스트=웹어플리케이션) ★

```

   웹프로그램의 구조(=웹프로젝트=웹컨텍스트=웹어플리케이션)

           |
            -Java Resources
                      |
                       -src=>자바파일(->서블릿파일)이 저장된 곳
            |
              -WebContent(/)->c:/=>메인페이지(index.html, index.jsp
                                                                                    or
                                                                                   main.jsp)
                          -images->이미지저장
                          -js
                        |
                         -META-INF=>DB연동할때 사용되는 Xml파일 저장되는곳
                        |
                         -WEB-INF
                                |
                                  -lib->외부의 자바의 라이브러리 파일이 저장되는위치
                                          ojdbc6.jar저장
                                |
                                 -web.xml->*웹프로그래밍에서 사용할 환경설정파일*




```

#### 환경설정

-	Windows->Preferences -> Server ->Runtime Environments -> 등록된 서버를 지우고 재등록 가능

-	Web -> css, html, jsp 를 UTF-8로 설정을 할것

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

---

#### Jsp 페이지의 구성요소

-	자바작성 <% %> 를 이용하여 자바코드를 사용할 수 있는 영역(Scriptlet)->지역변수 선언, 제어문
-	System.out.println(); // 콘솔용
-	out.println(); // 웹 용

```jsp
<%
//자바코드를 사용할 수 있는 영역(Scriptlet) -> 지역변수선언, 제어문
    String str = "홍길동";
    System.out.println("str=>"+str); //디버깅용
    out.println("str=>"+str); // 웹  
%>
```

-	http://도메인이름:포트번호/작업프로젝트명/요청문서이름.jsp or .html
-	http://localhost:8090/JspStudy/hello.jsp
-	http://192.168.0.57:8090/JspStudy/hello.jsp

---

---

```
스크립트(Script) 요소란? ★★★★★
스크립트 요소란 JSP 프로그래밍에서 사용되는 자바 코드의 표현형태
서블릿 코드로 자동 변환되는 성질을 가지고 있다.

JSP의 스크립트(Script) 요소들

1. 스크립트릿(Scriptlet)
   JSP의 자바 코드를 작성하는 부분
   <% ~ %>
2. 선언문(Declaration)
   페이지 전체에서 참조될 멤버 변수나 멤버 메소드를 선언
   <%! ~ %>
3. 표현식(Expression)
   HTML에 출력될 자바 변수의 내용을 표현하기 위해 사용
   <%=  ~ %>
```

---

#### Jsp의 스크립트(Script) 요소들★

```
웹페이지를 요청 (request)
링크문자열 클릭, 버튼클릭, url
응답(response)
```

1.스크립트릿-> 웹상에서 자바코드를 사용할 수 있도록 해주는 영역

-	<%%> <br>태그사용 안된다,<br>자바스크립트도 안됨<br>표현식도 사용불가( <%=count%> )

2.표현식(expression)

-	간단하게 출력문 대용으로 사용, 변수값 출력, 메서드 호출

```
형식)
<%=변수명%>
<%=객체명.일반메서드명(~)%>
<%=정적메서드명(~)%>
```

3.선언문(Declaration)

-	스크립트릿과 모양이 유사하다
-	<%! ~ %> => 자바코드를 사용이 가능(멤버변수 선언)
-	선언된 위치에 상관없이 변수를 불러다가 사용이 가능하다
-	메서드 작성이 가능-> 정적메서드 느낌

```jsp
<%! ~ %>
* 자바코드를 사용이 가능(멤버변수 선언)
*
```

```jsp
<%=count%>
<% //출력 불가능
   // <%! 로 쓰면 출력 가능
int count=3;
%>
```

```jsp
<%!
    String name="홍길동";

    public String getName(){ //===>따로 클래스를 만들어서 사용
        return name;            // DTO(Data Transfer Object)
                            // DAO(Data Apach ???????????적어라)
    }
%>
```

-	jsp내부에서 따로 메서드를 선언해서 호출허는구문을 사용 잘안함<br> why? 디자이너(html,css,js) + 소스코드(메서드 작성) = > 화면이 복잡하기때문에
-	매소두부분--> 따로 클래스로 만들어서 웹에서 불러오는 경우 자바빈즈클래에서 작업을한다->액션태그

---

#### 4. 주석(comment) or 지시어(태그와 태그사이에 적절히 배치)

-	\<!-- 눈에 보이는 주석 --> ==> html 주석
-	<%-- 눈에 안보이는 주석 --%> =>
-	//, /* ~ */ 자바주석

##### 주석을 사용할 시 주의할점

-	주석내부에 표현식을 사용이 가능하다
-	주석내부의 표현식을 잘못 사용하시면 에러가 유발
-	주석내부의 표현식에 자바주석을 사용이 가능하다

```jsp
<!-- 주석안에 표현식이 가능하다 -->
<!-- 5+3=<%=5+3%> -->
<!-- 5+3=<%=9+3 /* 표현식 안에 자바주석 가능*/ %> -->
<!-- 5+3=<%=5+3%> -->
```

##### 예제

```Java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<%!
    String name="홍길동";

    public String getName(){
        return name;
    }
%>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>JSP 페이지 3번째 표현식</title>
</head>
<body>
<%
    float f= 2.3f;
    int i=Math.round(f); //클래스명.정적메서드명(~)
    //날짜
    //외부의 클래스 불러오는 방법
    //import (권장) 2.상위패키지명.하위패키지명...참조할클래스명~
    java.util.Date d = new java.util.Date();
    out.println("d의값=>"+d); //d.toString()
%>

정수 f의 반올림값은? <%=i%><p>
현재의 날짜와 시간은? <%=d.toString()%>
선언문의 메소드를 호출? <%=getName()%>
</body>
</html>
```

---

#### 배포하는 방법 (export) ★★★

프로젝트 선택하여 마우스 오른쪽 클릭 -> export-> web-> war file -> next -> Destination 의 위치 선정 -> Export source files, Overwrite existing file 체크-> finish

#### import ★★★★

마우스 오른쪽 -> import -> war file ->

/////////////이부분 강사님 txt 적기

##### 삭제하는 방법

지우고자하는 프로젝트명 선택->마우스 오른쪽 클릭 -> delete -> delete projext contents on disk (cannot be undone) 체크 -> on disk상에 존재하는 물리적인 프로젝트도 같이 삭제할것

---

#### 배열을 이용한 테이블 출력

```java
<%! // <%! 를 사용하여 안에 선언 할 경우 전역으로 출력
String keyword[] = {"Expression","Script","Declaration","Comment"};
%>

<%
for(int i=0; i<keyword.length; i++){
    //out.println(keyword[i]);
}
%>

<table border=1>
<% for(int j=0; j<keyword.length; j++){ %>
<tr>
    <td><%=j%></td>
    <td><%=keyword[j]%></td>
</tr>
<% } %>
</table>
```

---

#### import 사용하는 방법

##### 1. 상위패키지명.하위패키지명.클래스명 붙이기

```java
java.util.Date d = new java.util.Date();
```

##### 위에 import 선언해주기

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import= "java.util.Date" // ; 안붙인다!
%>

/////////생략

<body>
<%
Date d = new Date();
%>
</body>
```

---

##### 웹프로그래밍=>jsp (main()을 가진 클래스 작업)

-	링크로 연결

-	요청을 하는 페이지(입력받는 페이지)

-	요청을 받아서 처리해주는 페이지

-	화면구현 페이지(회원가입), 로그인창

---

#### 실시간 문서 보면서 만들기

-	해당 jsp 마우스 오른쪽 버튼 클릭-> Open with -> web page editor

---

#### form 전송방식

1.	get :

2.	url창에 전송되는 데이터가 화면에 출력<br>

3.	보안에 취약

4.	적은양의 데이터 전송

5.	사이트 방문 시 default값

6.	post :

7.	회원가입, 로그인할 때, 파일업로드 할 때 사용

8.	url창에 전송되는 데이터가 화면에 출력X

#### HTTP Status 404 – Not Found 에러

-	그런페이지 없다
-	http://localhost:8090/jspStudy/control/ifirst.jsp?name=&color=blue
-	?매개변수명=전송할값&매개변수명2&전송할값2

---

#### form 전송/ 값 받기 get

```java
<form method="get" action="#">
이름 : <input type="text" name="name">


//..생략
request.setCharacterEncoding("UTF-8");
// method 받는 방식이 post인경우 안깨지게 하기 위해 선언
String name = request.getParameter("name");

```

p756, 757 ★

---

#### input.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>사용자로부터 값을 입력(전송폼)</title>
</head>
<body>
    <h1>이름, 색상을 입력</h1>
    <form method="get" action="ifirst.jsp">
        이름 : <input type="text" name="name">
        <p>
            좋아하는 색상 : <select name="color">
                <option value="blue">파란색</option>
                <option value="red">붉은색</option>
                <option value="orange">오렌지색</option>
                <option value="etx">기타</option>
            </select><p>
            <input type="submit" value="보내기">
    </form>
</body>
</html>
```

#### ifirst.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>요청을 받아서 처리해주는 페이지</title>

<%! //문서 전체에서 사용하고자하는
    String msg; //전달받은 값이 영어-> 한글로 바꿔서 출력(색상)
%>

<%
    //요청을 받아서 처리해주는 구문-> requests 내장객체(매개변수), response(응답객체)

    String name = request.getParameter("name");
    String color = request.getParameter("color");

    System.out.println(name+"/"+color);

    if(color.equals("blue")){
        msg = "파란색";
    } else if(color.equals("red")) {
        msg = "붉은색";
    } else if (color.equals("오렌지")) {
        msg = "오렌지색";
    } else {
        color="white";
        msg = "기타색(흰색)";
    }

%>

</head>
<body bgcolor="<%=color%>">
    <%=name%>님이 좋아하는 색상은 <%=msg%> 입니다.
</body>
</html>
```

---

```
1일차(웹개발 환경 개요및 구축 및 JSP 작성방법)
============================
1.웹개발 환경
2.JSP 프로젝트 구성->4가지 구성
3.활용->배포(.war 파이로)
*****************
웹개발 환경 개요및 구축(JSP)
*****************

1.Java 가 먼저 설치(JDK 8.0)->path설정->경로 상관없이
                                                             자바실행

2.웹서버를 설치->1).net  2)php 3)jsp
                                              ===
=>아파치서비->톰캣서버를 설치(9.7MB)->환경설정(무료)
=>유료(Resin,웹스피어,웹로직,,,,)->설치X

http://jakarta.apache.org->tomcat.apache.org
                                         --------------------->
=>가장 최신버전의 바로 밑의 버전을 설치할것.

Tomcat 8.5.16
==========

Binary Distributions

•Core:
 ◦ zip (pgp, md5, sha1)
->1)~.zip->다운받아서 압축풀면 끝->환경설정을 나중에 해야된다
                                                    8080->포트를 변경해야한다
                                                   ->오라클 서버와 충돌 방지(포트변경)

◦ tar.gz (pgp, md5, sha1) ->리눅스
◦ 32-bit Windows zip (pgp, md5, sha1)
◦ 64-bit Windows zip (pgp, md5, sha1)
◦ 32-bit/64-bit Windows Service Installer (pgp, md5, sha1)
 =>2) 다운->프로그램처럼 설치과정->설치하면서 환경설정(포트변경)
 =>9.7MB

 설치과정

1.환영메세지-->examples(체크유무)기본 예제 설치
 --->***포트번호(HTTP) 8080->8090***
 --->관리자계정->admin/1234
                          --------------
 ---->Java Virutal Machine=>자바가 설치된
       상태->설치경로가 나옴=>C:\Program Files\Java\jre1.8.0_131
       (톰캣서버->자바와 연결)
 ----->설치경로->C:\Tomcat 8.5 -->설치 끝
                          ==========
           Run Apache Tomcat
           Show Readme       =>둘다 체크 해제
=============================
  Tomcat 8.5->bin->Tomcat8.exe (권장) 디버깅창 역할
                               Tomcat8w.exe (대화상자 형태로 제공)

                      **===============================
                      lib->servlet-api.jar(서블릿에 관련된 라이브러리 파일명)
                      **=================================
 톰캣서버를 가동시킨 후
시작/실행->http://localhost:8090 =>고양이 그림 홈페이지
                                         ====
-------------------------------------------------------------
3.개발 편집 도구->editplus(불편 O)->Eclipse로 개발=>실행X or 실행 O

**이클립스의 웹프로그래밍을 위한 환경설정**

1.작업영역 변경

파일->switch workspace->변경=>C:\webtest\4.jsp\sou
                                       other

2.웹프로젝트를 생성->JspStudy

  서버를 등록=>유료서버(회사)->Resin,WebLogic,,,맞춰서 등록

  이클립스->서버(톰캣서버의 위치를 지정)->자바의 설치위치(서블릿때문에)
             ->그림참조

  ->Context root->JspStudy (프로젝트이름)
  ->Content directory:WebContent(/) (작업영역)->html,css,js,jsp

Generate web.xml deployment descriptor ->반드시 체크->web.xml 생성
==>xml 파일을 가지고 환경설정

   웹프로그램의 구조(=웹프로젝트=웹컨텍스트=웹어플리케이션)

           |
            -Java Resources
                      |
                       -src=>자바파일(->서블릿파일)이 저장된 곳
            |
              -WebContent(/)->c:/=>메인페이지(index.html, index.jsp
                                                                                    or
                                                                                   main.jsp)
                                               hello.jsp(1)
                          -images->이미지저장
                          -js
                                  abc->imsi.jsp(2)
                                           dec.jsp(3)
                                  sub->comment.jsp(4)
                                  control->array.jsp(5)
                        |
                         -META-INF=>DB연동할때 사용되는 Xml파일 저장되는곳
                        |
                         -WEB-INF
                                |
                                  -lib->외부의 자바의 라이브러리 파일이 저장되는위치
                                          ojdbc6.jar저장
                                |
                                 -web.xml->*웹프로그래밍에서 사용할 환경설정파일*

=>General
            Appearance
                  Colors and Fonts=>Basic
                                              Text Font

  Server->Runtime Environments->등록된 서버를
                                                     편집(지우고 재등록)
  Web->css,html,jsp=>UTF-8로 설정을 할것
 ================================
 html or jsp파일 작성->page 지시어+html 문서

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
            +
   html문서
========================================

http://localhost:8090/JspStudy/hello.jsp

http://192.168.0.57:8090/JspStudy/hello.jsp
http://도메인이름:포트번호/작업프로젝트명/요청문서이름.jsp or .html
        상대방의 ip주소 or 도메인이름
        192.168.0.57

**Jsp페이지의 구성요소**

 웹페이지를 요청(request)-------------------->
   링크문자열 클릭,버튼클릭,url
                         html문서<--------------------응답(response)

1.스크립트릿->웹상에서 자바코드를 사용할 수 있도록 해주는 영역
                  <% ~  %> =>태그사용X(자바스크립트 X)
                                         표현식도 사용불가

  http://localhost:8090/JspStudy/abc/imsi.jsp
                                               =
                                            WebContent

2.표현식(expression)->간단하게 출력문 대용으로 사용
                                변수값 출력,메서드호출->결과값을 출력

 형식) <%=변수명%> or <%=객체명.일반메서드명(~)%>
                                     <%=정적메서드명(~)%>

3.선언문(Declaration)->스크립트릿과 모양이 유사하다.

<%!  ~ %> =>자바코드를 사용이 가능(정적 멤버변수 선언)
                          ->선언된 위치에 상관없이 변수를 불러다 사용 가능
                          ->메서드 작성이 가능->정적메서드 느낌
                          ->사용이 가능한데 현실적으로 사용X

 <%!
      String name="홍길동";

      public String getName(){ ===>따로 클래스를 만들어서 사용
          return name;                  DTO(Data Transfer Object)
                                                      DAO(Data Access Object)
      }
%>

 jsp페이지 내부에서 따로 메서드를 선언해서 호출하는 구문을 잘 사용X

 디자이너(html5,css,js)+소스코드(메서드 작성)=>화면이 복잡

=>메서드부분-->따로 클래스로 만들어서 웹에서 불러오는 경우
                         자바빈즈클래스에서 작업을 한다.->액션태그
----------------------------------------------------------------------

4.주석(comment) or 지시어
    comment.jsp로 작성

 JSP 주석->태그와 태그사이에 적절히 배치

1.<!-- 눈에 보이는 주석 --> =>html주석
2.<%-- 눈에 안보이는 주석 --%>
3.자바주석-> //, /*  ~ */

주석을 사용할 시 주의할점

1.주석내부에 표현식을 사용이 가능하다.
2.주석내부의 표현식에 자바주석을 사용이 가능하다.
3.주석내부의 표현식을 잘못 사용하시면 에러가 유발
========================================

An error occurred at line: [13] in the jsp file: [/sub/comment.jsp]
Syntax error, insert ";" to complete Statement
===========>comment.jsp의 13째 라인에서 문법적인 오류

10: </head>
11: <!--  5+3=<%=5+3%> -->
12: <!--  9+3=<%=9+3/* ?옄諛붿＜?꽍?쓣 ?궗?슜媛??뒫?븯?떎.   */%> -->
13: <!--  10*3=<% 10*3 %> -->

서버에서 요청한 문서를 찾는다.(comment.jsp)=>html문서로 만들어서
 클라이언트에게 재 전송=>자기 브라우저의 소스보기

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<!-- JSP주석 연습입니다. -->

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Jsp의 주석(4)</title>
</head>
<!--  5+3=8 -->
<!--  9+3=12 -->
<!--  10*3=30 -->

<body bgcolor="yellow">
   <h1>JSP 주석을 확인하는 예제</h1>
</body>
</html>


=====================================
         <%!
                    private int total=0;

                    public int getTotal(){
                       return total;
                    }
            %>

            <%=getTotal()%>=>표현식에 메서드를 호출할때 주의할점
            <%=getTotal();%>
                                  == X
=========================================
**
배포하는 방법->프로젝트를 선택 오click
                       =>export->압축할 파일이 보관할 위치를 지정
                        ->프로젝트명.war(Web Archive)

불러오는 방법->오click->import=>war선택->프로젝트 불러오기
**
======================================
프로젝트를 제대로 삭제하는 방법

지우고자하는 프로젝트명 오click=->Delete->on disk상에 존재하는 물리적인
                                                            프로젝트도 같이 삭제할것.
==========================================
 class 1개(main)->class 2개, 3개
====================
웹프로그래밍=>jsp (main()를 가진 클래스 작업)

                                          링크로 연결->페이지 이동

1.요청을 하는 페이지(입력받는페이지)->2.요청을 받아서 처리해주는 페이지
   화면구현 페이지(회원가입),로그인창
    html,css,js로 구성==>input.jsp(보내기)->iftest.jsp

    Scanner->이름,색깔
===================================
 데이터 전송 방식
 =========
 form method="get"->url창에 전송되는 데이터가 화면에 출력
                                 보안에 취약,적은양의 데이터 전송
                                 사이트 방문시 (default)

http://localhost:8090/JspStudy2/control/iftest.jsp?name=test&color=red
                                                            ======
요청페이지(iftest.jsp)?매개변수명(name)=전송할값&매개변수명2(color)=전송할값

                        post"->회원가입,로그인할때, 파일업로드 할때 사용
                                  url창에 표시X

 404->경로 X ,파일명이 틀린경우


전송할때 Get방식-->

name=>테스트,color=orange==>get으로 전송을 받았을 경우
---------------------------------------------------------------------
name=>?????¤??¸2,color=orange=>post방식으로 전송을 받았을 경우
---------------------------------------------------------------------------
 JSP 내부 처리 과정

 다시 정리하는 예제
 지시어 정리(속성)
 =======
 서블릿의 개요 및 작성방법(3장 예습)
 =====================


















```

---

중요

\** 배포하는 방법->프로젝트를 선택 오click =>export->압축할 파일이 보관할 위치를 지정 ->프로젝트명.war(Web Archive)

불러오는 방법->오click->import=>war선택->프로젝트 불러오기

\*\*
====

프로젝트를 제대로 삭제하는 방법

지우고자하는 프로젝트명 오click=->Delete->on disk상에 존재하는 물리적인 프로젝트도 같이 삭제할것.

```java

request.setCharacterEncoding("UTF-8");
request.getParameter("name");
```

```
웹개발 환경 개요및 구축(JSP)
*****************

1.Java 가 먼저 설치(JDK 8.0)->path설정->경로 상관없이
                                                             자바실행

2.웹서버를 설치->1).net  2)php 3)jsp
                                              ===
=>아파치서비->톰캣서버를 설치(9.7MB)->환경설정(무료)
=>유료(Resin,웹스피어,웹로직,,,,)->설치X

http://jakarta.apache.org->tomcat.apache.org
                                         --------------------->
=>가장 최신버전의 바로 밑의 버전을 설치할것.

Tomcat 8.5.16
==========

Binary Distributions

•Core:
 ◦ zip (pgp, md5, sha1)
->1)~.zip->다운받아서 압축풀면 끝->환경설정을 나중에 해야된다
                                                    8080->포트를 변경해야한다
                                                   ->오라클 서버와 충돌 방지(포트변경)

◦ tar.gz (pgp, md5, sha1) ->리눅스
◦ 32-bit Windows zip (pgp, md5, sha1)
◦ 64-bit Windows zip (pgp, md5, sha1)
◦ 32-bit/64-bit Windows Service Installer (pgp, md5, sha1)
 =>2) 다운->프로그램처럼 설치과정->설치하면서 환경설정(포트변경)
 =>9.7MB

 설치과정

1.환영메세지-->examples(체크유무)기본 예제 설치
 --->***포트번호(HTTP) 8080->8090***
 --->관리자계정->admin/1234
                          --------------
 ---->Java Virutal Machine=>자바가 설치된
       상태->설치경로가 나옴=>C:\Program Files\Java\jre1.8.0_131
       (톰캣서버->자바와 연결)
 ----->설치경로->C:\Tomcat 8.5 -->설치 끝
                          ==========
           Run Apache Tomcat
           Show Readme       =>둘다 체크 해제
=============================
  Tomcat 8.5->bin->Tomcat8.exe (권장) 디버깅창 역할
                               Tomcat8w.exe (대화상자 형태로 제공)

                      **===============================
                      lib->servlet-api.jar(서블릿에 관련된 라이브러리 파일명)
                      **=================================
 톰캣서버를 가동시킨 후
시작/실행->http://localhost:8090 =>고양이 그림 홈페이지

**이클립스의 웹프로그래밍을 위한 환경설정**

1.작업영역 변경

파일->switch workspace->변경=>C:\webtest\4.jsp\sou
                                      other

2.웹프로젝트를 생성->JspStudy

 서버를 등록=>유료서버(회사)->Resin,WebLogic,,,맞춰서 등록

 이클립스->서버(톰캣서버의 위치를 지정)->자바의 설치위치(서블릿때문에)
            ->그림참조

 ->Context root->JspStudy (프로젝트이름)
 ->Content directory:WebContent(/) (작업영역)->html,css,js,jsp

Generate web.xml deployment descriptor ->반드시 체크->web.xml 생성
==>xml 파일을 가지고 환경설정

  웹프로그램의 구조(=웹프로젝트=웹컨텍스트=웹어플리케이션)

          |
           -Java Resources
                     |
                      -src=>자바파일(->서블릿파일)이 저장된 곳
           |
             -WebContent(/)->c:/=>메인페이지(index.html, index.jsp
                                                                                   or
                                                                                  main.jsp)
                                              hello.jsp(1)
                         -images->이미지저장
                         -js
                                 abc->imsi.jsp(2)
                                          dec.jsp(3)
                                 sub->comment.jsp(4)
                                 control->array.jsp(5)
                       |
                        -META-INF=>DB연동할때 사용되는 Xml파일 저장되는곳
                       |
                        -WEB-INF
                               |
                                 -lib->외부의 자바의 라이브러리 파일이 저장되는위치
                                         ojdbc6.jar저장
                               |
                                -web.xml->*웹프로그래밍에서 사용할 환경설정파일*

=>General
           Appearance
                 Colors and Fonts=>Basic
                                             Text Font

 Server->Runtime Environments->등록된 서버를
                                                    편집(지우고 재등록)
 Web->css,html,jsp=>UTF-8로 설정을 할것
================================
html or jsp파일 작성->page 지시어+html 문서

<%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
           +
  html문서
========================================

http://localhost:8090/JspStudy/hello.jsp

http://192.168.0.57:8090/JspStudy/hello.jsp
http://도메인이름:포트번호/작업프로젝트명/요청문서이름.jsp or .html
       상대방의 ip주소 or 도메인이름
       192.168.0.57

```

스크립트(Script) 요소란? ★★★★★ 스크립트 요소란 JSP 프로그래밍에서 사용되는 자바 코드의 표현형태 서블릿 코드로 자동 변환되는 성질을 가지고 있다.

JSP의 스크립트(Script) 요소들

1.	스크립트릿(Scriptlet) JSP의 자바 코드를 작성하는 부분<% ~ %>
2.	선언문(Declaration) 페이지 전체에서 참조될 멤버 변수나 멤버 메소드를 선언<%! ~ %>
3.	표현식(Expression) HTML에 출력될 자바 변수의 내용을 표현하기 위해 사용<%= ~ %>

4.	4.주석(comment) or 지시어 comment.jsp로 작성

JSP 주석->태그와 태그사이에 적절히 배치

1.<!-- 눈에 보이는 주석 --> =>html주석 2.<%-- 눈에 안보이는 주석 --%> 3.자바주석-> //, /* ~ */

servlet-api (서블릿에 관련된 라이브러리 파일명) ★★

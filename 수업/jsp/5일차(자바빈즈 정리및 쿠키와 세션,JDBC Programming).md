### 5일차(자바빈즈 정리및 쿠키와 세션,JDBC Programming).md

#### 자바스크립트 select의 index, value값 가져오기

```javascript
<script>
  alert(document.form_name.select_name.selectedIndex);
  alert(document.form_name.select_name.value);
</script>
```

##### cal.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>요청을 하는 페이지(빈즈이용)</title>
</head>
<body>
 <center>
   <h3>계산기</h3>
   <form name="form1" method="post" action="calResult.jsp">
    <input type="text" name="num1" width="200" size="5">
    <select name="operator">
       <option selected>+</option>
       <option>-</option>
       <option>*</option>
       <option>/</option>
    </select>
    <input type="text" name="num2" width="200" size="5"><p>
    <input type="submit" value="계산" name="b1">
    <input type="reset" value="다시입력" name="b2">
   </form>
 </center>
</body>
</html>
```

##### CalcBean.java

```java
package calc;

public class CalcBean {
    // 멤버변수는 <input type="text" name="num1"와 반드시 일치
    private int num1, num2;
    private String operator = "";// 연산자
    private int result;// 연산결과값을 저장할 변수

    public int getNum1() {
        return num1;
    }

    public void setNum1(int num1) {
        this.num1 = num1;
        System.out.println("setNum1()호출됨!");
    }
    /* Tomcat 8.5기준
     * <jsp:setProperty name="~" property="*"/>->에러유발
    public void setNum1(String num1) {
        this.num1 = Integer.parseInt(num1);
        System.out.println("setNum1()호출됨!");
    }
    */
    public int getNum2() {
        return num2;
    }

    public void setNum2(int num2) {
        this.num2 = num2;
        System.out.println("setNum2()호출됨!");
    }

    public int getResult() {
        return result;
    }
    //5+3=8
    public  void  calculate() {
        //+
        if(operator.equals("+")) {
            result=num1+num2;
        }
        //-
        if(operator.equals("-")) {
            result=num1-num2;
        }
        //*
        if(operator.equals("*")) {
            result=num1*num2;
        }
        // /
        if(operator.equals("/")) {
            result=num1/num2;
        }
    }

    public void setOperator(String operator) {
        this.operator = operator;
        System.out.println("setOperator() 호출됨!");
    }
}
```

##### calResult.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="calc.CalcBean"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>요청을 받아서 처리해주는 페이지</title>
</head>
<body>
<%
   //방법1->매개변수 전달받아=>계산 ->출력
   /*
   CalcBean ca=new CalcBean();
   ca.setNum1(Integer.parseInt(request.getParameter("num1")));  //"5"->5
   ca.setOperator(request.getParameter("operator"));//+
   ca.setNum2(Integer.parseInt(request.getParameter("num2"))); //"3"->3
   ca.calculate();*/
   //result->num+num2
%>

<!-- 방법2  -->
<jsp:useBean id="ca" class="calc.CalcBean" scope="page" />
<jsp:setProperty name="ca" property="*"  />
<%  ca.calculate();  %>
<hr>
계산결과:<%=ca.getResult() %><br>
계산결과2:<jsp:getProperty name="ca" property="result" />
</body>
</html>

```

---

```
<form name="form1" method="post">
<input type="submit" value="계산" name="b1">
            ==========
           type="button"주면 안된다.(주의할점)
```

---

```
 JspWork3->프로젝트-->쿠키와 세션에 대해 정리
        |
         -cookietest->makeCookie.jsp->useCookie.jsp
        |
         -sessiontest->준비된 예제

Windows7의 쿠키 위치

모뎀으로 인터넷으로 접속->인터넷속도가 너무 느린관계->쿠키를 이용
                                   ->접속속도를 향상시킬 목적

쿠키파일->특정 사이트에 접속->접속한 컴퓨터(서버)->자기 컴퓨터의 정보를
                담은 파일을 재전송->

==>해킹소지(개인정보 보호)->잘 사용하지 않는다.
```

---

#### 쿠키 들어가 있는 장소

-	C:\Users\kitcoop\AppData\Roaming\Microsoft\Windows\Cookies ===== 계정명

---

#### 쿠키과 세션

#### 쿠키의 생성

```java
  Cookie c = new Cookie("cookie_name", "cookie_value");
  c.setMaxAge(60*2);
  c.setValue("Mellon");
  response.addCookie(c);
```

#### 쿠키 가져오기

```java
<%
  Cookie[] cookies = request.getCookies();

  if(cookies != null) {
    for(int i=0; i<cookies.length; i++) {
      if(cookies[i].getName().equals("mycookie")) { %>
          cookieName : <%=cookies.getName()%>
          cookieValue : <%=cookies.getValue()%>
    <%  }

    }
  }
%>
```

---

#### 쿠키와 세션의 공통점 ★★★

-	클라이언트와 서버의 연결을 일정시간동안 유지시켜주는 방법(로그인)

#### 쿠키와 세션의 차이점

| 구분          | 쿠키                                           | 세션                                                                                                     |
|---------------|------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| 저장위치      | 파일로저장                                     | 서버의 메모리에저장                                                                                      |
| 특징          | 해킹의 소지<br>개인정보의 유출 가능성이 있다.  | 30분(default)유지<br>30분후 자동종료                                                                     |
| 저장방법      | ㅇㅇ                                           | 세션의 메모리에 저장(HashMap)<br>session.setAttribute(키,저장할값(계정id))<br>session.getAttribute(키명) |
| 저장되는 형식 | 텍스트                                         | Object                                                                                                   |
| 리소스        | 클라이언트의 리소스 사용                       | 서버의 리소스 사용                                                                                       |
| 용량제한      | 한 도메인당 20개<br>쿠키하나당 4kb<br>총 300개 | 서버가 허용하는 용량                                                                                     |

##### 쿠키 ★★★

-	쿠키 : 클라이언트에 파일로 저장되어 해킹의 소지와 개인정보유출 가능성이 있어서 잘 사용하지 않는다.

##### 세션 ★★★

-	세션 : 서버의 메모리에 저장되고 기본값으로(default) 30분 유지후에 자동 종료된다. - 세션의 서버 메모리에 저장(HashMap) -> key, value - 저장 형식

```java
session.setAttribute("키", "저장할 값(계정id)");
<->
session.getAttribute("키명");
```

##### 예시

```java
session.setAttribute("idKey","idValue");
session.setMaxInactiveInterval(60); //60초-> 세션유지시간을 설정

String id = (String)session.getAttribute("idKey");
//session.getAttribute()로 값을 얻어 올 경우 Object이므로 String으로 객체형변환을 해주어야한다
// Object 부모 String 자식

String sessionid = session.getId(); //아이디 값 얻어오기
int inteval = session.getMaxInactiveInterval(); // 유지시간

session.invalidate(); //세션 연결 해제

```

---

#### Java application의 DB연동방법(JDBC) ★★★★

1.	접속

2.	C:\jdk1.8\jre\lib\ext 안에 ojdbc6.jar이라는 파일을 복사할 것(전역) <br> => 추가적인 작업 안하여도 된다. <br> or <br> 다른 경로에 ojdbc6.jar를 복사 -> classpath환경변수를 선언하여 그 경로를 지정 (지역)

3.	Web Application의 DB연동방법 =>

4.	1) Tomcat8.5.x\lib\ 안에 접속드라이버(ojdbc6.jar) 를 복사 (전역)

5.	2)

```
  JspMember
       |
       -WebContent
               |
               -Web-INF
                   |
                    - lib -> ojdbc6.jar (지역)
                              my~.jar  
     ->이클립스에서 경로를 지정 ojdbc6.jar를 불러오기
```

---

#### oratest

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.sql.*" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h3>JDBC_Oracle 접속방법</h3>
    <%
        String url="jdbc:oracle:thin:@localhost:1521:orcl";
        Connection con = null;
        Statement stmt = null;
        PreparedStatement pstmt=null;
        Statement stmt2 = null;
        ResultSet rs=null; // select

        String sql = null;
        String sql2 = null;
        try{
            //1. 드라이버 메모리에 로드
            Class.forName("oracle.jdbc.driver.OracleDriver");
            //2.Connection객체를 얻어오기
            con=DriverManager.getConnection(url,"scott","tiger");//
            //System.out.println("con=>"+con);

            //3.테이블을 생성->create table->stmt
            stmt=con.createStatement(); //반환형
            sql="create table MyTest(name varchar(20), age number)";
            stmt.executeUpdate(sql);
            System.out.println("MyTest 테이블 생성 OK");

            //2.insert->pstmt
            pstmt = con.prepareStatement("insert into MyTest values(?,?)");
            //setString, setInt, setDouble,,,(?의 순서, 입력할 값)
            pstmt.setString(1,"Lee"); //request.getParameter("name");
            pstmt.setInt(2,34);
            pstmt.execute(); // executeUpdate()

            //3.select->필드별로 출력하자
            stmt2 = con.createStatement();
            sql2 = "select * from MyTest";
            rs = stmt2.executeQuery(sql2);
%>
    <table border="1" cellspacing="0" cellpadding="0">
        <tr bgcolor="pink">
            <th>name</th>
            <th>age</th>
        </tr>
        <% while(rs.next()) { //이동시킬 레코드가 존재한다면
        %>
        <tr>
            <td><%=rs.getString(1)%></td>
            <td><%=rs.getInt(2)%></td>
        </tr>
        <%
        }

        rs.close(); //con->stmt, pstmt, stmt2, rs
        stmt2.close();
        pstmt.close();
        stmt.close();
        con.close();
        %>
        </table>
        <%  

        }catch(Exception e){
            System.out.println("DB연결 실패=>"+e);

        }
    %>
</body>
</html>
```

---

### 자바빈즈 3가지 종류

1.	DB Connection빈 => DB연결관리(연결 및 해제)
	-	DBConnectionMgr.java(350Line)

```
  1). 이부분만 변경..
  private String _driver = "oracle.jdbc.driver.OracleDriver",
  _url = "jdbc:oracle:thin:@localhost:1521:orcl",
  _user = "scott",
  _password = "tiger";


  2). DBConnectionMgr클래스의 객체를 생성하여
  getConnection() 필요 -> Connection -> (정젝메서드로 구현)

  3) freeConnection()을 이용해서 ->DB메모리 해제구문

public void freeConnection(Connection c, PreparedStatement p, ResultSet r) {
        try {
            if (r != null) r.close();
            if (p != null) p.close();
            freeConnection(c);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

1.	SQL구문을 실행 -> 자바빈즈의 메서드 선언 => 메서드를 호출

	-	DAO(Data Access Object) =>DB접속 후 메서드 선언

2.	DTO(데이터 저장빈) -> Setter, Getter메서드 구성(테이블의 필드와 연관)

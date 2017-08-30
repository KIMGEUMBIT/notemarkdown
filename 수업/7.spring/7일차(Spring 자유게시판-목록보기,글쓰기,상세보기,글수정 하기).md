7일차(Spring 자유게시판-목록보기,글쓰기,상세보기,글수정 하기)
-------------------------------------------------------------

```
*** JNDI(Java Naming Directory Interface)->DB연결의 정보->Context.xml저장
 ->DB정보이름(default jdbc/orcl,,,) 등록
 =>DAO=>url,driver,계정,암호X
 JNDI이름을 검색(연결객체가져옴)
```

##### board-servlet.xml

-	생성 시 자동으로 DB연결할 수 있는 BoardDAO 객체 필요

```xml
<bean id="boardDAO" class="lee.BoardDAO" />
```

-	list.do인 경우 해당 lee.ListActionController

```xml
<!-- 1. 글목록보기 -->
<bean name="/list.do" class="lee.ListActionController" >
    <property name="dao">
        <ref bean="boardDAO" />
    </property>
</bean>
```

##### ListActionController은 Controller인터페이스 상속받아 생성

-	Controller인터페이스 상속 받는 이유? <br> ->요청을 받아서 처리해주는 역할

##### ListActionController

```java
public class ListActionController implements Controller {

    BoardDAO dao;

    public void setDao(BoardDAO dao) {
        this.dao = dao;
        System.out.println("setDao()호출됨(dao)=>"+dao);
    }

    @Override
    public ModelAndView handleRequest(HttpServletRequest arg0, HttpServletResponse arg1) throws Exception {
      System.out.println("ListActionController 실행됨!");
  		ArrayList list = dao.list();
  		ModelAndView mav = new ModelAndView();
  		mav.setViewName("list");

  		//request.setAttribute("list", list)
  		mav.addObject("list",list);

  		return mav;
    }
```

> BoardDAO dao;은 전역으로 생성, dao의 생성사 함수 호출로 (DB자동 연결)

##### list.jsp

-	<%@page import="java.util.*,lee.*" %> 선언

```java
<%
    ArrayList list=(ArrayList)request.getAttribute("list");
   if(list!=null){ //데이터가 존재한다면
       Iterator iter=list.iterator();
       while(iter.hasNext()){//꺼낼 데이터가 존재하는한
           Board data=(Board)iter.next();
           int num=data.getNum();
           String title=data.getTitle();
           String author=data.getAuthor();
           String content=data.getContent();
           String writeday=data.getDate();
           int readcnt=data.getReadcnt();
%>
    <tr>
        <td align="center"><%= num %></td>
        <td><a href="retrieve.do?num=<%= num %>"><%= title %></a></td>
        <td><%= author %></td>
        <td><%= writeday.substring(0,10)%></td>
        <td><%= readcnt%></td>
    </tr>
<%
    }//end while
}//end if
%>
```

##### index.jsp

```java
<%
  response.sendRedirect("http://localhost:8090/SpringBoard/list.do");
%>
```

### => index.jsp 실행 시 list.do 에서 목록 보여지기까지

---

### 글쓰기용

#### 위에서 했던 액션컨트롤러가 필요한 경우 board-servlet.xml 작성

```xml
<!-- 1. 글목록보기 -->
<bean name="/list.do" class="lee.ListActionController" >
    <property name="dao">
        <ref bean="boardDAO" />
    </property>
</bean>
```

#### 위에서 했던 액션컨트롤러가 필요없는 경우 board-servlet.xml 작성

```xml

 <!-- 글쓰기용 -->
   <bean name="/writeui.do"
           class="org.springframework.web.servlet.mvc.ParameterizableViewController">
          <property name="viewName" value="write"></property>
  </bean>
```

---

### 스프링에서 컨트롤러 역할

1.	Controller
2.	AbstractCommandController <br> ->사용자로부터 값을 입력(로그인폼, 글쓰기폼, 글수정폼,,,)

---

### 글쓰기

#### 1. board-servlet.xml

```xml
<bean name="/write.do" class="lee.WriteActionController" >
	<property name="dao">
		<ref bean="boardDAO" />
	</property>
	<property name="commandClass" value="lee.BoardCommand"></property>
</bean>
```

#### 2. BoardCommand.java 생성

-	사용자로부터 순수 입력받을 값만 처리하기위해 생성

```java
package lee;

//사용자로부터 순수 입력받은 값만 처리
public class BoardCommand {
	String athor, title ,content;

	public void setAthor(String athor) {
		this.athor = athor;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public void setContent(String content) {
		this.content = content;
	}
}
```

#### 3. BoardDAO.java 글쓰기 구현

-	글쓰기 번호 얻기

-	최대값+1

```java
public int getNewNum(){ //글쓰기 번호 얻기 (최대값+1)
  int newNum = 1;
  try {
    String sql = "select max(num) from springboard";
    Connection con = ds.getConnection();
    PreparedStatement stmt=con.prepareStatement(sql);
    ResultSet rs = stmt.executeQuery(sql);

    if(rs.next()) {
      newNum = rs.getInt(1)+1;
    }
  }catch(Exception e) {
    e.printStackTrace();
  }

  return newNum;

}//end getNewNum();

```

```java
//publicvoid write(BoardCommand board)
public void write(String author, String title , String content){
  try{
    int newNum = getNewNum();
    String sql ="insert into springboard(num,author,title,content) values(";
    sql +=  newNum + ",'" + author + "','" + title + "','" + content + "')";
    System.out.println(sql);

        Connection con = ds.getConnection();
        PreparedStatement stmt = con.prepareStatement(sql);
        stmt.execute(sql);
        stmt.close(); con.close();
    }catch(Exception e ) {e.printStackTrace();}

}//end write
```

#### 4. WriteActionController.java 생성

-	AbstractCommandController 클래스를 상속받는다

| 객체                         | 의미                                                  |
|------------------------------|-------------------------------------------------------|
| HttpServletRequest request   | request(요청객체)                                     |
| HttpServletResponse response | response(응답)                                        |
| Object command               | 입력받은 값을 저장하는 객체                           |
| BindException error          | 사용자로부터 값을 입력 시 에러 발생-> 처리해주는 객체 |

```java
public class WriteActionController extends AbstractCommandController {

	BoardDAO dao;

```

> Command 클래스는 사용자로부터 입력받은 값을 처리해주기 위해 사용

##### ModelAndView mav = new ModelAndView();

```java
//사용자로부터 값을 주로 입력을 받아서 처리해주는 컨트롤러
public class WriteActionController extends AbstractCommandController {

    BoardDAO dao;

    public void setDao(BoardDAO dao) {
        this.dao = dao;
        System.out.println("setDao()호출됨(dao)=>"+dao);
    }

    //1. request(요청객체) 2. response(응답) 3. 입력받은 값을 저장하는 객체
    //4. BindException -> 사용자로부터 값을 입력 시 에러 발생-> 처리해주는 객체
    @Override
    protected ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object command, BindException error)
            throws Exception {
        // TODO Auto-generated method stub

        request.setCharacterEncoding("utf-8");
        BoardCommand data = (BoardCommand)command;
        String author=data.getAthor();
        String content=data.getContent();
        String title=data.getTitle();

        dao.write(author, title, content);

        //response.sendRedirect("list.jsp");

        /*
        ModelAndView mav = new ModelAndView("redirect:/list.do");
        return mav;*/

        return new ModelAndView("redirect:/list.do");
    }

}

```

---

### 글 상세 보기

##### retrieve.jsp

##### 순서

1.	요청명령어 등록-> board-servlet.xml (RetrieveActionController)

2.	BoardDAO -> retrieve() 작성-> 레코드 한개

3.	RetrieveActionController 에서 retrieve()호출

4.	retrieve.jsp에서 Board를 출력

##### 1. 요청명령어 등록

```xml
<!-- 글 상세보기(글 입력X) -->

<bean name="/retrieve.do" class="lee.RetrieveActionController" >
 <property name="dao">
   <ref bean="boardDAO" />
 </property>
</bean>
```

##### 2.

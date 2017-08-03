```java
<%
request.setCharacterEncoding("UTF-8");

String str= request.getParameter("str");
String addr= request.getParameter("addr");
System.out.println("str=>"+str+"addr=>"+addr);

BeanTest bt = new BeanTest();

bt.setStr(str);
bt.setAddr(addr);

out.println("입력받은 이름은?"+bt.getStr()+"<br>");
out.println("입력받은 주소는?"+bt.getAddr()+"<br>");
%>
```

```java

@WebServlet("/test/GetData")
public class GetData extends HttpServlet {

    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        response.setContentType("text/html; charset=UTF-8");
        PrintWriter out = response.getWriter(); //클라이언트로 전송하기 위해서

        out.println("<html><head></head>");
        out.println("<body>");
        out.println("<h2>Hello Sevlet</h2>");

        //---------------------------------------------------//

        request.setCharacterEncoding("UTF-8");
        String name=request.getParameter("name");
        String addr = request.getParameter("addr");
        out.println("이름=>"+name);
        out.println("주소=>"+addr);

        //-------------------------------------------------//

        out.println("</body>");
        out.println("</html>");


    }

}


```

```java

<%
request.setAttribute("total", su);
%>

<%=request.getAttribute("total")%>
```

---

#### response응답객체

```java
response.setContentType("text/html; UTF-8");
response.sendRedirect("http://www.naver.com");
response.sendRedirect("./ggof");
```

---

#### 쿠키의 생성

```java
  Cookie c = new Cookie("cookie_name", "cookie_value");
  c.setMaxAge(60*2);
  c.setValue("Mellon");
  response.addCookie(c);
```

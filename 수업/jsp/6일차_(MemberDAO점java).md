### 홈페이지 제작을 위한 자바빈즈 3가지를 만들어 주어야한다.

#### 자바빈즈 3가지 ★★★

1.	DB연결빈

> DBConnectionMgr.java (url, account, pw)

1.	DTO

> (Data Transfer Object : 계층간의 데이터 교환을 위한 자바빈즈)
>
> 일반적인 DTO는 로직을 갖고 있지 않다
>
> 속성과 속성에 접근하기 위한 getter, setter메소드만 가진
>
> 테이블(20~) 설계 -> 필드별로 저장, 조회한다 테이블의 갯수와 DTO의 클래스 갯수는 같다

1.	DAO

> (Data Access Object : DB의 data에 access 하는 트랜잭션 객체)
>
> 테이블관리 -> insert, update, delete, select -> join~

---

```java
import java.sql.*;// DB
import java.util.*; //Vector, ArrayList
```

---

### DAO부분을 코딩하기 위해서는<br> 즉, DB에 접근하여 DML문을 실행하기 위한 <br>즉, sql문을 실행하기 위한 객체인 <br>Statement, PreparedStatement

#### Resultset :

-	Statement와 PreparedStatement 은sql실행 객체로써 이를 반환하기 위해서는 Resultset이 필요하다.

#### Statement와 PreparedStatement의 차이점

-	PreparedStatement는 미리 메모리에 올려놓아진 상태에서 where의 ?로 된 특정 부분만 수정을 하여 실행하므로 과부화가 걸리고 **메모리에 부담**을 주지만
-	**실행속도가 빠르기** 때문에 자주 사용한다
-	PreparedStatement를 언제 사용을 하느냐? <br> -> select * from emp where name = ? 에서의 ?부분만 수정이 된다면 이 preparedStatemt를 사용한다

#### Statement 사용법

```java

Statement stmt = null;

/*생략*/

sql = "select * from memeber where name='김금빛'";
stmt = con.createStatement(); // DB와 연결
rs = stmt.executeQuery(sql); //메모리에 올려준다
                             //true or false 반환

Boolean check = rs.next();
```

#### PreparedStatement 사용법

```java
PreparedStatement pstmt = null;

/*생략*/

sql = "select * from member where id=? and name=?";
pstmt = con.prepareStatement(sql); //미리 메모리에 sql구문을 올려준다
pstmt.setString(1, id);
pstmt.setString(2, name);
rs = pstmt.executeQurey();

Boolean check = rs.next();
```

---

### Vector

-	**연속적인 메모리 구조**를 가지고 있는 컬렉션

-	Vector 클래스는 Collection 인터페이스를 기반으로 구현한 **List 클래스**에서 파생한 클래스이다.

-	따라서 Vector 클래스에는 Collection 인터페이스에 약속한 기능들을 사용할 수 있다.

-	물론 Vector 클래스에서 추가적으로 제공하는 기능들도 있다.

-	Vectory<E>는 List<E>인터페이스를 구현한 클래스로서 **가변개수의 배열**이 필요시 적합

-	**요소에 값이 증가되면 자동으로 크기 조절**

-	요소의 값이 **중간이 삽입 가능**하고, 그 다음 요소들은 한 자리씩 뒤로 이동된다.

-	http://farmerkyh.tistory.com/842 참조

---

```java
package hewon;

//웹상의 호출되는 메서드를 선언하는 클래스
//DBConnectionMgr <---> MemberDAO 서로 연결(has a 관계)

import java.sql.*;// DB
import java.util.*; //Vector, ArrayList

public class MemberDAO {

    // 1.DBConnectionMgr 객체를 선언
    private DBConnectionMgr pool = null;

    // 2.생성자를 통해서 객체를 얻어온다 -> 서비스
    public MemberDAO() {
        try {
            pool = DBConnectionMgr.getInstance(); // getInstance() 인스턴스 얻어온다는 의미
            System.out.println("pool=>" + pool);
        } catch (Exception e) {
            System.out.println("DB연결 실패" + e);
        }
    }// 생성자

    // 1) 요구분석에 따른 회원로그인을 체크인해주는 메서드 필요
    // int, boolean(true, false)
    // select id, passwd from emeber where id='nup' and passwd='1234'

    public boolean loginCheck(String id, String passwd) {
        // 1. DB연결코딩

        Connection con = null;
        PreparedStatement pstmt = null;
        //미리 메모리에 띄어놓는 sql구문을 실행 시킬 수 있는 객체로써
        //sql문에 변수가 ?로 사용 가능
        //sql = select * from emp where name=?

        ResultSet rs = null;

        boolean check = false;

        // 2. 실행시킬 SQL구문이 필요
        String sql = "";

        try {

            /*
             * //1. 드라이버 메모리에 로드 Class.forName("oracle.jdbc.driver.OracleDriver");
             * //2.Connection객체를 얻어오기
             * con=DriverManager.getConnection(url,"scott","tiger");//
             *
             */

            con = pool.getConnection(); // getConnection()은 DBConnectionMgr에 선언되있음
            System.out.println("con=>" + con);
            sql = "select id, passwd from member where id=? and passwd=?";
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, id);
            pstmt.setString(2, passwd);
            rs = pstmt.executeQuery(); //실행

            check = rs.next(); // 데이터가 존재 ->true, 업으면 false

        } catch (Exception e) {
            System.out.println("loginCheck 실패" + e);
        } finally {// 3. DB연결 해제
            pool.freeConnection(con, pstmt, rs);
        }

        return check;

    }

    // 2) 회원가입-> 중복 id를 체크인 해주는 메서드 필요

    public boolean checkId(String id) {
        // 1. DB연결코딩

        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        boolean check = false;

        // 2. 실행시킬 SQL구문이 필요
        String sql = "";

        try {
            con = pool.getConnection(); // getConnection()은 DBConnectionMgr에 선언되있음
            System.out.println("con=>" + con);
            sql = "select id from member where id=?";
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, id);
            rs = pstmt.executeQuery();

            check = rs.next(); // 데이터가 존재 ->true, 업으면 false

        } catch (Exception e) {
            System.out.println("checkId 실패" + e);
        } finally {// 3. DB연결 해제
            pool.freeConnection(con, pstmt, rs);
        }

        return check;
    }


    // 3) 우편번호-> 우편번호를 검색 -> 자동으로 입력
    // select * from zipcode where area3 like '%수유3동%'; -> String

    //Vector, ArrayList
    public Vector zipcodeRead(String area3) {

        Connection con = null;
        PreparedStatement pstmt = null; //sql 실행 객체
        ResultSet rs = null; //sql 명령에 대한 반환값

        //추가
        Vector vecList = new Vector(); // 연속적인 메모리 구조를 가지고 있는

        // 2. 실행시킬 SQL구문이 필요
        String sql = "";

        try {
            con = pool.getConnection(); // getConnection()은 DBConnectionMgr에 선언되있음
            System.out.println("con=>" + con);
            sql = "select * from zipcode where area3 like '"+area3+"%'";

            pstmt = con.prepareStatement(sql);

            rs = pstmt.executeQuery();

            while(rs.next()) {
                //vector or ZipcodeDTO
                ZipcodeDTO tempZipcode = new ZipcodeDTO();
                //ZipcodeDTO 라는 클래스를 인스턴스화 시켜서 ZipcodeDTO라는 인스턴스 데이터타입의 tempZipcode라는 변수에 담아주었다
                tempZipcode.setZipcode(rs.getString("zipcode")); //우편번호
                //rs.getString("zipcode") 위 실행 sql구문의 zipcode라는 변수의 값을 가져와서
                //tempZipcode.setZipcode(String zipcode) setter 메소드를 실행
                tempZipcode.setArea1(rs.getString("area1")); // 시
                tempZipcode.setArea2(rs.getString("area2")); //도, 구
                tempZipcode.setArea3(rs.getString("area3"));
                tempZipcode.setArea1(rs.getString("area4"));

                //vector or ArrayList에 담는 구문
                vecList.add(tempZipcode);
            }



        } catch (Exception e) {
            System.out.println("zipcodeRead() 실행에러 유발" + e);
        } finally {// 3. DB연결 해제
            pool.freeConnection(con, pstmt, rs);
        }

        return vecList;
    }

    // 4) 회원가입

    // 5) 회원수정

    // 6) 회원탈퇴

}

```

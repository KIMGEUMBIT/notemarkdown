4일차(Spring JDBC Programming 작성2,어노테이션 개요및 작성법)
-------------------------------------------------------------

#### JdbcTemplate 클래스를 이용한 JDBC프로그래밍

-	insert,update,delete => **update()**로 통합
-	조회하여 쿼리를 구하는 행의 개수가 **한개**라면 : **queryForObject()** <br> **여러개** 라면 : **queryForList()**

#### Mapper인터페이스

-	mapRow()을 이용

#### RowMapper

-	조회결과 ResultSet으로부터 데이터를 읽어와 객체를 생성해주는 매퍼
-	검색하고자 할 경우 RowMapper를 상속받아야 한다.

##### StuudentMapper.java

```java
import java.sql.ResultSet;
import java.sql.SQLException;

import org.springframework.jdbc.core.RowMapper;

public class StuudentMapper implements RowMapper<Student> {
    @Override
    public Student mapRow()(ResultSet rs, int rowNum) throws SQLException {
      //while(rs.next()){

  		Student st = new Student();//DTO객체 생성
  		st.setId(rs.getInt("id"));
  		st.setName(rs.getString("name"));
  		st.setAge(rs.getInt("age"));

  		return st; //qurey 반환
    }
}

```

-	mapRow()는 callBack메서드로써 내부적으로 자동호출되는 메서드이다.

-	mapRow()의 인자 ResultSet rs : 객체반환해준다(테이블의 정보)<br> int rowNum : 검색된 갯수

-	implements RowMapper **\<Student>** : DB 조회 결과는 Student 클래서에서 받을 것이므로 Student값만 받을 수 있도록 설정해 놓았다

---

##### StudentDAO.java

```java
package studentdb;

import javax.sql.DataSource;
import java.util.List; //여러개의 레코드 필요

public interface StudentDAO {

    //1. DB연결을 시켜주는 메서드 작성(초기화)->DataSource객체->DB연동
    public void setDataSource(DataSource ds);

    //2.insert
    public void create(String name, Integer age); //학생명,나이

    //3.학생정보->id값-> select * from where id=1;
    public Student getStudent(Integer id); //(int id)

    //4. 학생들 정보->select * from student
    public List<Student> listStudent();

    //5. 학생정보를 삭제-> delete from student where id = 2;
    public void delete(Integer id);

    //6. 학생정보를 수정 -> update 테이블명 set 필드명=값 ,,, where
    public void update(Integer id, Integer age);

}
```

### StudentJDBCTemplate.java에서 DB 연결 방법

##### StudentJDBCTemplate.java

-	StudentJDBCTemplate implements StudentDAO //상속받았다
-	private DataSource dataSource; //DB접속
-	private **JdbcTemplate jdbcTemplateObject**; <br> insert,update, delete를 하기위한 PreparedStatement pstmt와 역할이 비슷한 클래스

#### Bean.xml

```xml
<!--1.DB연결(dataSource)  -->
<bean id="dataSource"
          class="org.springframework.jdbc.datasource.DriverManagerDataSource">
  <property name="driverClassName"  value="com.mysql.jdbc.Driver" />
  <property name="url"  value="jdbc:mysql://localhost:3306/mydb" />
  <property name="username"  value="root" />
  <property name="password"  value="1234" />
</bean>

<!--2.DB연결시켜서 가져올 수 있는 빈즈객체  -->
<bean id="studentJDBCTemplate" class="studentdb.StudentJDBCTemplate">
    <property name="dataSource" ref="dataSource" />
</bean>
```

-	dataSource 객체를 연결해준다.

##### StudentJDBCTemplate.java

##### 1. DB접속

```java
public void setDataSource(DataSource dataSource) {
  // TODO Auto-generated method stub
  this.dataSource = dataSource;
  this.jdbcTemplateObject = new JdbcTemplate(dataSource); //생성자
  System.out.println("setDataSource()호출돼서 DB연결됨!");
}
```

##### 2. insert 실행

-	형식) jdbcTemplate객체명.update(실행시킬 sql구문, 입력받은 값을 매개변수)

```java
@Override
public void create(String name, Integer age) {
  // TODO Auto-generated method stub
  //형식) jdbcTemplate객체명.update(실행시킬 sql구문, 입력받은 값을 매개변수)
  String sql = "insert into student(name, age) value(?,?)";
  jdbcTemplateObject.update(sql, name, age);
  System.out.println("생성된 레코드이름->"+name+"+age="+age);
}
```

##### 3. 하나 행 출력 (queryForObject)

-	jdbc객체명.queryForObject(실행sql, 매개변수(배열), RowMapper객체지정)

##### StudentMapper.java

```java
public class StudentMapper implements RowMapper<Student> {

    //callBack 메서드
    @Override
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        // TODO Auto-generated method stub
        System.out.println("mapRow() 호출됨(rownum)=>"+rowNum);

        //while(rs.next()){

        Student st = new Student();//DTO객체 생성
        st.setId(rs.getInt("id"));
        st.setName(rs.getString("name"));
        st.setAge(rs.getInt("age"));

        return st; //qurey 반환
    }
}

```

```java
@Override
public Student getStudent(Integer id) {
  String sql = "select * from student where id=?";
  Student st = jdbcTemplateObject.queryForObject(sql, new Object[] {id}, new StudentMapper());
  return st;
}
```

-	StudentMapper.java의 return 된 st와 위 new StrudentMapper와 연결되있다고?????

##### 4.List<Strudent> 값 리턴

```java
@Override
public List<Student> listStudent() {
  // TODO Auto-generated method stub
  String sql = "select * from student where id=?";
  //반환값(List) = jdbc객체명.qurey(실행sql, RowMapper객체지정)

  List<Student> sts = jdbcTemplateObject.query(sql, new StudentMapper());
  return sts;
}
```

##### 삭제

```java
@Override
public void delete(Integer id) {
  // TODO Auto-generated method stub
  String sql = "delete from student where id=?";
  jdbcTemplateObject.update(sql, id);
}
```

##### 수정

```java
@Override
public void update(Integer id, Integer age) {
  // TODO Auto-generated method stub
  String sql = "update student set age=? where id=? ";
  jdbcTemplateObject.update(sql, age, id);
  System.out.println("수정된id=>"+id);
}
```

---

### 어노테이션

-	어노테이션은 @표시 뒤에 어노테이션 이름을 부팅며 클래스, 필드, 메소드 등과 같은 프로그램의 선언부에 적용할 수 있다.

-	데코레이션(장식자) -> import {} for '@angular/core'

-	@키워드 @Component -> 기능

-	클래스 연관 -> import를 어노테이션과 연관이 있는 클래스를 불러온다.

-	목적★★★ <br> 1) 환경설정에 관한 어노테이션 => xml에서 사용<br>2) 자바코딩의 간결화

-	장점★

```
１. 시스템 복잡성이 아니라면 어노테이션 사용은 적합하게 쓰이면 코드가 간결해지고 유지보수가 용이해짐
２. 대형 시스템엔 계층 구조가 잘 파악되기 위해서는 xml 사용이 필수<br>
```

-	단점★

```
어노테이션과 관련된 클래스와 같이 연동->이해하기가 쉽지않다.

1. 어노테이션 은 메타정보가 소스코드에 들어가므로 파악하기 어려움
 (재 컴파일이 필요하다.)
2. 소스코드가 같이 제공되지 않으면 사용에 제약이 따름
 (어노테이션 과 클래스 같이 사용할것)
```

### 어노테이션 사용법

1.어노테이션이 클래스 명 위에 명시한 경우

1.	어노테이션이 멤버변수명 위에

2.	Setter Method외에 일반메서드도 가능 <br> -> 메서드명 위에 어노테이션을 붙일 수가 있다.

### 어노테이션 정리

#### 1. @Required

-	@Required 어노테이션은 **필수 프로퍼티를 명시** 할때 사용된다.
-	해당 메서드를 호출시키지 않을 경우 멤버변수를 이용해서 에러발생

#### 적용 방법

1.	setter메소드에 @Required 지정 및 import 한다

##### Camera.java

```java
package com.spring.anno1;

import org.springframework.beans.factory.annotation.Required;

public class Camera {
    private int number;

    @Required
    public void setNumber(int number) {
        this.number = number;
        System.out.println("setNumber()호출");
    }

    @Override
    public String toString() {
        // TODO Auto-generated method stub
        return "Camera[number="+number+"]";
    }
}
```

1.	어노테이션을 빈즈가 인식 할 수 있도록 xml에 추가

```xml
<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" />
```

#### 2. @Autowired

```
**
xml에서 @Required를 사용하기위해서는 아래와 같은 빈즈클래스를
미리 등록시켜줘야 한다.

<bean class="org.springframework.beans.factory.
                     annotation.RequiredAnnotationBeanPostProcessor">

</bean>

<!-- @Required,@Autowired,@Resource 때문에 필요(xml에 등록) -->
<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" />
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor" />
<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor" />

->다음과 같은 멤버변수와 관련된 에러메세지를 출력시켜준다.

nested exception is org.springframework.beans.factory.BeanInitializationException:
 Property 'number' is required for bean 'camera1'


어노테이션은 한개이상 겹쳐서 나올 수가 있다.(특징)
  ***********************************************                                         
               @Autowired=>특징

                       1.생성자,멤버변수(=속성),메서드에 지정이 가능
                       2.
                        메서드명위에 @Autowired를 부여
                                    이메서드를 호출하면서 매개변수가 의존성객체를
                                    얻어넣어준다.(멤버변수에)

                           xml에 <property>태그 또는 <p:멤버변수-ref="~">
                                    생략해도 호출한다.
                         3.Setter Method외에 다른 메서드에서 동일하게 적용이 된다.
                         4.해당하는 빈즈객체가 없거나 두개이상 존재시 에러가 유발
                         NoSuchBeanDefinitionException: No qualifying bean of type [com.spring.anno2.SmsSender] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
                            ->xml에 빈즈가 등록X
                            =>디폴트 설정=>Autowired(required=true)
                                                    무조건 가져와라는 옵션
                             =>@Autowired(required=false)
                                                      추가옵션

    @Required =>반드시 호출했는지 안했는지를 체크해주는 기능
    public void setSender(SmsSender sender) {
        this.sender = sender;
        System.out.println("setSender()호출됨=>"+sender);
    }
----------------------------------------------------------------

```

```java


	@Required =>반드시 호출했는지 안했는지를 체크해주는 기능
	public void setSender(SmsSender sender) {
		this.sender = sender;
		System.out.println("setSender()호출됨=>"+sender);
	}
```

```xml
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor" />
```

#### 3. @Inject 애노테이션을 이용한 의존 자동 설정

```xml
<!--  @Inject때문에 -->
<dependency>
  <groupId>javax.inject</groupId>
  <artifactId>javax.inject</artifactId>
  <version>1</version>
</dependency>
```

-	Autowired 어노테이션이 required속성을 이용해서 필수여부를 지정할 수 있는 것과 달리 Inject 애너테이션은 반드시 사용할 빈이 존재해야한다

#### 4. @Resource

```xml
<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor" />
```

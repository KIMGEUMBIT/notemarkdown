8일차(Spring게시판-글삭제,글검색하기,MyBatis DB환경설정하기)
------------------------------------------------------------

### MyBatis

-	JDBC Programming(20~30%) 정도 코딩양이 줄어든다 <br> => 개발시간 단축

### MyBatis 변경

#### 1. lib -> Spring 3.x이상으로 변경 <br>(라이브러리 전부 삭제, 강사님 lib 복사 붙)

#### 2. DB연동 : <br> WEB-INF/jdbc

##### sql.properties

```
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://13.124.117.186:3306/project?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=1234
```

##### dataAccessContext-local.xml

-	각각 왜 설정했나 정의 ★

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN 2.0//EN"
 "http://www.springframework.org/dtd/spring-beans-2.0.dtd" >

<!-- 1./WEB-INF/m.properties -->

<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" >
    <property name="locations">
        <list>
            <value>WEB-INF/sql.properties</value>
        </list>
    </property>
</bean>

<!-- 2.sql.properties파일에서 각각의 키를 분리 메모리에 올림-->

<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driverClassName}" />
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>

<!-- 3. (Mybatis) bean등록 (SqlSessionFactoryBean)

        1)configLocation -> 태이블에 대한 xml 파일을 불러올 때
        2)dataSource -> DB연결정보 멤버변수

    ===> DB접속해서 Mybatis 연결하여  DB 연결해주는 소스
-->

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" >
    <property name="configLocation" value="WEB-INF/SqlMapConfig.xml"></property>
    <property name="dataSource" ref="dataSource" ></property>
</bean>

<!-- 4 SqlSessionTemplate(sqlSession 객체를 더 쉽게 얻어오기 위해서 설정)-->

<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>

</beans>


```

#### 3. web.xml

---

##### lee.BoardCommand의 글자를 board로 줄이는 SqlMapConfig.xml

```xml
<typeAliases>
  <typeAlias type = "lee.BoardCommand" alias="board" />
</typeAliases>
```

---

##### BoardCommand.java

-	변수 지정 Setter, Getter 만들기

	Board.xml
	=========

-	sql문장 선언

１. **id** (필수) : <br> sql문장에 종류와 상관없이 필수로 작성(구분인자값)

２. **parameterType** (선택) : <br> SQL구문중에서 매개변수를 입력받아서 처리해주는 경우의 SQL구문이 필요 <br> -> where 조건식<BR> **입력을 받는 자료형** 을 쓰게 되어있다<BR>

```
parameterType="java.lang.String" or "String"
             ="java.lang.Integer" or "int"

select * from springboard where num = #{매개변수명} <- ?대신이다
insert into springboard values(#{매개변수},,,)
```

３. **resultType** (선택) : <br> sql구분을 사용해서 반환값이 있는 경우 <br>select num from springboard<br>(insert,update,delete X)<br>resultType="java.lang.Integer" or "int"

#### 게시판에서 공통으로 사용할 기능인 추상메서드 선언

-	BoardDAO.java
-	import java.util.*; // List사용하기 위해
-	import org.springframework.dao.DataAccessException; //스프링 예외처리 클래스

#### 8. SqlMapBoardDao클래스 작성

-	BoardDAO인터페이스, SqlSessionDaoSupport클래스를 상속
-	형식)

```
//레코드 한개
//형식) sqlSession객체명.selectOne(실행시킬 sql구분의 id값, 매개변수명)

//레코드가 여러개인경우
//형식) sqlSEssion객체명.selectList(실행시킬 sql구문의 id값, 매개변수명)
```

```java
public List list() throws DataAccessException {
  return getSqlSession().selectList("list");
}
```

##### 9. board-servlet.xml에서 전에설정된 dataAccessContext-local.xml에서 설정했던 sqlSessionFactory ★★★★★ㅁ88ㅁ88ㅁ88

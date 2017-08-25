### 5일차(어노테이션 마무리 및 SpringMVC 프로그래밍 개요및 작성법)

-	has a => 내가 가지고 싶은 객체를 멤버변수로 해주어라

#### 의미

```xml
<!--빈즈등록(빈즈객체생성 X)  -->
<bean id="camera1" class="com.spring.anno1.Camera">
    <property name="number" value="12" />  
</bean>
```

> Camera클래스의 setter메서드 number에 값을 12를 넣어라

#### P네임스페이스 사용법

1.	xml의 Namespaces탭에 들어가서 <br> -> xmlns:p="http://www.springframework.org/schema/p" 자동 추가 뒤
2.	xml에 p:number="2" 라고 추가해준다

```xml
<bean id="camera2" class="com.spring.anno3.Camera" p:number="2"/>
```

---

#### 1. 같은 자료형을 가진 객체들중에 선택하여 가져오는 방법 @Resource

#### Camera.java

```java
public class Camera {

    private int number;//카메라 수

    @Required //호출 시 필수 값 설정하여 에러 발생하게 해준다
    public void setNumber(int number) {
        this.number = number;
        System.out.println("setNumber()호출");
    }
```

```xml
<!-- @Resource -->
<bean id="camera2" class="com.spring.anno3.Camera" p:number="2"/>
<bean id="camera3" class="com.spring.anno3.Camera" p:number="3"/>
<bean id="camera4" class="com.spring.anno3.Camera" p:number="4"/>

<bean id="homeController" class="com.spring.anno3.HomeController" />
```

##### HomeController.java

1.	@Resource(name="자바빈즈에서 지정한 id값") 선택 멤버변수 위에 지정
2.	@Resource에 대한 import 시켜준다 import javax.annotation.Resource;

```java
package com.spring.anno3;

import javax.annotation.Resource;
//같은 클래스자료형이 여러개 있을 때 어떻게 구분해서 각각의 객체를 가져오기
public class HomeController {

		@Resource(name="camera2")
		private Camera camera2; //new Camera()로 객체를 생성하는 방법이 아닌 getBean을 통하여 멤버변수에 접근해서 객체 생성

		private Camera camera3;
		private Camera camera4;

		/*public void setCamera2(Camera camera2) {
			this.camera2 = camera2;
		}*/

		@Resource(name="camera3")
		public void setCamera3(Camera camera3) {
			this.camera3 = camera3;
		}

		@Resource(name="camera4")
		public void setCamera4(Camera camera4) {
			this.camera4 = camera4;
		}
}

```

> **@Resource(name="camera2")**은 **setXXX메서드 생략**이 가능하다. <br> 즉, 자동으로 연결할 의존객체를 얻어올 때 사용<br><br> camera3, camera4는 setter메서드 위에 선언하였으므로 settter메서드 호출한다

##### Main.java

```java
HomeController homeController =context.getBean("homeController",HomeController.class);
System.out.println("homeController=>"+homeController);
```

---

### 2. @PostConstruct, @PreDestroy

-	자바코드로써 빈즈객체를 생성하기 전, 후에 대한 메서드를 호출 처리
-	@PostConstruct : 빈즈객체 생성 전 -> Camera<br> before()
-	@PreDestroy : 빈즈 객체 생성 후<br> after()

-	@PostConstruct와 @PreDestroy와 같은 기능을 가진 xml로 설정방법<br>\<bean id="camera1" class="com.spring.Camera" init-method="before" destroy-method="after" />

#### 예시

##### app.xml

```xml
<bean id="homeController2" class="com.spring.anno4.HomeController2" />
<bean id="camera5" class="com.spring.anno4.Camera" />
```

##### HomeController2.java

```java
package com.spring.anno4;

import javax.annotation.*;

// 빈즈객체를 가져올때 호출되는 메서드(SetCamera())
//@
public class HomeController2 {

	private Camera camera;// new Camera();

	@Resource(name = "camera5")
	public void setCamera(Camera camera) {
		this.camera = camera;
		System.out.println("camera5이름을 가진 setCamera()호출됨");
	}

	@PostConstruct
	public void init() {
		System.out.println("빈즈객체 생성전에 초기화작업(init)호출됨");
	}

	@PreDestroy
	public void close() {
		System.out.println("빈즈객체 생성후에 메모리해제(close)호출됨");
	}

}

```

##### Camera.java

```java
public class Camera {}
```

---

### @Component

-	@Component어노테이션 클래스명 위에 기재
-	특정패키지에 지정해주면 스프링 컨테이너가 생성

１. app2Scan.xml 만들기

![123](/assets/123.GIF)

-	**<context:component-scan base-package="com.spring.anno5" />**
-	의미 : **com.spring.anno5패키지**를 찾아서 **클래스**들 중에서 <br> @Component가 부여된 **클래스의 객체를 빈즈**로 **자동등록**하라. <br> 뿐만아니라 @Autowired, @Required, @Resource 도 자동으로 빈즈(빈객체)를 등록해주어라  

```xml
<context:component-scan base-package="com.spring.anno5" />
<!--<bean id="~" class="com.spring.anno5.Camera~ " />
<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" />
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor" />
<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor" />
-->

```

２. HomeController2.java

-	HomeController2클래스위에 @Component
-	@Component("아이디명") 아이디명 등록 안할 경우 클래스명으로 따라간다(앞글자는 소문자로)

```java
package com.spring.anno5;
// 빈즈객체를 가져올때 호출되는 메서드(SetCamera())

@Component //이 클래스 객체 빈즈를 등록 해진다
public class HomeController2 {

	@Autowired
	private Camera camera;// new Camera();

	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "Homecontroller2[camera="+camera+"]";
	}
}
```

３. Camera.java

-	Camera 클래스위에 @Component

```java
@Component
public class Camera {}
```

４. Main.java

```java
HomeController2 home2 =context.getBean("homeCvn",HomeController2.class);
```

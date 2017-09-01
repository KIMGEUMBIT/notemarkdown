10일차(Maven 설정 및 메인페이지 작성(스프링타일즈))
---------------------------------------------------

##### board-servlet.xml

-	class가 아이디 없이 해당 클래스로 이동

```xml
<context:annotation-config />
<bean class="dr.mini.controller.TestController"></bean>
```

##### 현재 날짜 찍기

```
int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
```

##### TextController.java

-	POJO클래스<br> 상속 불가능, 단독, 독립적으로 사용 가능<br> Class는 메서드명, 매개변수에 변화 가능

```java
package dr.mini.controller;

import java.util.Calendar;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class TestController { //POJO클래스=>상속불가능=>단독으로 독립적으로 사용
                              // 클래스-> 메서드명, 매개변수에 변화를 줄 수 있따.
    @RequestMapping("/hello.do")
    public ModelAndView hello() {

        System.out.println("TestContorller 호출");
        ModelAndView mav = new ModelAndView();
        mav.setViewName("hello"); //hello파일
        mav.addObject("greeting", getGreeting());

        return mav;
    }

    private String getGreeting() { //내부에서만 사용하도록 =>private
        System.out.println("greeting에서 호출");
        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
        //현재 시간이 6~10
        if(hour >=6 && hour <=10) {
            return "좋은아침입니다";
        }   else if (hour >=12 && hour <=15) {
            return "점심식사는 하셨나요?";
        }   else if (hour >=18 && hour <=22) {
            return "좋은 밤 되세요";
        }

        return "주말 잘 보내세요";
    }
}
```

---

SpringTiles
-----------

-	New -> Spring Legacy Project -> SpringTiles ->

빌드어쓰에서

자바 로더랑 컴파일러 1.8로 변경쿠

![1](/assets/1.png)

![2](/assets/2.png)

![3](/assets/3.png)

![4](/assets/4.png)

---

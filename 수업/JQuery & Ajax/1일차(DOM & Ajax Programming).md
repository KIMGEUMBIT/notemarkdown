1일차(DOM & Ajax Programming)
-----------------------------

### 자바스크립트 함수(생성자)를 객체(클래스)처럼 선언<br><br>

#### 1. 선언부분과 값 넣는방법

-	형식 :<br> **function 함수명(매개변수,,,)->동적 멤버변수**

```js
<script>
   function Student(name,grade,sung,addr){
       //this.멤버변수=값
       this.name=name;
       this.grade=grade;
       this.sung=sung;
       this.addr=addr;
   }
</script>
```

#### 선언한 함수 인스턴스화

-	형식 : <br> **객체생성->클래스명 객체명=new 클래스명(=생성자)()->자바** <br> **객체명= new 함수명(매개변수~)**

```js
<script>
    hong = new Student("홍길동",1,"남","서울시 강남구 대현빌딩2층");
    alert(hong.name+"/"+hong.grade+"/"+hong.sung+"/"+hong.addr);
</script>
```

### 2.객체생성과 동시에 값 추가

-	형식 <br> **var 객체명 = new 형식();**

```
<script>
    window.onload = function(){

        // 1. 객체 생성
        // 형식) var 객체명 = new 형식();
        var mem = new Object();

        // 2. 변수 선언
        // 형식) 객체명.속성명 = 저장할 값

        mem.id=1234;
        mem.name= "테스트";
        mem.hobby = "바둑";
        mem.sung = "남자";

        // 3. setter 메서드 선언
        //Setter Method -> public void setId(int id){this.id=id;}
        //형식) 객체명.함수명 = function(매개변수명){this.멤버변수=값}

        mem.setHobby = function(hobby){
            this.hobby = hobby;
        }

        // 4. getter 메서드 선언
        //Getter Method -> public 반환값  get멤버변수명(){ return this.멤버변수명}
        //형식) 객체명.함수명 = function(){return this.멤버변수명;}

        mem.getHobby=function(){
            return this.hobby;
        }

        // 5. 메서드 선언
        // public void toString(){}

        mem.toString=function(){
            return this.id+" "+this.name+" "+this.hobby+" "+this.sung;
        }

        // 6. 객체명.함수명()
        mem.setHobby("음악감상");
        alert(mem.toString())
    }
</script>
```

#### 3. key,value값을 이용한 객체생성3(JSON표기법)

```js
<script>
/* 형식) angluar의 선언적 프로그래밍:selector:'app-root'
 * var 객체명={
      속성명(=멤버변수명):값
      속셩명2:속성값
      ,,,
      함수명:function(매개변수~){~}

}
 */
 var person={
      name:"홍길동",
      age:23,
      eat:function(food){
         alert(this.name+'은 오늘'+food+'은 먹습니다.')
      }
}

//객체명.속성명
document.write(person.name);
person.eat("도시락")
</script>
```

#### 4. 배열과 객체(for~in구문)

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>배열과 객체(for~in구문)</title>
<script>
 //배열생성->var 배열객체명=new Array(); // var 배열객체명={요소1,요소2,,,}
 var students=[];//배열생성
 //배열에 값을 저장->push
 students.push({이름:'테스트',국어:100,수학:90,영어:80});
 students.push({이름:'임시',국어:89,수학:78,영어:45});
 students.push({이름:'테스트2',국어:90,수학:85,영어:23});
 students.push({이름:'테스트3',국어:90,수학:78,영어:89});
 //자바스크립트=>for(var i=0~) X ->형식)for(var 첨자변수 in 출력객체명)
     for(var i in students){ //소스코드 반복적인 구문을 줄이기위해
         //1.총점을 구해주는 함수
         students[i].getSum=function(){
             return this.국어+this.수학+this.영어
         }
         //2.평균을 구해주는 함수
         students[i].getAvg=function(){
             return this.getSum()/3;
         }
     }//for
     //출력목적->for in구문
     var output='이름,총점,평균\n';
     for(var i in students){
         output+=students[i].이름+','+students[i].getSum()+','+
                       students[i].getAvg()+'\n'
     }
     alert(output);

</script>
</head>
<body>

</body>
</html>



```

---

### Ajax

![캡처](/assets/캡처.PNG)

#### Ajax의 구성 ★★★★★

１. 자바스크립트

-	이벤트를 연결
-	사용자의 동작 감지, XMLHttpRequest와 DOM, CSS 사이의 중개

２. XMLHttpRequest

-	브라우저에서 생성
-	웹 서버와의 통신 담당

３. DOM(Document Object Model)

-	요청결과를 화면출력
-	문서 구조 담당

４. CSS

-	글자색, 배경색 등의 UI 담당

---

#### 기존방식과 Ajax 방식 차이점 ★★★★★

1.	웹 브라우저가 아닌 ===>웹서버에 직접적으로 요청X XMLHttpRequest 객체가 웹서버와 통신

2.	웹 서버의 응답 결과가 HTML(text/html)이 아닌 단순 텍스트(text) 또는 XML 형태로 보내준다==>json

3.	페이지 이동 없이 결과 표시

---

#### Ajax 프로그래밍 순서 ★★★★

1.	XMLHttpRequest(XHR) 객체 구하기
2.	웹 서버에 요청 전송하기
3.	웹 서버의 응답을 화면에 반영하기(XHR)->DOM

---

##### simple.txt

-	파일형식 : UTF-8

```
simple.txt의 응답 텍스트
```

##### simple2.txt

-	파일형식 : ANSI

```
simple.txt의 응답 텍스트
```

##### simple.jsp

-	파일형식 : UTF-8

```
<%@ page contentType="text/plain; charset=utf-8" %>
simple.jsp의 응답 텍스트
```

##### simple2.jsp

-	파일형식 : euc-kr

```
<%@ page contentType="text/plain; charset=euc-kr" %>
simple2.jsp의 응답 텍스트
```

![KakaoTalk_20170801_145232099](/assets/KakaoTalk_20170801_145232099.png)

---

##### domAjax2.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Ajax의 개요 및 작성법(XHR객체)</title>
<script>
    //IE(ActiveXObject속성), IE외의 chrome,,,(XMLHttpRequest속성)
    //브라우저별로 얻어오는 구문이 틀리다
    // 하지만 IE7이후 XMLHttpRequest속성으로 통합되었다.
    //1.xhr객체를 얻어오기()

    var httpRequest = null; //xhr객체 저장

    function getXHR() {
        if (window.XMLHttpRequest) {
            return new XMLHttpRequest(); //객체를 생성->반환
        }
    }

    function load(url) { //요청문서를 매개변수->서버에 요청하는 함수
        //1.xhr객체를 구해주는 코드
        httpRequest = getXHR();
        //alert(httpRequest);

        //2.xhr객체가 준비작업(콜백함수) 서버에 요청한다
        //콜백함수란? 특별한 조건이 만족하면 자동적으로 실행이 되는 함수

        // xhr객체명.onreadystatechange=콜백함수();
        //viewMessage()???????????????????????????????????????????????????
        httpRequest.onreadystatechage = viewMessage();

        //3.xhr객체명.open(1. 요청방식(get or post),
        //  2. 요청문서(url),
        //  3. 처리방식(비동기 or 동기))

        //비동기방식 -> 채팅,메일보내기 (답장이 오든 안오든 무한요청 가능)
        // ㄴ 상대방이 결과를 보내주는 것과는 상관없이 다른 일을 할 수 있는 방식

        httpRequest.open("GET", url, true);

        //4.xhr객체.send(null or 매개변수); //요청
        httpRequest.send(null);//요청     
        //------------------step2_e------------------------/

    }

    var httpRequest = null;

    function getXHR() {
        if (window.XMLHttpRequest) {
            return new XMLHttpRequest();
        }
    }

    function load(url) {
        httpRequest = getXHR();
        httpRequest.onreadystatechage = viewMessage();
        httpRequest.open("GET", url, true);
        httpRequest.send(null);
    }

    //서버의 결과를 받으면 자동으로 호출되는 함수-> 콜백함수
    function viewMessage() {
        alert("viewMessage()호출됨");
        //1.톰캣서버가 클라이언트의 요청을 다 받았는지 확인->readyState=4
        if (httpRequest.readyState == 4) {
            //2.서버가 클라이언트에게 데이터를 다 전송했는지를 체크
            if (httpRequest.status == 200) { //404(페이지X), 500(문법에러유발)
                //텍스트(text)-> responseText or xml(responseXml), json
                alert(httpRequest.responseText);
            } else { //404 or 500, 403(접근금지)
                alert('실패:' + httpRequest.status);
            }
        }
    }

    /*

    [ document.readyState ]
    - uninitialized - 아직 로딩 시작 되지 않음 1
    - loading - 로딩중 2
    - interactive - 충분히 로드되고 사용자가 상호 작용 할수 있음 3
    - complete - 로딩 완료  4
     */
</script>
</head>
<body>
    <h2>텍스트파일에 대한 한글처리방법</h2>
    <input type="button" value="simple.txt" onclick="load('simple.txt')">
    <input type="button" value="simple2.txt" onclick="load('simple2.txt')">
    <input type="button" value="simple.jsp" onclick="load('simple.jsp')">
    <input type="button" value="simple2.jsp" onclick="load('simple2.jsp')">
</body>
</html>
```

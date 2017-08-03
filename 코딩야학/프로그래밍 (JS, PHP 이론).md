#### 복습과 수업 예고

#### link 태그

-	css를 웹으로 분리할 때 사용
-	분리된 css를 불러 올 때 사용하는 태그

#### 상호관계

**웹브라우저, 웹서버, php, DB**

> 웹서버도 웹브라우저와 같은 소프트웨어이다

1.	웹브라우저가 웹서버에게 http://a.com/a.php 페이지 요청
2.	웹서버는 a.php해석하기 위해 php엔진에게 요청
3.	php는 하드디스크등 a.php파일을 읽어서 내용을 해석한다. (html태그는 넘어가고)
4.	php엔진은 DB에게 topic이라는 테이블의 내용을 가져오라고 요청한다.
5.	DB->php->웹서버->웹브라우저에게 전달

예제)a.php

```php
<html>
<head>
</head>
<body>
  <?php
    //데이터베이스의 topic에서 수업제목들을 가져온다.
  ?>
</body>
</html>
```

---

#### JavaScript vs PHP

##### HTML, CSS

-	정적인 언어이다.
-	사용자가 무언가를 해도 변화가 없다
-	문서를 만들기 위한 목적이다.

#### JavaScript, PHP

-	동적언어이다.
-	사용자가 무언가를 할 때 변경

---

#### 웹페이지에 코드 삽입하기

```php
<?php  ///php시작을 알림
  echo "aa";    
?>
```

위 내용을 웹브라우저에서 소스코드 볼 경우 php엔진에 의해서 해석되고 hello world를 웹페이지에서 동작 할 수 있도록 해놓았기 때문이다.

---

예제) JavaScript를 php에 추가시키기

```php
<script>
  document.write("Hello world");
</script>
```

위 내용을 웹브라우저에서 소스코드로 볼 경우

\<script> document.write("Hello world");\</script> 똑같이 보인다.

-	\**php는 서버쪽에서 실행되는 언어이다. <br> 그렇기에 출력 결과를 웹브라우저에게 보내준다.
-	JavaScript는 웹브라우저가 해석하고 화면에 반영하기 때문이다. 즉 클라이언트 언어이다\**.

---

### 데이터타입과 연산자

#### php

```php
<?php
  echo "10"+"10";
 ?>
```

20을 출력한다 왜냐하면 +를 사용 할 경우 php는 자동으로 연산으로 생각하기때문이다.

---

#### 디버깅

버그? 프로그래밍이 오동작 디버그? 오동작 현상 해결하려는 동작

---

#### 변수

php, JavaScript 변수 지정 방법

```php

<script>
 name= "egoing";
<script>
<?php
  $name = "egoing";
  echo "안녕하세요".$name;
 ?>
```

#### php 의 var_dump(조건식) 함수

```php
var_dump(1==1); // bool(true); 출력
```

---

### 자바스크립트로 로그인 기능 구현하기 (prompt, alert, confirm)

```js
password = prompt("비밀번호를 입력해주세요");
//사용자의 입력값 받기

alert ("경고창");

var cm = confirm("삭제?");
if (cm == true ) {
  alert ("삭제되었습니다");
} else {
  alert("삭제되지 않았습니다.");
}
```

#### 배열

-	연관된 정보를 담는 그릇
-	체계적으로 관리 가능

#### 자바스크립트 배열

```js
var list = new Array("one","two","three");
document.write(list); //전체배열 값 출력
document.write(list[0]); //배열인덱스로 출력
document.write(list.length); //배열 길이
```

#### PHP 배열

```php
$list = array("one", "two", "three");
echo $list[0]; //인덱스를 이용한 배열 출력
echo count($list); // 배열 길이 출력
```

#### Java 배열

```java
String[] list = {"A","B","c"};
String[] list2 = new Array[4];
```

---

#### 배열과 반복문

```php
<?php

<script type="text/javascript">
  list = new Array("최진혁", "최유빈", "한이람", "한이은");

  var i =0;
  while(i<list.length) {
    document.write(list[i]+"<br>");
    i++;
  }
</script>

<?php
$list = array("최진혁", "최유빈", "한이람", "한이은");

  $i = 0;
  while($i <count($list)) {
    echo $list[$i]."<br>";
    $i++;
  }
 ?>
```

---

### 함수

#### 함수- 함수의 기본문법

#### javascript, PHP 함수선언 및 호출

```php
<body>
  <script type="text/javascript">
    function function_name() {
      document.write("Hello Function");
    }

    function_name();
  </script>
  <?php
      function function_name() {
        echo "Hello php Function";
      }
      function_name();
   ?>
</body>
```

#### 함수의 입력과 출력

```php
<script>
function a(input) {
  document.write(input+1+"<br>");
}
a(1);

function a2(input) {
  return input+1;
}
document.write(a2(5)+"<br>");
</script>

<?php
function a($input) {
  return $input + 1;
}

echo a(5);
 ?>
```

#### document란? 객체이다.

---

#### UI vs API

---

#### 자바스크립트(html을 제어 할 수 있는 언어)

##### 예제1) 자바스크립트로 밑줄 넣기

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <style>
    .em {
      text-decoration: underline;
    }
  </style>
</head>
<Script>
function kangjo() {
  document.getElementById('list1').className='em';
  document.getElementById('list1').className='em';
}
</Script>
<body>
  <ol id="list1">
    <li>html</li>
    <li>css</li>
    <li>javascript</li>
  </ol>
  <ul>
    <li>김금빛</li>
    <li>김은빛</li>
    <li>김회경</li>
    <li>형제다</li>
  </ul>
  <input type="button" value="강조" onclick="kangjo()">
</body>
</html>
```

```html
<input type="button" value="white" onclick="document.getElementById('target').className='white'" />
<input type="button" value="black" onclick="document.getElementById('target').className='black'"  />
```

#### php 내용 출력

```php
<?php
  if( empty($_GET['id']) == false ) {
    echo file_get_contents($_GET['id'].".txt");
  }
?>

<?php
  if(empty($_GET['id']) == false){
    file_get_content
  }
?>

```

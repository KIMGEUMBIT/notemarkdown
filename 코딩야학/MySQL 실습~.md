서버접속

| 구분     | Mysql monitor                         | mysqli                                                 |
|----------|---------------------------------------|--------------------------------------------------------|
| 서버접속 | mysql -hlocalhost -uroot - prlarma12; | $conn = mysql_connect('localhost','root', 'rlarma12'); |
| DB 선택  | mysql> use opentutorials              | mysql_select_db($conn, 'opentutorials');               |
| 조회     | select * from topic                   | \$result = mysqli_qurey($conn, "select * from topic"); |
|          |                                       |                                                        |

#### 접속sql

```php
<?php
$conn = mysqli_connect("localhost","root","rlarma12");
mysqli_select_db($conn, "opentutorials");
$result = mysqli_query($conn, "select * from topic");
?>

```

#### 출력sql

```php

<?php
if(empty($_GET['id']) === false) {

  $sql = "select * from topic where id=".$_GET['id'];
  $result = mysqli_query($conn, $sql);
  $row = mysqli_fetch_assoc($result);
}
?>


```

---

#### 연관배열

```php
<?php

$a = array("AA","BB");
echo $a[];

$b = array("title"=>"title_content", "description"=>"java");  //연관배열(assoc 어떤 값에 연결돼 있다.)
echo $b["title"];
 ?>

```

---

#### write.html

```php
<form action="http://localhost/process.php" method="post">
     <p>
       제목 : <input type="text" name="title">
     </p>
     <p>
       작성자 : <input type="text" name="author">
     </p>
     <p>
       본문 : <textarea name="description"></textarea>
     </p>
     <input type="submit" name="name">
   </form>
```

---

#### process.php

```php
 <?php
 $conn = mysqli_connect("localhost", "root", "rlarma12");
 mysqli_select_db($conn, "opentutorials");
 $sql = "INSERT INTO topic (title,description,author,created) VALUES('".$_POST['title']."','".$_POST['description']."' , '".$_POST['author']."', now())";
 $result = mysqli_query($conn, $sql);
 header('Location: http://localhost/index.php');
 ?>

```

#### 열 갯수, 해당 쿼리 상세히 알려주기

```php
<?php
$result = mysqli_query($conn, $sql);

if($result->num_rows == 0 ){}

$row = mysqli_fetch_assoc($result);
//echo var_dump($row); //var_dump: 입력값으로 들어간 해당 $row의 정보를 상세히 보여주는 내장함수

?>
```

#### 예제) 업로드 케어

```php

<?php
$conn = mysqli_connect("localhost", "root", "rlarma12");
mysqli_select_db($conn, "opentutorials");
$sql = "select * from user where name='".$_POST['author']."'";
$result = mysqli_query($conn, $sql);
$row = mysqli_fetch_assoc($result);

//echo var_dump($row); //var_dump: 입력값으로 들어간 해당 $row의 정보를 상세히 보여주는 내장함수

if($result->num_rows == 0 ){
  $sql = "insert into user (name, password) values ('".$_POST['author']."' , '777777')";
  $result = mysqli_query($conn, $sql);

  $sql2 = "select * from user order by id desc limit 1";
  $result2 = mysqli_query($conn, $sql2);
  $row2 = mysqli_fetch_assoc($result2);
  $user_id = $row2['id'];
} else {
  $user_id = $row['id'];
}
echo $user_id;

$sql = "INSERT INTO topic (title,description,author,created) VALUES('".$_POST['title']."','".$_POST['description']."' , '".$user_id."', now())";

$result = mysqli_query($conn, $sql);
header('Location: http://localhost/index.php');
?>

```

---

#### UPLOADCARE

#### htmlspecialchars 은 entity해준다.

```php
htmlspecialchars("<a>");
//htmlentity로 값을 리턴해준다.
//보기 화면 <a>
//소스 &lt;a&gt;
```

#### strip_tags

### 라이브러리

-	중복해서 사용되는 로직을 재사용 할 수 있도록 부품화(모듈화) 시키는것

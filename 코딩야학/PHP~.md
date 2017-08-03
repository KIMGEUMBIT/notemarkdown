웹브라우저 --> 웹서버-->php-->file(우리가 작성한 파일)

웹브라우저가 http://a.com/a.php 보여달라고 웹서버에게 요청 웹서버는 php에게 해석해달라고 요청 php에게 해석을 받아 웹브라우저에게 돌려준다.

---

### PHP 실습2

http://localhost/php/1.php?name=egoing&id=1&age=19

-	주소와 값을 구분 : ?
-	값과 값 구분 : &

```php
 <?php
   echo $_GET['name'].",".$_GET['id'].",".$_GET['age'];
  ?>
```

-	파일 불러오기 : file_get_contents("디렉토리/파일명");

```php
<?php
  echo file_get_contents("./data/".$_GET['id'].".txt");
 ?>
```

---

#### 데이터베이스(MySQL) 이론

-	정보를 관리해주는 애플리케이션

#### DBTABASE

-	안전하다.
-	빠르다.(인덱스)
-	프로그래밍적으로 제어 가능

---

###### cmd를 통해 mysql 제어할 것임

```
cd C:\Bitnami\wampstack-5.6.30-3\mysql\bin
mysql -hlocalhost -u root -prlarma12!
```

##### mysql -hlocalhost -uroot -p 의미

mysql : mysql모니터 프로그램을 실행시키겠다 -h : -h뒤에 따라오는 이것이 mysql주소 -u : -u 뒤에 오는 것이 비밀번호 -p : 비밀번호를 입력받아라

##### 데이터베이스 보기

```
show databases;
```

##### 데이터베이스 생성

```
CREATE DATABASE opentutorials CHARACTER SET utf8 COLLATE utf8_general_ci;
```

##### 데이터베이스 선택

```
use opentutorials; //이 db를 사용하겠다
```

##### 테이블생성

```
CREATE TABLE `topic` (
`id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(100) NOT NULL,
  `description` text NOT NULL,
  `author` varchar(30) NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

##### 생성된 테이블 확인

```
show tables;
```

#### colum 추가

// 컬럼 이름: \`` 값 입력 ''

INSERT INTO `topic` (title, description, author, created) VALUES('about javascript', 'javascript is ~ ', 'egoing', '2015-04-10 12:20:5');

#### select * from topic;

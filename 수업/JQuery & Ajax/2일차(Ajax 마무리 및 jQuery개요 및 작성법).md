2일차(Ajax 마무리 및 jQuery개요 및 작성법)
------------------------------------------

#### jQuery

#### **jQuery의 특징** ★★★★

1.	일단 무료, 오픈소스
2.	작은 용량 (min : 18KB, uncompressed : 114KB)
3.	수많은 사용자 커뮤니티
4.	웹 브라우저간의 차이를 자체적으로 표준화 플러그인의 다양성 잘 정리된 API 문서 브라우저보다 앞선 W3C 명세 수용 사용자인터페이스 제공

---

-	사용법

```
<script type="text/javascript"
    src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script>
  $(function(){
    alert("cdn방식을 이용함!!");

  });
  </script>
```

#### 기본형식

-	$(element(태그명) or id or class ...).함수명(~).함수명2,,,,

#### ex ) 형식1 : css

```
$('h1').css('color','red');
$('span').css('border','3px solid blue');
```

#### ex ) 형식2 : class

-	$(태그명.아이디명).함수명(~)

```
$('p.my').css('border', '5px dotted green');
```

#### ex) 형식 : id

```
$('#content').css('background','yellow')
$('*').('color','red')
```

#### css 나열

```
test.css('font-family','궁서체' ).css('font-size','30pt').css('color','red')
```

#### each함수

-	형식) $('선택자').each(호출할 함수명 | 익명함수(function(){}))

#### attr함수

-	$(this).attr('속성명','속성값');
-	속성명에 대한 속성값을 넣어준다.

```html
<style>
    .fromjQuery0 {color:red}
    .fromjQuery1 {color:pink}
    .fromjQuery2 {color:blue}
    .fromjQuery3 {color:yellow}
</style>
```

```js
<script>
var i = 0;

$(function(){
    $('ul').each(forEach);
    // ul의 forEach함수 선언
})

function forEach(){
  $(this).attr('class','fromjQuery'+i);
  alert(i);
  i++
}

</script>
```

```html
<ul><!-- 태그정보.setAttribute('속성명','값')-->
    <li>jQuery each()연습</li>
    <li>jQuery each()연습2</li>
    <li>jQuery each()연습3</li>
    <li>jQuery each()연습4</li>
</ul>
```

---

#### jQuery 값 가져오기★★★★

1.	text()<br>오직 **태그의 텍스트만** 가져오는 함수 // **getter**<br> <-> text(매개변수(값))// **setter**
2.	html()<br> **태그와 텍스트들** 까지 같이 가져옴<-> html(매개변수)
3.	val()<br>**폼태그안에 입력양식에 해당되는 값** 을 가져올때(input,check~) //<->val(매개변수(값))->Setter

#### each함수2

```js
  $(function(){
    $('#root').children().each(function(){
          txt += $(this).text();

          //alert( $this.text() );
            //홍길동

          //alert( $this.html() );
            //<b>홍길동</b>

          //alert( $this.val() );
            //
      })
  })
```

```html
<div id="root">
   <div value="홍길동값이다"><b>홍길동</b></div>
   <div>김길수</div>
   <div><i>테스트</i></div>
   <div>임시</div>
</div>
```

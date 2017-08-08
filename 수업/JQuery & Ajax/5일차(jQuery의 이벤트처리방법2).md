5일차(jQuery의 이벤트처리방법2)
-------------------------------

### bind 함수

-	bind()의 경우 파라미터의 값으로 이벤트 이름을 넣음 으로써 해당 이벤트를 체크

#### 문) 아래와 같은 메서드를 작성하여라

```js
$("div").click(function() { alert('click'); }
```

#### 정답)

```js
$("div").bind(click, function(){ alert('click'); })
```

> .click() 메서드의 직접호출이 아닌 해당 이벤트의 이름을 넘김으로 써 동일한 효과를 얻을 수 있습니다.

#### remove(), empty()

-	remove() : 자신을 포함 전체 삭제
-	empty() : 자식 태그만 삭제

```
$('#button1').click(function(){
  $('#div1').remove()//$(선택자).remove()->전체 내용 삭제
})
//
$('#button2').click(function(){
  $('#div1').empty()//$(선택자).empty()->특정태그의 자식태그을 모두 제거
})
//
$('#button3').click(function(){
  $('.test').empty()//특정 자식태그의 내용 삭제
})
```

---

#### 이벤트 형식

```
//형식)window.on이벤트종류명=호출할 함수명 또는 익명함수
//형식2)window.addEventListener(이벤트종류,실행할함수명)
//형식3)$(이벤트발생 대상자 선택자).bind(등록할 이벤트종류명,함수명)->react
```

#### 'dblclick' 더블클릭

#### $(this).unbind(); //이벤트 해제

```
$(this).width($(this).width()*2);
$(this).unbind();
```

> 위 처음 더블 클릭 시에<br>\$(this).width($(this).width()*2) 이벤트가 실행되지만<br> **두번 째 더블클릭 이후**에는 **unbind()**에 의해 더블클릭 이벤트가 해제된다.

<br>

#### hover()

-	특정 엘리먼트의 마우스를 올리거나 떠날경우 사용
-	형식 $('선택자').hover(mouseover|function(){}, mouseout()|function(){})

```js
$(function(){
    $('h1').hover(function(){
        $(this).addClass('reverse')
    }, function(){
        $(this).removeClass('reverse');
    })
})
```

#### keyUp

-	키 입력 했을 때 이벤트

```js
$(function(){
    //사용자가 글자를 입력했을 때 keyUp
    $('input').keyup(function(){
        var $value = $(this).val() //입력받은 값
        report($value);
    })

    //버튼을 눌렀을 때
    $('button').click(function(){
        $('input').val('val메서드 연습중')
    })

    //div태그에 입력받은 값을 넣어주는 함수 작성
    function report(msg){
        $("#console").text(msg)
    }
})

```

#### wrap()이벤트

#### preventDefault()

```js
//전송버튼의 기본적인 기능->데이터를 전송시키는 기능(action="aa.jsp")
$('#frm').submit(function(event){
  var new_line = $('<li>'+$('#data').val()+'</li>')
  $('#disp').append(new_line);
  event.preventDefault();
  //전송을 하지 못하게 설정
  //return false
})
```

cf) 이벤트 동작정지 함수들

| 함수명                           | 설명                                                                                                                |
|----------------------------------|---------------------------------------------------------------------------------------------------------------------|
| event.preventDefault()           | 현재 이벤트의 기본 동작을 중단한다.                                                                                 |
| event.stopPropagation()          | 현재 이벤트가 상위로 전파되지 않도록 중단한다.                                                                      |
| event.stopImmediatePropagation() | 현재 이벤트가 상위뿐 아니라 현재 레벨에 걸린 다른 이벤트도 동작하지 않도록 중단한다.                                |
| return false                     | jQuery를 사용할 때는 위의 두개 모두를 수행한 것과 같고, jQuery를 사용하지 않을 때는 event.preventDefault() 와 같다. |

---

```js
<script>
$(function(){
    $('h1').css({'color':'red','text-decoration':'underline'})
    //형식) $('자식선택자').wrap('부모태그 내용')
    $('#message').wrap('<h2>요소안에 자료를 넣기</h2>')
    //focus이벤트->커서가 들어가는 경우 발생
    $('input').focus(function(){
        $(this).addClass('focused')
    })
    //blur이벤트->커서가 벗어났을때 발생하는 이벤트
    $('input').blur(function(){
        $(this).removeClass('focused')
    })
    //전송버튼의 기본적인 기능->데이터를 전송시키는 기능(action="aa.jsp")
    $('#frm').submit(function(event){
        var new_line=$('<li>'+$('#data').val()+'</li>')
        $('#disp').append(new_line)
        event.preventDefault();//전송을 하지 못하게 설정
        //return false
    })
    //a링크 문자열을 클릭한 경우
    $('a').click(function(){
        alert('당신은 이동할 권한이 없습니다.')
        return false;
    })
})
</script>
```

```html
<body>
 <h1>jQuery의 이벤트종류 정리</h1>
  <!-- <h2>요소안에 자료를 넣기 -->
 <div id="message"></div>
  <!-- </h2> -->
 <form id="frm" action="aa.jsp">
   <input type="text" id="data">
   <input type="submit" value="확인">
 </form>
 <ul id="disp">
      <!--
      <li>aaa</li>
      <li>bbb</li> -->
 </ul><p>
 <a href="http://www.naver.com">naver</a>
</body>
```

![123123](/assets/123123.GIF)

#### jQuery로 submit()

-	$('#signup').attr('action','register.jsp').submit(); W

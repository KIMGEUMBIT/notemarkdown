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

하트하트시러욧
왜욧
헤헤

```

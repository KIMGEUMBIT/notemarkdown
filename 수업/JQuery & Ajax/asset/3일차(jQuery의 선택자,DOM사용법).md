3일차(jQuery의 선택자,DOM사용법)
--------------------------------

#### 부모태그 > 자식태그

-	부모태크 아래 자식태그를 가리킨다

```js
$(function(){
    $('body > div').css('border', '3px solid navy');
    $('p > span').css('border', '3px solid red');
})
```

#### 같은 레벨 > 같은 레벨의 옆태그

-	옆에 있는 태그를 선택 시 사용

```js
 $(function(){
    $('p > span').css('font-size', '30px');
 })
```

```html
<p>
  <span>태그정보</span>
  <a>one</a>
  <span>태그정보2</span>
</p>
```

![sdf](/assets/sdf.PNG)

---

#### 요소[속성명]

| 형식              | 의미                                                                        |
|-------------------|-----------------------------------------------------------------------------|
| 요소[속성명]      | 특정 속성을 가진 태그를 찾을 경우 사용                                      |
| 요소[속성명=값]   | 속성값이 일치하는 태그를 찾아라                                             |
| 요소[속성명!=값]  | **속성값이 일치하지 않는** 태그를 찾아라                                    |
| 요소[속성명^=값]  | 지정한 값으로 **시작하는** 태그를 찾아라                                    |
| 요소[속성명｜=값] | **지정하는값** 을 찾거나 **지정한글자-** 태그를 찾아라                      |
| 요소[속성명$=값]  | 지정한 값으로 끝나는 태그를 찾아라                                          |
| 요소[속성명*=값]  | 지정한 값을 포함한 태그를 찾아라<br>(ex sql의 like연산자 비슷) <br>공백인식 |
| 요소[속성명~=값]  | 지정한값을 단어로서 포함하는 태그를 찾아라<br> 공백인식                     |

---

```
$('input[type=button]').val('회원가입')
=
$('input:button').val('회원가입')
```

#### jQuery로 checkbox 선택/해제

```js
$(function(){
  $('input:checked').attr('checked', true) //체크
  $('input:checked').attr('checked', false) //체크 해제
})
```

#### jQuery로 button 활성화/비활성화

```js
$(function(){
  $('input:disabled').attr('disabled', true) // 비활성화
  $('input:disabled').attr('disabled', false) // 활성화

})
```

#### setTimeout()

-	한번만 실행

-	setTimeout(호출할 함수명|익명함수(function(){<br><br>},1000)<br><br>->1초(한번만 실행)

```js
setTimeout(function(){
  var value = $('select > option:selected').val()
  alert(value);
},2000 )
```

#### setInteval()

-	반복적으로 실행

-	setInteval(호출할 함수명|익명함수(function(){<br><br>},1000)<br><br>->1초(한번만 실행)

```js
  setTimeout(function(){
    var value = $('select > option:selected').val()
    alert(value);
  },2000 )

```

```js


```

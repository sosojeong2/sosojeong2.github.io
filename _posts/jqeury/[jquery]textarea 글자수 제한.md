---
layout: post
title: "[jquery]textarea 글자수 제한"
date: 2021-03-17 09:41:23 +0900
category: jqeury
---

# [JQuery]textarea 글자수 제한

부제 : textarea에 최대 100자까지만 입력 가능하게 하기 

textarea에 글자를 입력받을 때 최대 N자까지만 입력 가능하게 하고싶을 때가 있다. 

```html
<textarea id="test" name="test" cols="30" rows="10"></textarea>
<div id="test_cnt">(0 / 100)</div>
```

```jsx
$(document).ready(function() {
    $('#test').on('keyup', function() {
        $('#test_cnt').html("("+$(this).val().length+" / 100)");
 
        if($(this).val().length > 100) {
            $(this).val($(this).val().substring(0, 100));
            $('#test_cnt').html("(100 / 100)");
        }
    });
});
```

- **<textarea> 태그의 maxlength 속성**

    <textarea> 태그의 maxlength 속성은 <textarea> 요소에 입력할 수 있는 최대 문자수를 명시합니다.

```jsx
<textarea maxlength="100"></textarea>
```
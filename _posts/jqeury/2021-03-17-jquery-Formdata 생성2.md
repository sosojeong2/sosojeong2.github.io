---
layout: post
title: "[jquery]Formdata 생성"
date: 2021-03-17 09:40:23 +0900
category: jqeury
---

# [JQuery]form data 생성

해봐야될것.. 다중파일을 리스트로 받기

- (보통 text 파일만 있는 경우) form 데이터를 Object로 바꿔주는 Jquery serializeObject 플러그인 입니다.
- (파일 업로드를 할 경우) new formdata를 사용하였다.

— — — — — — — — — — — — — —    1번    — — — — — — — — — — — — — — — 

데이터 전송 시 데이터가 적을 때는 간단할 수 있겠지만 데이터가 많아질수록 세팅하기 힘들고 form을 수정하면 javascript도 수정해야하는 번거로움이 생긴다. 

```html
<form id="form"> 
	<input type="text" id="email" name="email"/> 
	<input type="text" id="age" name="age"/> 
</form>
```

```jsx
var email = $('#email').val(); 
var age = $('#age').val(); 
var data = { 
	email : email, 
	age : age 
};
```

JQuery 에서 지원하는 serializeArray는 form내부의 input 태그의 특정 type의 name 속성을 key, 값은 value로 갖는 Object 배열을 만들어 준다. 

```jsx
$("#form").serializeArray();

// 결과 
[{"name":"email","value":"soso@gmail.com"},{"name":"age","value":"26"}]
```

name을 통한 객체화가 가능하다. 

```html
<input name="foo" value="a"> 
<input name="foo" value="b">
```

```jsx
$("#form").serializeArray();

// 결과 
{foo: ["a", "b"]}
```

숫자인자로 갖는 []로 선언시 value는 array의 특정 index에 추가된다.

```html
<input name="foo[2]" value="a"> 
<input name="foo[4]" value="b">
```

```jsx
$("#form").serializeArray();

// 결과 
{foo: [,"a", "b",]}
```

문자인자를 갖는 []로 선언시 인자를 key값, value로 한 object로 생성한다. 

```html
<input name="foo[bar]" value="a"> 
```

```jsx
$("#form").serializeArray();

// 결과 
{foo: 
	{
		bar : "a"
	}
}
```

— — — — — — — — — — — — — —    2번    — — — — — — — — — — — — — — — 

multipart/form-data 는 파일 업로드가 있는 양식요소에 사용되는 enctype 속성 중 하나이고, multipart는 폼데이터가 여러부분으로 나뉘어 서버로 전송되는 것을 의미한다. 

```html
<form id="fileUploadForm" action="fileUpload" method"post" encType="multipart/form-data">
	<input type="file" id="fileUpload1" name="fileUpload1" />
	<input type="file" id="fileUpload2" name="fileUpload2" />
</form>
```

```jsx
var form = $("#fileUploadForm")[0];
var formData = new FormData(form);

$.ajax({
    url : "fileUpload.do",
    type : "post",
    contentType : false,
    processData : false,
    data : formData,
    success : function (result) {
		       alert("성공");
    },
    error : function(error){
        alert("실패");
    }
});
```

```java
@Controller
public class mainController{
	
	  @RequestMapping(value = "/fileUpload.do", method = RequestMethod.POST)
    public String fileUpload(MultipartHttpServletRequest multi){
        String fileName = "";
				Iterator<String> files = multi.getFileNames();

				while(files.hasNext()){

					String uploadFile = files.next();
					MultipartFile mfile = multi.getFile(uploadFile);
					fileName = mFile.getOriginalFilename();

					try{
						mFile.transferTo(new File(path+fileName));
					} catch(Exception e){
						e.printStackTrace();
					}

				}
        return "ajaxUpload";
    }
}
```
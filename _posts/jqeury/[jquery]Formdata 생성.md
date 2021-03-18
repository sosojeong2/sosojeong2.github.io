---
layout: post
title: "[jquery]Formdata 생성"
date: 2021-03-17 09:39:23 +0900
category: jqeury
---

# [jquery]Formdata 생성

Ajax로 form을 전송할 때 formdata 객체를 만들어서 보낸다.

- FormData 새성

var formData = new FormData();

- Form에 데이터 삽입

formData.append(key, value);

나는 파일 보내기에서 헤맸기 때문에 파일 위주로 써본다. 

- file 보내기

```jsx
<input type="file" id="file1" name="file" class="default" />
<input type="file" id="file2" name="file" class="default" />
<input type="file" id="file3" name="file" class="default" />

<script>
	var formData = new FormData();
  formData.append('file1', $('file1)[0].files[0]);
	formData.append('file2', $('file1)[0].files[0]);
	formData.append('file3', $('file1)[0].files[0]);

  // 데이터 전송(저장하기)
  $.ajax({
      url : "/board/noticeWrite",
      type : "post",
      contentType : false,
      processData : false,
      async: false,
      data : formData,
      success : function (result) {
          if(result.resultCode == '0000'){
              alert("저장되었습니다.");
              location.href="noticeList";
          }else{
              alert("저장이 실패되었습니다.");
          }
      },
      error : function(){
          alert("관리자에게 문의하세요.");
      }
  });

</script>
```

```java
public void noticeWrite(MultipartHttpServletRequest fileList){
  Iterator<String> iter = filelist.getFileNames();
	MultipartFile mfile = null;
  String fieldName = "";

	while (iter.hasNext()) { 
    fieldName = (String) iter.next(); //파일이름, 위에서 file1과 file2로 보냈으니 file1, file2로 나온다.
    mfile = filelist.getFile(fieldName);  //저장된 파일 객체
  }
}
```
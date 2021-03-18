---
layout: post
title: "[java]ByteStream : FileInputStream/FileOutputStream"
date: 2021-03-17 09:34:23 +0900
category: java
---

# [java]ByteStream : FileInputStream/FileOutputStream

![alt text](/public/img/java_image_file/bytestream1.png)

중요한 데이터,자료 들은 대부분 데이터베이스에 저장되어 있거나 파일시스템에 저장된다.

FileInputStream과 FileOutputStream은 바이트 단위로 파일을 통한 입출력을 처리한다.

- FileInputStream

InputStream 클래스를 상속받은 후손 클래스로 하드디스크 상에 존재하는 파일로부터 바이트단위의 입력을 받는 클래스이다. 이 클래스는 출발지점과 도착지점을 연결하는 통로, 즉 스트림을 생성하는 클래스이다.

생성자의 인자로는 File객체를 주거나 파일의 이름을 직접 string형태로 줄 수 있다. (난 string 형태로 줌) 

일반적으로 파일의 이름을 String형태로 주는경우가 많은데 파일이 존재하지 않을 가능성도 있으므로 Exception처리를 해야된다. 

- FileOutputStream

OutputStream 클래스의 후손클래스로 파일을 바이트 단위의 출력을 내보내는 클래스이다.

- 예제

    - 현재버전

    fileInputStream 에서 10바이트씩 데이터를 읽어서 화면에 출력한다. 

    ```java
    ServletOutputStream servletOutputStream = response.getOutputStream();
    String imgPath = filePath + File.separator + img;
    FileInputStream fileInputStream = new FileInputStream(imgPath);
    int length;

    byte[] buffer = new byte[10];

    while((length = fileInputStream.read(buffer)) != -1){
        servletOutputStream.write(buffer, 0, length);
    }
    fileInputStream.close();
    ```

    - 버퍼링을 위한 개선

    파일을 읽을 때 10바이트씩 읽고 쓰기를 반복하고 있으므로 프로그램 효율에 좋지 않다. 

    읽고 쓰는 입출력 작업은 기계적인 동작이며 이런 기계적인 동작은 엄청나게 많은 시간을 요구한다.  따라서 효율적인 프로그램을 작성하고 싶다면 기계적인 동작의 횟수를 줄이는 것이 좋다.

    결론적으로 한꺼번에 많은 바이트를 배열로 읽어드리는 메소드를 사용한다. 

    ```java
    ServletOutputStream servletOutputStream = response.getOutputStream();
    String imgPath = filePath + File.separator + img;
    FileInputStream fileInputStream = new FileInputStream(imgPath);
    int size = fileInputStream.available();
    byte[] buffer = new byte[size];
    int length = fileInputStream.read(buffer);
    servletOutputStream.write(buffer, 0, length);
    fileInputStream.close();
    ```

    파일의 데이터 바이트 수를 가져온 뒤 그 크기만큼의 byte[] 배열을 만든다.

    10바이트씩 읽기와 쓰기를 반복하는 것과는 달리 한번에 읽어서 한번에 출력하므로 기계적인 입출력 횟수가 급감하였다. 

- 버퍼링 개선의 문제점과 사용자 버퍼링

또 생각해야될 것이 한번에 읽는것은 기계적인 동작을 최소화하기 위한 방법이었지만, 무조건 버퍼의 크기를 크게하는것이 좋은가에 대해서도 생각해 봐야한다.

**버퍼가 크면 메모리의 낭비를 유발할 수 있다.** 

따라서, 버퍼의 크기를 적정선에서 만족할 수 있도록 잡아서 입력과 출력의 반복이 필요하며,

1024, 2048, 4096, 8192와 같이 배수 형식으로 버퍼의 크기를 잡는 것이 일반적이다. 

buffer size를 1024로 하였고, size를 10으로 두었을때와 시간을 비교해 보았다. 

```java
ServletOutputStream servletOutputStream = response.getOutputStream();
String imgPath = filePath + File.separator + img;
FileInputStream fileInputStream = new FileInputStream(imgPath);
int length;

byte[] buffer = new byte[1024];
d= new Date();
long start = d.getTime();

while((length = fileInputStream.read(buffer)) != -1){
    servletOutputStream.write(buffer, 0, length);
}

d= new Date();
long end = d.getTime();
System.out.println("복사 시간 : " +(end-start));

fileInputStream.close();
```

buffer size가 1024일때? 1초

buffer size가 10일때? 40초

- 결론

FileInputStream으로 읽어오는 데이터를 모두 사용하고 용량이 크지 않다면 한번에 입력과 출력을 수행하는것이 효율적이다.

하지만, 한번에 메모리에 담아서 처리하기에는 용량이 크거나 전체에 비해 일부 데이터만 사용한다면 한번에 읽어오는 것은 비효율적이다.

상황에 맞게 잘 쓰길..ㅎㅎㅎ

참고 URL : 

[[JAVA] ByteStream : FileInputStream / FileOutputStream](https://hyeonstorage.tistory.com/236)
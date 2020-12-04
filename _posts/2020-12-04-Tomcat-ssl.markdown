---
layout: post
title: "[Tomcat]ssl 적용하기"
date: 2020-12-04 13:54:23 +0900
category: Tomcat
---
 
Was를 이용한 단독 서버를 이용하는 경우 SSL을 적용하는 방법이다.  


> 인증서를 준비한다. 

* `keytool -genkey -alias tomcatTest -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore-test.p12 -validity 3650`  

* `keytool -importkeystore -srckeystore keystore-test.p12 -srcstoretype pkcs12 -destkeystore tomcat-test.jks -deststoretype jks`  

**설명**     
-genkey		  : 개인 키와 자체서명 인증서를 생성하는 명령어  
-export		  : 인증서 파일 추출(truststore)  
-import		  : 인증서를 키 스토어에 저장  
-list		    : JKS파일 조회  
-v        	: 결과를 상세하게 보기 옵션이다.   
-keystore  	: 키가 저장될 JKS를 지정한다. 없을 경우는 생성한다.   
-alias      : 키의 별칭이다.    
-sigalg    	: 인증서의 알고리즘이다. 해시 알고리즘으로 구성된다.   
-keysize   	: 직역대로 키 사이즈이며 키 사이즈가 클 수록 암복호화 시간이 오래 걸리지만 좀 더 안전하다.    
-validity   : 인증서의 유효기간이다.    


> tomcat server.xml 수정

**/Severs/Tomcat/server.xml 에 수정**   

```console
<Connector
  URIEncoding="UTF-8" connectionTimeout="20000" protocol="HTTP/1.1"  
  port="443" scheme="https" secure="true" SSLEnabled="true"
  keystoreFile="인증서경로/tomcat-test.jks 파일" keystorePass=".jks 패스워드"  sslProtocol="TLS"
/>
```

> trouble shooting  

** INFO: Error parsing HTTP request header Note: further occurrences of HTTP header parsing errors will be logged at DEBUG level **   

<Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on"/>


* * *  
참고   
https://cheezred.tistory.com/124
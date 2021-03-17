---
layout: post
title: "[Centos]service 등록하기"
date: 2020-12-09 09:37:23 +0900
category: Centos
---

# Centos7 에서 service 등록

## service 등록 예시

vi /etc/systemd/system/haproxy.service
```
[Unit]
Description=haproxy
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/sw/proxy/haproxy/start_proxy.sh start
ExecStop=/sw/proxy/haproxy/start_proxy.sh stop
User=test
Group=test

[Install]
WantedBy=multi-user.target
```

<br/>

> [Unit]섹션

**Description**   
해당 유닛에 대한 상세한 설명을 포함한다.    

**After**    
해당 유닛이 시작된 이후 나열된 유닛이 실행된다. (network와 mysqld 서비스가 실행 후 작동하게 된다.)   

<br/>

> [Service]섹션   

**Type**    
* "simple" (기본값) 유닛이 시작된 경우 즉시 systemd 는 유닛의 시작이 완료되었다고 판단한다. 다른 유닛과 통신하기 위해 소켓을 사용하는 경우 이러한 설정을 사용하면 안된다.  
* "forking" 자식 프로세스를 생성이 완료되는 단계까지를 systemd 가 시작이 완료되었다고 판단하게 된다. 부모 프로세스를 추적할 수 있도록 PIDFile= 필드에 PID 파일을 선언해 주어야 한다.  
* "oneshot" 은 "simple" 과 다소 유사하지만 단일 작업을 수행하는데 적합한 타입니다. 또한 실행 이후 해당 실행이 종료되더라도 RemainAfterExit=yes 옵션을 통해 유닛이 활성화 상태로 간주할 수 있다.  
* "notify" 은 "simple" 과 동일하다. 다만 유닛이 구동되면 systemd 에 시그널을 보낸다. 이때 시그널에 대한 내용은  libsystemd-daemon.so 에 선언 되어 있다.  
* "dbus" DBUS 에 지정된 BusName 이 준비될때까지 대기한다. 즉 DBUS 준비가 완료된 이후 유닛이 시작되었다고 간주한다.  

**User**    
유닛의 프로세스를 수행할 사용자명, 그룹명 등을 지정한다.    

**Environment**    
해당 유닛에서 사용할 환경 변수를 선언한다. 또한 반드시 Exec= 옵션보다 위에 있어야된다.    

**EnvironmnetFile**    
해당 유닛에서 사용할 환경 변수 파일을 선언한다. 또한 반드시 Exec= 옵션보다 위에 있어야된다.    

**ExecStart**    
구동 명령어(스크립트)를 선언한다. 

**ExecStop**    
중지 명령어(스크립트)를 선언한다.

<br/>

> [Install]섹션

systemctl enable 명령어로 유닛을 등록할 때 필요한 유닛을 지정한다. 해당 유닛을 등록하기위한 종속성 검사 단계로 보면된다.    

<br/>

> 생성된 서비스를 아래 명령어로 등록 및 시작

```
systemctl enable haproxy.service
systemctl daemon-reload
systemctl start haproxy.service
systemctl status haproxy.service
```

<br/>

* * *
참고URL   
https://confluence.curvc.com/pages/viewpage.action?pageId=16449736
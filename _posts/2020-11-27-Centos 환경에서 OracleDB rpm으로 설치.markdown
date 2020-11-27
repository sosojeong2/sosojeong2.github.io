---
layout: post
title: [install]Centos 환경에서 OracleDB rpm으로 설치
date: 2020-11-27 15:27:23 +0900
category: DB
---


# oracle rpm install

## 1. 오라클 설치 파일 다운로드     

oracle-xe-11.2.0-1.0.x86_64.rpm.zip           

   
## 2. 의존성 라이브러리 설치    
```console
$ yum install libaio bc flex unzip   --downloadonly --downloaddir=/root/oracle_lib
$ rpm -ivh *
```

## 3. zip 파일 압축 풀기 
```console
$ unzip -q oracle-xe-11.2.0-1.0.x86_64.rpm.zip
$ cd Disk1
```

## 4. rpm 파일 설치

임시로 가상 메모리 추가 
오라클 설치시 최소 1990MB의 가상메모리가 필요하다
```console
// 가상 메모리용 파일 생성
$ dd if=/dev/zero of=/swapfile bs=1024 count=4194304

//  파일을 가상 메모리로 포맷
$ mkswap /swapfile

//  가상 메모리 활성화
$ swapon /swapfile
swapon: /swapfile: insecure permissions 0644, 0600 suggested.

// 가상 메모리 용량 확인
$ swapon -s
Filename    Type  Size Used Priority
/dev/sda1                               partition 2047996 134528 -1
/swapfile    
```

rpm 파일 설치 
`rpm -ivh oracle-xe-11.2.0-1.0.x86_64.rpm`


## 5. oracledb configure

port번호, system계정 비밀번호 ...   
`/etc/init.d/oracle-xe configure` 


## 6. 오라클 서비스 시작
`/etc/init.d/oracle-xe start`

## 7. 방화벽 설정
`firewall-cmd --add-port=1521/tcp --permanent`   
`firewall-cmd --reload`

## 8. /etc/profile 편집
```console
$ vi /etc/profile
// 맨아래줄에 추가
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
export ORACLE_SID=XE
export PATH=$ORACLE_HOME/bin:$PATH

$ source /etc/profile
$ init 6 
```

## 재부팅 후 root계정으로 로그인
```cosole
$ /etc/init.d/oracle-xe start

$ sqlplus system/1234

```


# 
참고 url :   
http://yumdevelop.blogspot.com/2018/03/linux-oracle.html  
https://corock.tistory.com/232
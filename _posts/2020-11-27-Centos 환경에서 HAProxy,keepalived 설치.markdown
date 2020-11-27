---
layout: post
title: Centos 환경에서 HAProxy,keepalived 설치
date: 2020-11-27 15:27:23 +0900
category: install proxy
---

# HAProxy , Keepalived


 개발환경

centos 7.6  
vm1 : 10.10.10.155  
vm2 : 10.10.10.156  
vip : 10.10.10.157  
  
  
## haproxy 설치 
의존성 라이브러리, haproxy.tar 파일 다운  
   
-> 인터넷이 되는 환경에서 haproxy 의존성 라이브러리, haproxy파일 다운
  ```console 
    $ yum install gcc openssl pcre-static pcre-devel systemd-devel openssl-devel  --downloadonly --downloaddir=/sw/install/haproxy/library 

    $ rpm -Uvh * 

    $ wget http://www.haproxy.org/download/2.1/src/haproxy-2.1.9.tar.gz

    $ tar -zxvf  haproxy-2.1.9.tar.gz  
  ```

-> 내부망으로 파일 옮기고 설치 
  ```console 
    $ rpm -Uvh * 
    $ tar -zxvf  haproxy-2.1.9.tar.gz  
    $ cd haproxy-2.1.9
    $ make USE_NS=1 USE_TFO=1 USE_OPENSSL=1 USE_ZUB=1 USE_PCRE=1 USE_LIBCRYPT=1 USE_THREAD=1 USE_SYSTEMD=1 TARGET=linux-glibc
    $ make install
  ```

## keepalived 설치  

-> 인터넷이 되는 환경에서 keepalived 의존성 라이브러리, keepalived 파일 다운
  ```console 
    $ yum install kernel-headers kernel-devel libnfnetlink-devel libnl-devel --downloadonly --downloaddir=/sw/install/keepalived/library

    $ wget http://www.keepalived.org/software/keepalived-2.0.11.tar.gz
  ```

-> 내부망에 파일 옮기고 설치
  ``` 
    $ rpm -Uvh * 
    $ tar -zxvf  keepalived-2.0.11.tar.gz  
    $ cd keepalived-2.0.11
    $ ./configure
    $ make && make install
  ```


## 로그 적용  

```console
  $ vi /etc/rsyslog.con
  -------------------------------------------------------------------------
  # 15번,16번째줄 주석 제거

  # 17번째줄 추가   
  $AllowedSender UDP 127.0.0.1
  
  # 54번째줄 변경
  *.info;mail.none;authpriv.none;cron.nonei,local5.none   /var/log/messages

  # 55번쨰줄 추가
  local5.*  /logs/proxy/haproxy.log
  -------------------------------------------------------------------------

  $ service rsyslog restart
```


## 커널값 수정  
```console
  $ vi /etc/sysctl.conf
  -------------------------------------------------------------------------
  net.ipv4.ip_forward = 1
  net.ipv4.ip_nonlocal_bind = 1
```

## 프로세스 자원 한도 설정 변경  
```
  $ vi /etc/security/limits.conf
  -------------------------------------------------------------------------
  # 마지막줄에 추가
  *   soft   nofile   65565
  *   hard   nofile   65565
  -------------------------------------------------------------------------

  # 변경내용 확인
  $ ulimit -a 
```

  

## VM에서 가상아이피(VIP) 세팅하기  
VM1 : nat(enp0s3), host(enp0s8)  
VM2 : nat, host  

> VM1에 가상아이피 설정하기
```
  $ vi /etc/sysconfig/network-scripts/ifcfg-enp0s8:0
  --------------------------------------------------
    TYPE=Ethernet
    BOOTPROTO=static
    DEVICE=enp0s8:0
    ONBOOT=yes
    IPADDR=10.10.10.157
    NETMASK=255.255.255.0
    GATEWAY=10.10.10.1
  ---------------------------------------------------
```

## haproxy config file 
```cfg
global
    daemon
    maxconn    4096
    user       haproxy
    pidfile    /sw/haproxy/run/haproxy.pid

    setenv     DORO_DAPP1   "10.10.10.155"
    setenv     DORO_DAPP2   "10.10.10.156"

defaults
    mode    http

    timeout  client 3m
    timeout  server 3m
    timeout  connect 60m

    option   dontlognull
    log      127.0.0.1:514 local5
    log-format "%ts %ci:%cp => %fi:%fp => %si:%sp(%s) "


#######################################################################
# dapp
#######################################################################
frontend doro_dapp
    bind  *:8080

    use_backend doro_dapp if { src -f /sw/haproxy/whitelist_doro.list }

backend doro_dapp
    balance   roundrobin
    server    dapp1    "${DORO_DAPP1}":80 check on-marked-down shutdown-sessions
    server    dapp2    "${DORO_DAPP2}":80 check on-marked-down shutdown-sessions

```

## keepalived config file
```
global_defs{
    router_id keepalived
    script_user root
    enable_script_security
}

vrrp_script chk_haproxy {
    script '/sbin/pidof haproxy'
    interval 2
    weight 2
}

vrrp_instance VI_1 {
    state MASTER
    interface enp0s8
    virtual_router_id 51
    priority 101
    advert_int 1

    unicast_src_ip 10.10.10.155  # local IP
    unicast_peer{
       10.10.10.156            # keepalived node2 IP
    }
    virtual_ipaddress{
        10.10.10.157           # VIP 1
    }
    track_script {
      chk_haproxy
    }

```

## haproxy, keepalived start, stop
> keepalived 
  ``` 
    $ keepalived -f [keepalived.conf] -p keepalived.pid 

    $ kill [keepalived pid]

  ```

> haproxy
  ```
  $ haproxy -f [haproxy.cfg] -p haproxy.pid

  $ kill [haproxy.pid]
  ```



## Trouble shooting
로그 적용할때     
**rsyslogd: file '/logs/haproxy.log': open error: Permission denied**  
selinux 꺼주기! ㅋㅋ  `setenforce 0`



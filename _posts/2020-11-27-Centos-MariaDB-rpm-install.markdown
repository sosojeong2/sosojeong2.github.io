---
layout: post
title: "[install]Centos 환경에서 MariaDB rpm으로 설치"
date: 2020-11-27 15:27:23 +0900
category: DB
---

# Centos 환경에서 MariaDB rpm으로 설치

## 파일구조

변경가능하다

```sh
├── root
│   ├── mariadb            # mariadb rpm 파일
│   └── mysql              # mariadb data
...
```


<br/><br/>

## MariaDB 설치에 필요한 rpm 구하기

- MariaDB 설치에 필요한 rpm 파일 다운로드

  - yum repo에 MariaDB 패키지 저장소 설정 추가

    ```console
    $ vi /etc/yum.repos.d/MariaDB.repo
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.4/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    ```

  - rpm 다운로드

    ```console
    $ yum install --downloadonly --downloaddir=/root/mariadb MariaDB-client MariaDB-server
    ```

* MariaDB 수동설치
  ```console
  $ cd /root/mariadb
  $ rpm -Uvh boost-program-options-1.53.0-28.el7.x86_64.rpm
  $ rpm -Uvh lsof-4.87-6.el7.x86_64.rpm
  $ rpm -Uvh rsync-3.1.2-4.el7.x86_64.rpm
  $ rpm -Uvh perl*
  $ rpm -Uvh socat-1.7.3.2-2.el7.x86_64.rpm
  $ rpm -Uvh galera-25.3.25-1.rhel7.el7.centos.x86_64.rpm
  $ rpm -Uvh MariaDB*
  ```

<br/><br/>

## MariaDB 사용법

- 실행

  ```console
  $ service mariadb start
  ```

- 패스워드 변경

  ```console
  $ /usr/bin/mysqladmin -u root password '1111'
  ```

- 접속 확인

  ```console
  $ mysql -u root -p
  Enter password: 1111
  ```

- 상태

  ```console
  $ service mariadb status
  ```

- 중지
  ```console
  $ service mariadb stop
  ```

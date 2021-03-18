---
layout: post
title: "[install]Centos 환경에서 MariaDB-galera cluster 구성"
date: 2020-11-27 15:27:23 +0900
category: DB
---
# Centos 환경에서 MariaDB-galera cluster 구성

## 파일구조

변경가능하다

```sh
rpm_install/
├── mariadb
├── mariadb-backup
└── xtrabackup
```

<br/>

## galera 설치에 필요한 rpm 구하기

xtrabackup 기능을 사용하려면 아래 설치파일이 필요하다

```console
$ find / -name wsrep_sst_common

$ find / -name innobackupex
```

없다면 설치

1. rpm 다운로드

   ```console
   $ yum install --downloadonly --downloaddir=/root/rpm_install/mariadb-ackup Maria-backup

   $ rpm -Uvh percona-release-latest.noarch.rpm
   $ yum install --downloadonly --downloaddir=/root/rpm_install/xtrabackup percona-xtrabackup-22
   ```

2. 설치

   ```console
   $ cd /root/rpm_install/mariadb-backup
   $ rpm -Uvh MariaDB-backup-10.4.14-1.el7.centos.x86_64.rpm

   $ rpm -Uvh perl*
   $ rpm -Uvh percona-xtrabackup-22-2.2.13-1.el7.x86_64.rpm
   ```

<br/>

## mysql(mariaDB) 및 4567,4568,4444 포트 허용하기

```console
$ firewall-cmd --state
running

$ firewall-cmd --permanent --add-service=mysql
success

$ firewall-cmd --permanent --add-port={4567,4568,4444}/tcp
success

$ firewall-cmd --reload
success

$ firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: dhcpv6-client mysql ssh
  ports: 4567/tcp 4568/tcp 4444/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

<br/>

## seLinux 허용모드로 설정

```console
$ setenforce 0
$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

```

<br/>

## Galera Cluster 설정

```console
$ vi /etc/my.cnf.d/server.cnf

[galera]
# Mandatory settings
wsrep_on=ON
wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
wsrep_cluster_address="gcomm://192.168.56.104,192.168.56.106"
binlog_format=row
default_storage_engine=InnoDB
innodb_autoinc_lock_mode=2
wsrep_cluster_name='mariadb-cluster'
wsrep_node_address='192.168.56.104'
wsrep_node_name=test-cluster1
#wsrep_sst_method=rsync
wsrep_sst_method=mariabackup
wsrep_sst_auth=root:1111
wsrep_sst_receive_address=192.168.56.104
#wsrep_provider_options="gcache.recover=yes"
#
# Allow server to accept connections on all interfaces.
#
bind-address=0.0.0.0
```

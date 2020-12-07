---
layout: post
title: "[Centos]리눅스 시스템 부팅 시 스크립트 자동 실행하기"
date: 2020-12-07 17:15:23 +0900
category: Centos
---

# 리눅스 시스템 부팅 시 스크립트 자동 실행하기 

## rc.local에 스크립트 등록
* `vi /etc/rc.d/rc.local` 에 실행명령을 넣어준다.  
* 가능하면 풀패스를 적어준다.  
* 시스템(H/W)와 관련된 명령을 입력할 경우에는 부팅시 rc.local의 실행 순서가 빠르기 때문에 실행되지 않는 경우가 있을 수 있다.

centos 7 버전에 스크립트를 기입했는데 실행이 안되길래 보니깐
```
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
```
라고 써있었다.   
결론은 **chmod +x /etc/rc.d/rc.local** 해줘야한다. 

<br/><br/>

## 런레벨(Run level)
시스템 관리의 용이함을 위하여 서비스의 실행을 단계별로 구분하여 적용하는 것을 의미한다.

*  0 : halt (DO NOT set initdefault to this)
시스템 종료를 의미합니다. 즉, 런레벨 0으로 변경하라는 명령을 내리면 시스템을 종료하는 것이죠.

* 1 : Single user mode
시스템 복원모드라고도 하며, 기본적으로 관리자 권한 쉘을 얻게 됩니다.
주로, 파일시스템을 점검하거나 관리자 암호를 변경할 때 사용합니다.

* 2 : Multiuser mode, without NFS (The same as 3, if you do ot have networking)
NFS(Network File System)을 지원하지 않는 다중 사용자 모드입니다.
네트워크를 사용하지 않는 텍스트 유저모드라고 할 수 있죠.

* 3 : Full muliuser mode
일반적인 쉘 기반의 인터페이스를 가진 다중 사용자 모드입니다.
쉽게 말하면 그래픽 유저 모드가 아닌 '텍스트 유저 모드'입니다.

* 4 : unused
4번은 쓰이지 않습니다. 기본적으로는 사용되지 않지만, 임의로 정의해서 사용할 수 있는 레벨입니다.

* 5 : X11
기본적으로는 level 3과 같습니다. 다른 점은 '그래픽 유저 모드' 라는것!!!

* 6 : reboot (DO NOT set initdefault to this)
시스템 재부팅을 의미합니다. 런레벨 6으로 변경하라는 명령을 내리면 시스템을 재부팅 하죠.





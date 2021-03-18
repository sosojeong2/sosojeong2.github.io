---
layout: post
title: "[MariaDB]Sequence"
date: 2021-03-18 15:27:23 +0900
category: DB
---

# [MariaDB]Sequence

마리아디비 10.3이후 버전부터는 기본적으로 시퀀스생성을 사용할 수 있다.

```sql
-- 10.3 버전 이상인지 확인 
SELECT VERSION();

-- 시퀀스 생성문
CREATE SEQUENCE seq_template;

-- 현재 값
SELECT LASTVAL(seq_template);
-- 다음 값
select NEXTVAL(seq_template);

-- 현재 시퀀스 값 초기화
ALTER SEQUENCE seq_template RESTART 1;
```

ID 값을 숫자가 아닌 다양한 방식(예를들어, job0001 이런식으로)으로 저장하고 싶을 경우 사용한다. 

Auto_increment 대신 사용할 수 있다. 

- 채번 시 고려사항
    - Auto_Increment

        이방식은 새로 입력되는 row에 대해 유일한 값을 자동으로 부여하는 옵션을 가진다.

        테이블 생성 시 컬럼에 auto_increment옵션을 정의하는데 한번 증가하면 자동적으로 줄어들진 않는다. 

    - sequence 대체함수

        일련번호를 발급받듯이 문자열과 조합하여 id를 생성하는 등 특별한 규칙을 부여하여 사용하는 경우에는 해당경우를 사용한다. 

        auto_increment보다는 빠르진 않고, 객체함수가 많아지는 단점이 존재한다. 

참조 URL :

[[MariaDB] Sequence 사용 방법](https://bamdule.tistory.com/200)
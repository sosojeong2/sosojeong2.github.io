---
layout: post
title: "[MariaDB]Join"
date: 2021-03-18 15:27:23 +0900
category: DB
---

# [MariaDB]Join

board table과 file table을 left join 을 사용하여 board_no가 같은 것을 찾아서 합쳐준다. 

```xml
SELECT 
      b.no AS board_no,
      b.content ,
      b.date ,
      b.title ,
      b.board_type AS boardType,
      f.original_file_name AS originalFileName,
      f.stored_file_name AS storedFileName,
      f.fmt_type AS fmtType,
      b.rnum
FROM 
  (
      SELECT * , ROW_NUMBER() OVER ( order by b2.date, b2.create_date ) as rnum
      FROM board b2 
      WHERE b2.board_type = 'press'
      AND b2.showYN = 'Y'
  ) AS b    
LEFT OUTER JOIN 
      file f 
ON 
      b.no=f.board_no 
WHERE 
      b.rnum BETWEEN #{rowEnd} AND #{rowStart}
ORDER BY 
      b.date desc, rnum desc
```

찾아보는김에 정리해보자 

JOIN연산은 두 테이블을 결합하는 연산이다. 

- 배경 :

    DB를 사용하면 할 수록 성능최적화가 요구된다. 

    join의 따라 성능이 달라질 수 있다. 

- 목적 :

    - 조인의 종류와 각각의 용도와 차이점을 이해한다.

    - 각 조인 별 SQL 문법의 차이를 이해한다. 

- sql join 종류

![alt text](/public/img/db_image_file/join1.png)

1. Inner Join : 교집합 
2. Cross Join

    한 쪽 테이블의 모든행들과 다른테이블의 모든 행을 조인시키는 기능

    모든 경우의 수, 즉 A테이블 row개수 * B테이블 row개수 만큼의 row를 가진 테이블을 출력

    왜 사용하는지? 테스트로 사용할 대용량의 테이블을 생성할 경우에 사용된다. 

3. Outer Join
    1. Left Outer 
    2. Right Outer
    3. Full Outer 

참조 URL : 

[[DB/MariaDB] SQL 예제를 통한 JOIN의 종류 파악](https://postitforhooney.tistory.com/entry/DBMARIADB-SQL-%EC%98%88%EC%A0%9C%EB%A5%BC-%ED%86%B5%ED%95%9C-JOIN%EC%9D%98-%EC%A2%85%EB%A5%98-%ED%8C%8C%EC%95%85)
---
layout: post
title: "[PostgreSQL]CTE(WITH 절)"
date: 2021-04-05 10:32:23 +0900
category: DB
---

# [PostgreSQL]CTE(WITH 절)

sql문이 복잡해지면 서브쿼리의 중첩이 많아지는데 이렇게 되면 가독성이 떨어진다.

이때 공통 테이블 식(CTE : common table expression)을 사용하면 임시로 테이블을 정의하고 재활용할 수 있습니다. 이때 사용하는 것이 with문 입니다.

## with문

```sql
WITH (테이블 이름) AS (SELECT ~ FROM ~) SELECT ~
```

## with문 활용 (예시)

우선 테이블이랑 데이터 채워넣고

```sql
create table categories_sales(
	category_id integer,
	name varchar(255),
	sales_amout integer
);

insert into categories_sales 
values (1,'dvd',850000),(2,'book',444444),(3,'tape',300000) ;

create table sales_ranking(
	category_id integer,
	product_id varchar(255),
	sales integer
);

insert into sales_ranking 
values (1,'D001',50000),
		(1,'D002',20000),
		(1,'D003',10000),
		(2,'B001',60000),
		(2,'B002',30000),
		(2,'B003',40000),
		(3,'A001',60000),
		(3,'A002',30000),
		(3,'A003',40000) ;
		
commit;
```

categories_sales랑 sales_ranking을 조인한 것을 ranking으로 정의하고 ,

카테고리별로 rank가 가장 놓은 것을 뽑아본다. 

```sql
with ranking as (
	select m.category_id , m."name" , m.sales_amout,
			s.product_id , s.sales, 
			row_number() over(partition by name order by sales desc) as rank
	from categories_sales as m
		join sales_ranking as s
			on m.category_id = s.category_id )

select name, product_id ,rank 
from ranking
where rank =1;
```

with문 밑에 임시테이블을 여러개 만들 수 있다.(with문은 맨 앞에 하나만 쓴다.)

참조 URL : 

[[PostgreSQL] CTE (WITH 절)](https://ysyblog.tistory.com/142)
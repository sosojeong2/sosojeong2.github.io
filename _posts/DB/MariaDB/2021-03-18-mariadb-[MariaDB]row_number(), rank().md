---
layout: post
title: "[MariaDB]row_number(), rank()"
date: 2021-03-18 15:27:23 +0900
category: DB
---

# [MariaDB]row_number(), rank()

- row_number()

페이징 기능에 꼭 필요한 기능은 쿼리실행 결과에 행마다 순번을 매겨주는 rownum일 것이다. 

systax는 아래와 같다.

```sql
ROW_NUMBER() OVER ( 
	[ PARTITION BY partition_expression ] 
	[ ORDER BY order_list ] 
)
```

- rank()

row_number와 같은 일종의 순번이지만 row_number는 무조건 1에서부터 순차적인 순번을 매기는게 일반적인 방식이지만 rank는 순수하게 순위에 대한 순번으로 동률일 경우 같은 순번으로 처리된다. 

- dense_rank()

rank() 함수와 유사하지만 동률 순번이 있을경우 그 다음 순번으로 매겨진다는 다른점이 있다.  

- ntile()

- partition by

위의 함수들과 결합 하여 사용하는 일종의 옵션으로 대상 행들의 결과 집합을 파티션을 나누어

그 파티션 안에서 순위를 매기기 위해 사용된다.

참고 URL : 

[[MariaDB] MariaDB 에서 ROW NUMBER() 사용하기](https://dorongdogfoot.tistory.com/25)
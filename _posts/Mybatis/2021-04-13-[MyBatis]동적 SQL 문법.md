---
layout: post
title: "[MyBatis]동적 SQL 문법"
date: 2021-04-13 09:43:23 +0900
category: mybatis
---

# [MyBatis]동적 SQL 문법

마이바티스의 가장 강력한 기능 중 하나는 동적 SQL을 처리하는 방법이다.

- if

    ```sql
    <select id="getOrgInfo" parameterType="string" resultType="map">
    		select * 
    		from channel 
    		
    		<if test="chnlNm != null">
    				WHERE chnlNm =  #{chnlNm}
       	</if> 

    </select>
    ```

    조건에 맞게 if문을 변경할 수 있다. 

- choose (when, otherwise)

    모든 조건을 원하는 대신 한가지 경우만을 원할 수 있다. 자바에서는 switch 구문과 유사하며 마이바티스에서는 choose 엘리먼트를 제공한다.

    ```sql
    <select id="findChnl">
    	select *
      from channel
    	<choose>
    		<when test="chnlNm != null">
    			where chnlNm like #{chnlNm}
    		</when>
    		<when>
    			where orgNm like #{orgNm}
    		</when>
    		<otherwise>
    			where statCd = "ACTIVE"
    		</otherwise>
    	</choose>
    </select>
    ```

- trim (where, set)

    ```sql
    <select>
    	select * 
    	from channel
    	<trim prefix="where" prefixOverrides="and | or">
    		<if test="param1 != null">
    			and param1 = #{param1}
    		</if>
    		<if test="param2 != null">
    			and param2 = #{param2}
    		</if>
    	</trim>
    </select>
    ```

- foreach

    동적 SQL에서 공통적으로 필요한 것은 collection에 대해 반복처리 하는 것이다. 

    ```sql
    <select id="getChannel" parameterType="hashmap" resultMap="ChannelVo">
    		SELECT  *
    		FROM channel
    		WHERE CHNL_NM IN
    		<foreach collection="chnlList" item="chnl" open="(" close=")" separator=",">
    			#{chnl}
    		</foreach>
    </select>
    ```

    element 내부에서 사용할 수 있는 item, index 두가지 변수를 선언한다. 

    collection 파라미터로 Map이나 배열객체와 더불어 List, Set등과 같은 반복 가능한 객체를 전달 할 수 있다.

    배열을 사용할 때 index값은 현재 몇번째 반복인지를 나타내고 value 항목은 반복과정에서 가져오는 요소를 나타낸다. Map을 사용할 때 index는 key객체가 되고 항목은 value객체가 된다.

참조 URL : 

[MyBatis 동적 SQL 처리 문법](https://meyouus.tistory.com/93)
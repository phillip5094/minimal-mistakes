---
title: "Mybatis-Mapper-ResultMap"
categories: 
  - 공부
last_modified_at: 2020-02-04T13:00:00+09:00

toc: true
---
2020.02.04

resultMap :  Describes how to load your objects from the database result sets. join mapping을 사용하는 것 처럼 복잡한 상태의 코드를 쓸 때 매우 유용하다.
<br>
<br>
아래와 같은 VO가 있다고 가정하자.
```
public class User{
    private int id;
    private String username;
    private String hashedPassword;
    ...
}
```

간단한 구문에서는 select를 통해서 가져오는 column명과 field명이 같기 때문에 mapping되어서 저장될 것이다.
```
<select id="selectUsers" resultsType="com.someapp.model.User">
    select id, username, hashedPassword
    from TABLE
    where id = #{id}
</select>
```


만약에 column명과 field명이 다르다면 sql구문에 별칭을 지정하여 mapping 시킬 수 있다.
```
<select id="selectUsers" resultsType="com.someapp.model.User">
    select 
        user_id         as "id",
        user_name       as "username",
        hashed_password as "hashedPassword"
    from TABLE
    where id = #{id}
</select>
```

<br>
만약 현재 데이터베이스에 저장되어 있는 column 명이 모두 다르고 상황마다 여러 가지의 select문을 사용한다면 매번 column들의 별칭을 지정하는 것은 매우 힘들 것이다. 다른 방법을 찾아야 한다.
sql구문에 별칭을 지정하는 방법 외에 resultMap을 선언하는 방법을 통하여 명시적으로 선언할 수 있다.
```
<resultMap id="userResultMap" type="User">
    <id property="id" column="user_id" />
    <result property="username" column="username" />
    <result property="password" column="password" />
</resultMap>
```

```
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from TABLE
  where id = #{id}
</select>
```
resultMap에서 property와 column을 mapping 시킬 때 사용하는 엘리먼트이다. 이 둘의 설명은 아래와 같다.

<h4>id와 result 모두 한 개의 column을 한 개의 property 필드에 매핑시킨다. 둘의 차이점은 id 값은 객체 인스턴스를 비교할 때 사용되는 구분자 property로 처리되는 점이다.</h4>
>> 사실 잘 모르겠다. 나중에 좀 더 알아봐야겠다.

<br>
<br>
<br>
&nbsp;하나의 궁금증이 생겨서 mapper-xml에 대해서 공부를 했다. resultMap 안에 collection 엘리먼트를 사용한다면 resultMap 안에 또 다른 resultMap을 중첩시킬 수 있다는 것을 알았다. 만약 데이터베이스에서 join을 하고 싶고 join을 한 결과가 현재 내가 가지고 있는 VO에 mapping 되지 않는다면 어떻게 하면 좋을까? 
<br>
&nbsp;간단하게 생각했을 때 2가지 방법이 가장 쉽게 떠올랐다.<br>
1. mapping이 되는 VO를 따로 만들어서 resultType으로 설정 <br>
2. resultType에 HashMap으로 설정하여 key, value 형식으로 결과값을 전달받는 방법 <br>
<br>
각각의 장단점을 간단하게 찾아보았다.
&nbsp; 1번 방법은 코드가 매우 직관적이라 가독성이 좋을 것 같았다. 하지만 만약 각각 다른 테이블과의 join 연산을 해야하고 결과값의 column 목록들도 모두 다르다면 매번 VO를 새로 만들어야한다. VO를 선언할 때 멤버변수의 type을 지정해줘서 코드적인 문제가 생기면 컴파일 에러가 생긴다는 것이었다.
<br>
&nbsp; 2번 방법은 매번 VO를 생성하지 않고 key, value 형태로 쓸 수 있다는 장점이 있다. 하지만 매번 HashMap.put()을 통해서 값을 넣어야하기 때문에 개발자가 어떤 값이 들어갔는지 일일이 읽어봐야한다는 단점이 있다. 또한 hash가 되기 때문에 overhead가 발생하고 value의 형태가 상위 객체인 Object 형태가 되어버리면 런타임에 에러가 발생하기 때문에 문제점을 찾기 어렵다는 것이었다.
<br>
<br>

사실 아직까지 뭐가 더 좋은지와 현재 프로젝트에는 뭐가 더 좋을지는 잘 모르겠다. 좀 더 지식을 넓힌 후에 다시 포스팅을 해야할 것 같다.



 : https://mybatis.org/mybatis-3/sqlmap-xml.html#Result_Maps

---
title: MyBatis 学习笔记(2)
date: 2018-10-31 19:27:57
tags:
    - mybatis
    - java
categories: 
    - note
---

* [ResultMap](#resultmap)
* [动态 sql](#sql)
* [多表查询](#join)

### <h2 id="resultmap">ResultMap 定义</h2>

``` python
<mapper namespace="com.uerax.mybatis.mapper.StudentMapper">
    <!-- type -> 数据库对应的 JAVA 实体类 -->
    <resultMap type="com.uerax.mybatis.domain.Student" id="result_map_id">
        <!-- id -> 用于映射主键(primary key) -->
        <!-- property -> 实体类属性  column -> 数据库字段 -->
        <id property="id" column="id"/>
        <!-- result -> 用于映射普通字段 -->
        <result property="" column=""/>
    </resultMap>

    <!-- 使用 ResultMap -->
    <select id="getStudentNameById" parameterType="" resultMap="result_map_id">
        // sql
    </select>
</mapper>
```

### <h2 id="sql">动态 sql</h2>

``` python
// if 标签
<select>
    select *
    from `student`
    where 1 = 1
    <if test = "name != null and name != ''">
        and name like '%${name}%'
    </if>
    <if test = "age != null and age != ''" >
        and age = #{sex}
    </if>
</select>

// where 标签
// 自动补上 where 关键字, 同时处理多余的 and, 不能再自行加上 where 关键字
<select>
    select *
    from `student`
    <where>
        <if test = "name != null and name != ''">
            and name like '%${name}%'
        </if>
        <if test = "age != null and age != ''" >
            and age = #{sex}
        </if>
    </where>
</select>

// sql 片段

<sql id="sql_id">
    // sql
    *
</sql>
<select>
    select
    <include refid="sql_id"></include>
    from `studetn`
</select>

// foreach 标签
select *
from `student`
<where>
    // collection -> 要遍历的集合(实体类内的集合属性名)  open -> 循环开始前输出的内容
    // separator -> 分割符号  item -> 设置的循环变量  close -> 循环结束输出的内容
    <foreach collection="ids" open="id in(" item="id" separator="," close=")">
        #{id}
    </foreach>
</where>
```

### <h2 id="join">多表查询</h2>

#### 一对一关联查询

> 方法一

``` python
// foreign key(A.col) references B(col)
class B {}
class A extends B{}
<mapper>
    <select id="" resultType="package.A">
        // sql
    </select>
</mapper>
```

> 方法二

``` python
// foreign key(A.col) references B(col)
class B {}
class A {
    private B b;
}

<mapper>
    <resultMap type="package.A" id="id">
        <id property="alias" column="primaryKey" />
        <result property="alias" column="columnName" />

        // javaType -> B 的数据类型 property -> A 内的 B 属性
        <association property="b" javaType="package.B">
            // 如果在 sql 中设置别名那么 column 要输入别名
            <id property="alias" column="primaryKey" />
            <result property="alias" column="columnName" />
        </association>
    </resultMap>
    <select id="" resultMap="id">
        // sql
    </select>
</mapper>
```

#### 一对多关联查询

``` python
// foreign key(A.col) references B(col)
class B {}
class A {
    private List<B> b;
}

<mapper>
    <resultMap type="package.A" id="id">
        <id property="alias" column="primaryKey" />
        <result property="alias" column="columnName" />
        // collection 用于配置一对多类型
        // ofType -> B 的数据类型 property -> A 内的 B 属性
        <collection property="b" ofType="package.B">
            <id property="alias" column="primaryKey" />
            <result property="alias" column="columnName" />
        </collection>
    </resultMap>
    <select id="" resultMap="id">
        // sql
    </select>
</mapper>
```
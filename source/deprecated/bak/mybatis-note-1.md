---
title: MyBatis 学习笔记(1)
date: 2018-10-30 15:37:39
tags:
    - mybatis
    - java
categories: 
    - note
---

* [配置环境](#environment)
* [核心配置文件](#mainmapper)
* [映射文件](#mapper)
* [别名](#alias)

## <h2 id="environment">配置环境</h2>

* 基础 jar 包 : MyBatis 自带所有 jar 包
* 编写数据库的 JAVA 类并提供 GET / Set 方法

``` python
// 配置文件依赖
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  
// 映射文件依赖
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

### <h2 id="mainmapper">配置文件编写</h2>

``` python
<configuration>
    // 导入 properties 文件
    <properties resource="jdbc.properties" ></properties>
    <!-- 和 spring 整合后 environments 配置将废除 -->
    <environments default="development">
        <environment id="development">
            <!-- 使用 JDBC 事务管理 -->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.user}" />
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载映射文件 -->
    <mappers>
        <mapper resource="url/mapper.xml"/>
    </mappers>
</configuration>
```

### <h2 id="mapper">映射文件 Mapper 编写</h2>

``` python
<!-- namespace 用于隔离 sql 语句 -->
<mapper namespace="student">
    // id -> sql 语句标识符  parameterType -> 入参的数据类型  resultType -> 返回结果数据类型
    <select id="getStudentById" parameterType="int" resultType="com.uerax.mybatis.domain.Student">
        //  #{} 点位符号, 类似 JDBC 的 ?
        //  ${} 字符串拼接指令, 如果入参为普通类型 {} 只能写 value 如 : '%${value}%'
    </select>

    // 多个查询
    <select id="getStudentByStudentName" parameterType="string" resultType="com.uerax.mybatis.domain.Student">
        // sql
    </select>

    // 插入
    <insert id="insertStudent" parameterType="com.uerax.mybatis.domain.Student">
        // sql
    </insert>

    // 修改
    <update id="updateStudent" parameterType="com.uerax.mybatis.domain.Student">
        // sql
    </update>

    // 删除
    <delete id="deleteStudent" parameterType="int">
        // sql
    </delete>
</mapper>
```

### 调用方法

``` python
// 创建 SqlSeesionFactoryBuilder 对象
SqlSessionFactoryBuilder ssfb = new SqlSessionFactoryBuilder();

// 创建核心配置文件输入流
InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");

// 通过输入流创建 SqlSessionFactory 对象
SqlSessionFactory ssf = ssfb.build(is);

// 创建SqlSession 对象
SqlSession ss = ssf.openSession();

// 执行查询 (单个)
Student stu = ss.selectOne("student.getStudentById", 2);
// 查找多个, 映射的 resultType 直接用 List 内部数据类型
List<Student> list = selectList();

// 执行插入
Student student = new Student();
ss.insert("student.insertStudent", student);
// 执行 commit 提交才能成功插入, 或者在openSession(true) 传入 true
ss.commit();

// 执行修改, 删除
// 如果参数包含多个 Student 类的属性则将需要的属性赋值即可
Student stu = new Student();
ss.update("student.updateStudent", stu);
ss.delete("student.deleteStudent", 1);
// 执行 commit 提交才能成功执行, 或者在openSession(true) 传入 true
ss.commit();
ss.close();
```

### 动态开发代理

``` python
<!-- namespace 是接口的全路径名 
	接口方法名必须与 sql id一致 
	接口的入参必须与 parameterType 类型一致 
	接口的返回值必须与 resultType 类型一致 -->
<mapper namespace="com.uerax.mybatis.mapper.StudentMapper">
    // 和上方写法一致
</mapper>

// StudentMapper 是一个接口, 只要遵守上方四条规则底层会自动帮我们实现
public interface StudentMapper {
    Student getStudentByI(Integer id);
    List<Student> getStudent();
    void insertStudent(Student stu);
}

// 调用时需要获取映射
SqlSession.getMapper(StudentMapper.class);
```

### <h2 id="alias">别名设置</h2>

``` python
<typeAliases>
    <!-- 单个别名定义, 不区分大小写 -->
    <typeAlias type="com.uerax.mybatis.domain.Student" alias="student" />

    <!-- 包扫描器, 别名是类的全称,不区分大小写 -->
    <package name="com.uerax.mybatis.domain"/>
</typeAliases>
```

### 映射文件加载方法

``` python
<mappers>
    <mapper resource="url" />
    // 接口文件必须与映射文件在同一目录下, 接口文件必须与映射文件名称一致
    <mapper class="package.mapperName" />
    / 包扫描, 接口文件必须与映射文件在同一目录下, 接口文件必须与映射文件名称一致
    <package name="package" />
</mappers>
```

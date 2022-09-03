---
title: Mybatis 逆向工程
date: 2018-12-20 19:24:08
tags:
    - java
    - mybatis
categories: 
    - note
---

### <h2 id="generator">MyBatis 逆向工程</h2>

* 需要的 jar 包 : mybatis-generator-core 包 (安装了插件不需要导入 jar 包)

``` python
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/spring" userId="root"
                        password="root">
        </jdbcConnection>
        <!--
             默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，
             为 true时把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal
        -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>
        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.uerax.crud.domain"
                            targetProject=".\src\main\java">
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="mapper"
                         targetProject=".\src\main\resource">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.uerax.crud.mapper"
                             targetProject=".\src\main\java">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="" tableName="employee"></table>
        <table schema="" tableName="department"></table>
    </context>
</generatorConfiguration>

// 安装插件直接执行 xml 文件即可生成, 没有安装插件则执行
List<String> warnings = new ArrayList<String>();
boolean overwrite = true;
File configFile = new File("generatorConfig.xml");
ConfigurationParser cp = new ConfigurationParser(warnings);
Configuration config = cp.parseConfiguration(configFile);
DefaultShellCallback callback = new DefaultShellCallback(overwrite);
MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
myBatisGenerator.generate(null);

// 使用 selectByExample
public void testSelectByExample() {
    StudentMapper stuMapper = applicationContext.getBean(StudentMapper.class);
    StudentExample example = new StudentExample();
    // 创建 Criteria
    Criteria c = example.createCriteria();
    // 设置查询条件 c.andAgeBetween(11, 23);
    List<Student> list = stuMapper.selectByExample(example);
    for (Student stu : list) {
        System.out.println(stu);
    }
}

// 使用 insertSelective 自动忽略不插入的值
public void testInsertSelective() {
    StudentMapper stuMapper = applicationContext.getBean(StudentMapper.class);
    Student stu = new Student();
    stu.setName("");
    stu.setAge(41);
    stu.setMoney(22221);
    stuMapper.insertSelective(stu);
}
```

#### 多表查询

``` python
// class A { private B b}
// 在 Mapper 接口内添加自定义方法

// 在 Mapper 配置文件中添加
<sql id="sql_id">
    // 需要查询出来的字段
</sql>

<resultMap id="WithDeptResultMap" type="package.A">
// 将 BaseDeptResultMap 的内容复制进来
<association property="b" javaType="package.B">
    <id column="" property="" />
    <result column="" property="" />
</association>

List<Employee> selectByExampleWithA(A a);

<select id="selectByExampleWithA" parameterType="package.AExample" resultMap="WithDeptResultMap">
// 换成我们需要的查询的字段
<include refid="sql_id" />
// 修改成多表查询语句
from employee e left join department d on e.dept_id = d.dept_id
</select>

// 添加自定义指定查询语句
Employee selectByPrimaryKeyWithA(Integer a.primaryKey);

<select id="selectByPrimaryKeyWithA" parameterType="java.lang.Integer" resultMap="WithDeptResultMap">
select
<include refid="sql_id" />
from employee e left join department d on e.dept_id = d.dept_id
where emp_id = #{empId,jdbcType=INTEGER}
</select>
```

### Idea 使用 maven 创建 Mybatis 逆向工程

``` python
// pom.xml 文件添加插件
// 必须放在 pluginManagement 标签同级的 plugins 标签内
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
    </configuration>
    // 添加数据库驱动依赖复制找不到 JDBC Driver
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
            <scope>runtime</scope>
        </dependency>
    </dependencies>
</plugin>

// maven 的 mybatis-gererator 插件默认作用于 resources 文件夹内的 generatorConfig.xml 配置文件

```
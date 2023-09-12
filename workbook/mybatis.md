# 😀Mybatis

<br>

---

## 🤐<font color="pink">配置文件</font>

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/imooc?useSSL=false"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
      </dataSource>
    </environment>
  </environments>
</configuration>

```

</details>

<br>

## 🥶<font color="pink">select标签</font>

**select 属性**

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| id            | 在命名空间中唯一的标识符                                     |
| parameterType | 语句的参数类型，默认可选，MyBatis 会自动推断                 |
| resultType    | 语句返回值类型，如果返回的是集合，那应该设置为集合包含的类型 |
| resultMap     | 语句返回映射的 id；可以使用 resultType 或 resultMap，但不能同时使用 |
| flushCache    | 设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认为 false |
| useCache      | 设置为 true 后，本条语句的查询结果被二级缓存缓存起来，默认 select 标签为 true。 |
| timeout       | 设置超时时间                                                 |
| fetchSize     | 设置预期返回的记录数量                                       |
| statementType | STATEMENT，PREPARED 或 CALLABLE 中的一个，默认为 PREPARED（预处理） |

> 将 select 查询写在 mapper.xml 文件中

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.imooc.mybatis.mapper.UserMapper">
  <select id="selectUserAgeById" parameterType="java.lang.Integer" resultType="java.lang.Integer">
    SELECT age FROM imooc_user WHERE id = #{id}
  </select>
</mapper>
```

</details>

<br>

## 🤪<font color="pink">resultMap标签</font>

> 首先要了解，一个 resultMap 中都有些什么常用的玩意：

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml

<resultMap id="唯一标识" type="映射的entity对象的绝对路径">
    <id column="表主键字段" jdbcType="字段类型" property="映射entity对象的主键属性" />
 
    <result column="表某个字段" jdbcType="字段类型" property="映射entity对象的某个属性"/>
 
    <!-- 指的是entity对象中的对象属性 -->
    <association property="entity中某个对象属性" javaType="这个对象的绝对路径">
        <id column="这个对象属性对应的表的主键字段" jdbcType="字段类型" property="这个对象属性内的主键属性"/>
        <result column="表某个字段" jdbcType="字段类型" property="这个对象属性内的某个属性"/>
    </association>
 
    <!-- 指的是entity对象中的集合属性 -->
    <collection property="entity中的某个集合属性" ofType="这个集合泛型所存实体类的绝对路径">
        <id column="这个集合属性中泛型所存实体类对象对应表的主键字段" jdbcType="字段类型"
            property="这个集合属性中泛型所存实体类对象的主键属性"/>

        <result column="表某个字段" jdbcType="字段类型" 
                property="这个集合属性泛型所存实体类对象的属性"/>  
    </collection>
 
    <!-- 引用另一个resultMap (套娃) -->
    <collection property="entity中的某个集合属性" 
                resultMap="这个引用的resultMap的type,就是这个集合属性泛型所存实体类的绝对路径"/>
</resultMap>

```

</details>

<br>

---

**注意：** 

> 一个<resultMap></resultMap>，就是对应着一个实体类。id就是他的名字，type就是他们的身体。实体类和对应表是一一对应的，实体类中每一个属性都对应着一个表中的字段。其中有个别属性上标明注解@TableField(exist = false)，其含义就是该属性在表中不存在，大多为引用的属性，引用的集合，或者是为了满足某种业务需求而声明的某种标志（比如sql多表查询时把某个表中的某个字段数据给另一个实体中某个属性赋值而起的别名）等等。

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

那么对于实体类中的各个属性：

```xml
主键id则建议使用 <id column="" ../>

一般的字段建议使用 <result column="" ../>

引用了另外某个实体类对象 <association ... />

引用了由某个实体类组成的集合 <collection ... />
```

</details>

<br>

## 🥲<font color="pink">sql标签</font>

> 在实际的开发中像SELECT * FROM user这样的代码段其实非常常见，会在多个 select 标签中用到它。我们可以将其定义为一个 sql 标签，这样所有的 select 标签都可以快速复用到这段代码。

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<sql id="selectUser">
  SELECT * FROM imooc_user
</sql>
```

</details>

<br>

> 同样的，我们必须为这个代码段定义一个唯一的 id，定义好后，我们就可以在其它标签中使用了：

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<select id="selectUserByAgeAndScore" parameterType="com.imooc.mybatis.model.User"
        resultMap="userMap">
  <include refid="selectUser"/>
  WHERE age = #{age} AND score = #{score}
</select>
```

</details>

> 我们必须使用一个 include 标签来将 SQL 标签包含进来，并且 refid 属性必须是该 SQL 标签的 id 值。没有注解

## 😚<font color="pink">trim标签的使用</font>

> mybatis的trim标签一般用于去除sql语句中多余的and关键字，逗号，或者给sql语句前拼接 “where“、“set“以及“values(“ 等前缀，或者添加“)“等后缀，可用于选择性插入、更新、删除或者条件查询等操作。

以下是trim标签中涉及到的属性：


|    **属性**     |                     给sql语句拼接的前缀                      |
| :-------------: | :----------------------------------------------------------: |
|     suffix      |                     给sql语句拼接的前缀                      |
|     suffix      |                     给sql语句拼接的后缀                      |
| prefixOverrides | 去除sql语句前面的关键字或者字符，该关键字或者字符由prefixOverrides属性指定，假设该属性指定为"AND"，当sql语句的开头为"AND"，trim标签将会去除该"AND" |
| suffixOverrides | 去除sql语句后面的关键字或者字符，该关键字或者字符由suffixOverrides属性指定 |

<br>

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml

<insert id="insertInformSc" parameterType="InformSc" useGeneratedKeys="true" keyProperty="id">
    insert into inform_sc
    <trim prefix="(" suffix=")" suffixOverrides=",">
        <if test="title != null">title,</if>
        <if test="mgr != null">mgr,</if>
        <if test="createMan != null">create_man,</if>
        <if test="img != null">img,</if>
        <if test="state != null">state,</if>
        </trim>
    <trim prefix="values (" suffix=")" suffixOverrides=",">
        <if test="title != null">#{title},</if>
        <if test="mgr != null">#{mgr},</if>
        <if test="createMan != null">#{createMan},</if>
        <if test="img != null">#{img},</if>
        <if test="state != null">#{state},</if>
        </trim>
</insert>

<update id="updateInformSc" parameterType="InformSc">
    update inform_sc
    <trim prefix="SET" suffixOverrides=",">
        <if test="title != null">title = #{title},</if>
        <if test="mgr != null">mgr = #{mgr},</if>
        <if test="createMan != null">create_man = #{createMan},</if>
        <if test="img != null">img = #{img},</if>
        <if test="state != null">state = #{state},</if>
    </trim>
    where id = #{id}
</update>

```

</details>

<br>

## 🤔<font color="pink">insert</font>

> 同 select 一样，每一个 insert 标签都必须有一个唯一的 id 和可选的 parameterType。标签里面则是真正的 SQL 语句，该语句共有 4 个参数，分别对应 User 类的四个属性。

insert 属性

其中常见且重要的属性如下表：

|       属性       |                             描述                             |
| :--------------: | :----------------------------------------------------------: |
|        id        |                   在命名空间中的唯一标识符                   |
|  parameterType   |         语句的参数类型，默认可选，MyBatis 会自动推断         |
|    flushCache    | 设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认为 false |
|     timeout      |                         设置超时时间                         |
|  statementType   | STATEMENT，PREPARED 或 CALLABLE 中的一个，默认为 PREPARED（预处理） |
| useGeneratedKeys | 取出由数据库自动生成的主键，仅对支持主键自动生成的数据库有效，默认为 false |
|   keyProperty    |   主键的名称，必须与useGeneratedKeys 一起使用，默认未设置    |

### 🫠自增主键

**xml 方式**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<insert id="insertUserNoId" useGeneratedKeys="true" keyProperty="id"
        parameterType="com.imooc.mybatis.model.User">
  INSERT INTO imooc_user(username,age,score) VALUES (#{username},#{age},#{score})
</insert>
```

</details>

<br>

> 在 insertUserNoId 中，我们并未添加上 id 参数，而是使用了数据库自增主键的特性，keyProperty 属性值对应 id 字段的名称，这样当语句执行成功后，对象的 id 字段会被自动设置为返回的 id 值。

**注解方式**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```java
@Insert("INSERT INTO imooc_user(username,age,score) VALUES (#{username},#{age},#{score})")
@Options(useGeneratedKeys = true, keyProperty = "id")
int insertUser(User user);
```

</details>

<br>

### 🤫selectKey 标签

**xml 方式**

> 如果使用的数据库不支持主键自增，如 Oracle，MyBatis 提供了 selectKey 标签来通过 SQL 语句获得主键。

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<insert id="insertUserNoId" parameterType="com.imooc.mybatis.model.User">
  INSERT INTO imooc_user(username,age,score) VALUES (#{username},#{age},#{score})
  <selectKey keyColumn="id" resultType="long" keyProperty="id" order="AFTER">
    SELECT LAST_INSERT_ID()
  </selectKey>
</insert>
```

</details>

<br>

> selectKey 标签必须在 insert 标签里面，selectKey 有 4 个属性，它们的作用如下表：

|    属性     |                         描述                          |
| :---------: | :---------------------------------------------------: |
|  keyColumn  |         数据库字段名，对应返回结果集中的名称          |
| keyProperty |          目标字段名称，对应Java 对象的字段名          |
| resultType  |                     id字段的类型                      |
|    order    | 执行的顺序，在 insert 之前调用为 BEFORE，之后为 AFTER |

> 注意，selectKey 中的语句其实就是 SQL 语句，不同数据库得到主键的语句均不一样。

**注解方式**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```java
@Insert("INSERT INTO imooc_user(username,age,score) VALUES (#{username},#{age},#{score})")
@SelectKey(statement = "SELECT LAST_INSERT_ID()", keyProperty = "id", before = false, resultType = Long.class)
int insertUser(User user);
```

</details>

<br>

## 🤔<font color="pink">update</font>

**xml 实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<update id="updateUserAgeById">
  UPDATE imooc_user SET age = #{age} WHERE id = #{id}
</update>
```

</details>

<br>

> 每一个 update 标签都必须有一个唯一的 id 属性，在 update 标签内部则是一条 SQL 语句。

**注解实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```java
@Update("UPDATE imooc_user SET age = #{age} WHERE id = #{id}")
int updateUserAgeById(@Param("age") Integer age, @Param("id") Integer id);
```

</details>

<br>

|     属性      |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|      id       |                   在命名空间中的唯一标识符                   |
| parameterType |         语句的参数类型，默认可选，MyBatis 会自动推断         |
|  flushCache   | 设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认为 false |
|    timeout    |                         设置超时时间                         |
| statementType | STATEMENT，PREPARED 或 CALLABLE 中的一个，默认为 PREPARED（预处理） |


**实践**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

首先，在 UserMapper.xml 中添加 update 标签，并在标签中写入 SQL ：

```xml
<update id="updateUserAgeById">
  UPDATE imooc_user SET age = #{age} WHERE id = #{id}
</update>
```

</details>

<br>

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

然后在 UserMapper.java 中添加上对应的接口方法，方法接受 age 和 id 两个参数。

```java
package com.imooc.mybatis.mapper;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface UserMapper {
  int updateUserAgeById(@Param("age") Integer age, @Param("id") Integer id);
}
```

</details>

<br>

> 注意：这里我们使用了@Param这个注解，由于在 update 标签中有两个参数 age 和 id，我们需要通过 @Param 来告诉 MyBatis 参数的对应关系。

## 😁<font color="pink">delete</font>

**xml 实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<delete id="deleteUserById">
  DELETE FROM imooc_user WHERE id = #{id}
</delete>
```

</details>

<br>

> 每一个 delete 标签都必须有一个唯一的 id 属性，在 delete 标签内部则是一条 SQL 语句。

**注解实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```java
@Delete("DELETE FROM imooc_user WHERE id = #{id}")
int deleteUserById(Integer id);
```

</details>

<br>

|     属性      |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|      id       |                   在命名空间中的唯一标识符                   |
| parameterType |         语句的参数类型，默认可选，MyBatis 会自动推断         |
|  flushCache   | 设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认为 false |
|    timeout    |                         设置超时时间                         |
| statementType | STATEMENT，PREPARED 或 CALLABLE 中的一个，默认为 PREPARED（预处理） |

## 🌊<font color="pink">OGNL 表达式</font>

常见的 OGNL 表达式

```
e1 or e2：或关系

e1 and e2：与关系

e1 == e2 或者 e1 eq e2：相等

e1 != e2 或者 e1 neq e2：不等

e1 lt e2 ；e1 < e2；e1 gt e2；e1 > e2；e1 lte e2；e1 <= e2；e1 gte e2；e1 >= e2：比较关系

e1 + e2；e1 - e2；e1 * e2；e1 / e2；e1 % e2：运算关系

!e 或者 not e：非，取反

e.method(args)：调用对象方法

e.property：访问属性值

e1[e2]：访问数组、链表（e2 为序号）或者 Map（e2 为键值）

```

OGNL 最常用于 if 标签中，用于判断条件是否满足，如下：

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<if test="age != null">
  AND age = #{age}
</if>
```

</details>

<br>

## 😎<font color="pink">if和多数据库支持</font>

> *if* 常用于 where 语句中，通过判断参数来决定在 select 语句中是否使用某个查询条件，或者在 update 语句中判断是否更新一个字段，还可以在 insert 语句中决定是否插入一个满足条件的值。

**实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<select id="selectUserByAgeAndScore" parameterType="com.imooc.mybatis.model.User" resultMap="userMap">
  SELECT * FROM imooc_user
  WHERE age = #{age}
  <if test="score != null">
    AND score = #{score}
  </if>
</select>
```

</details>

<br>

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

selectUserByAgeAndScore 的作用是通过用户年龄和积分来查询用户，当 score 值为 null 时，if 判断不成立，此时生成的 SQL 语句为：

```mysql
SELECT * FROM imooc_user WHERE age = ?
```

而当 if 成立时，生成的 SQL 语句为：

```mysql
SELECT * FROM imooc_user WHERE age = ? AND score = ?
```

</details>

<br>

### 🥴多数据库支持

> MyBatis 可以根据不同的数据库厂商执行不同的语句
> 
>MyBatis 的全局配置文件中添加如下配置：

> 在 configuration 中加入 databaseIdProvider 后，还需要在 databaseIdProvider 标签中添加上需要使用到的数据库名称，如：SQL Server。每一个 property 属性都代表了一个数据库，name 表示数据库厂商名称，value 用来设置别名。

**配置**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <databaseIdProvider type="DB_VENDOR">
    <property name="SQL Server" value="sqlserver"/>
    <property name="MySQL" value="mysql"/>
    <property name="PostgreSQL" value="postgre"/>
  </databaseIdProvider>
</configuration>
```

</details>

<br>

**使用**

配置完毕后，我们就可以在 mapper xml 文件中，通过 if 来判断当前的数据库厂商，从而动态生成不同数据库的 SQL。

例如，PostgreSQL 支持使用 ||来拼接字符串，而 MySQL 需要使用 concat函数来拼接字符串。

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<select id="selectUserByLikeName" resultType="com.imooc.mybatis.model.User">
  SELECT * FROM imooc_user
  WHERE username LIKE
  <if test="_databaseId == 'mysql'">
    CONCAT('%',#{username},'%')
  </if>
  <if test="_databaseId == 'postgre'">
    '%' || #{username} || '%'
  </if>
</select>
```

</details>

<br>

> 注意，这里 _databaseId 参数是由 MyBatis 提供的内置参数，对应于 databaseIdProvider 中配置的数据库名称。
> 
> 通过 databaseIdProvider 配置，即时数据库厂商之间存在差异，但仍然可以通过动态 SQL 的形式来支持多数据库。

---

## 🍎<font color="pink">choose 和 bind</font>

> choose 标签相当于编程语言 if…else 语句，用于动态 SQL 中的多条件判断，是 if 标签的增强版。
> 
> bind 标签可以在动态 SQL 中通过 OGNL 表达式来创建一个新的变量绑定到上下文中，供后续的 SQL 使用。

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<select id="selectUserByLikeName" resultType="com.imooc.mybatis.model.User">
  SELECT * FROM imooc_user
  WHERE username LIKE
  <choose>
    <when test="_databaseId == 'mysql'">
      CONCAT('%',#{username},'%')
    </when>
    <when test="_databaseId == 'postgre'">
      '%' || #{username} || '%'
    </when>
    <otherwise>
      <bind name="usernameLike" value="'%' + username + '%'"/>
      #{usernameLike}
    </otherwise>
  </choose>
</select>
```

</details>

<br>

> 在这个例子中，choose 是一个条件选择标签，第一个 when 相当于 if 判断，第二个 when 相当于 else if，最后的 otherwise 相当于 else。比起 if 标签，choose 标签无疑更为易用，适用于同一条件的多次判断逻辑。

## 😂<font color="pink">where、set标签</font>

> where、set、trim 三个标签都是为了解决 MyBatis 在动态生成 SQL 时，产生了多余的前缀和后缀的问题。

**where 实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<select id="selectUserByIdAndName" resultType="com.imooc.mybatis.model.User">
  SELECT * FROM imooc_user
  <where>
    <if test="id != null">
      id = #{id}
    </if>
    <if test="username != null">
      AND username = #{username}
    </if>
  </where>
</select>
```

</details>

<br>

- 在 id 和 username 都为空的情况下，where 标签不会产生任何 SQL 代码段，最后的 SQL 语句为：SELECT * FROM imooc_user；

- 在 id 不为空，username 为空的情况下，或者在 id 不为空，username 也不为空的情况下，where 标签会自动给 SQL 代码段添加上 WHERE 前缀；

- 在 id 为空，username 不为空的情况下，where 标签会自定去除 AND 前缀，此时生成的 SQL 语句为： SELECT * FROM imooc_user WHERE username = ?。

**set 实例**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

```xml
<update id="updateUsernameAndScoreById">
  UPDATE imooc_user
  <set>
    <if test="username != null">
      username = #{username},
    </if>
    id = #{id}
  </set>
  WHERE id = #{id}
</update>
```

</details>

<br>

> set 标签会自动在 SQL 语句中添加上相应的 SET 前缀，并根据里面的条件动态地添加相应的字段。
> 
> 由于 SQL update 的语法问题，若 set 标签里面条件均不满足，此时 set 标签也不会添加上 SET 前缀，但此时 SQL 会报语法错误，所以 set 标签中还是得有一个必然存在的赋值。

## 🍞<font color="pink">foreach标签</font>

> foreach 标签用来遍历数组、列表和 Map 等集合参数，常与 in 关键字搭配使用。

```
foreach 标签共有 6 个属性，它们的作用分别为：

collection： 被遍历集合参数的名称，如 list；

open： 遍历开始时插入到 SQL 中的字符串，如 ( ；

close： 遍历结束时插入到 SQL 中的字符串，如 ) ；

separator： 分割符，在每个元素的后面都会插入分割符；

item： 元素值，遍历集合时元素的值；

index： 元素序列，遍历集合时元素的序列。
```

**遍历列表**

<details>
  <summary><font size="4" color="orange">展开代码</font></summary>

xml：

```xml
<select id="selectUserInIds" resultType="com.imooc.mybatis.model.User">
  SELECT * FROM imooc_user
  WHERE id IN
  <foreach collection="list" open="(" close=")" separator="," item="item" index="index">
    #{item}
  </foreach>
</select>
```

java:

```java
List<User> selectUserInIds(List<Integer> ids);
```

</details>

<br>

> foreach 标签的 collection 属性在接受参数名有两种情况：
> 
> 一、匿名参数，当在 java 方法中没有通过 @Param 注解指定参数名时，列表类型的使用默认参数名 list。
> 
> 二、具名参数，java 方法中使用了@Param 注解指定了参数名称，则 foreach 中的 collection 属性必须为参数名

```java
List<User> selectUserInIds(@Param("ids") List<Integer> ids);
```

```xml
<foreach collection="ids" open="(" close=")" separator="," item="item" index="index">
  #{item}
</foreach>
```

**遍历数组**

当 Java 方法使用的参数类型为数组时

```java
List<User> selectUserInIds(Integer[] ids);
```

> 如果 ids 参数使用 @Param 注解指定了参数名称，则 foreach 标签中的 collection 属性必须为该名称；但若未指定名称，则在 foreach 标签中使用默认数组名称 array，

```xml
<select id="selectUserInIds" resultType="com.imooc.mybatis.model.User">
  SELECT * FROM imooc_user
  WHERE id IN
  <foreach collection="array" open="(" close=")" separator="," item="item" index="index">
    #{item}
  </foreach>
</select>
```

**遍历 Map**

当 Java 方法使用的参数类型为 Map 时

```java
int updateUserById(@Param("params") Map map, @Param("id") Integer id);
```

> 使用 foreach 标签遍历 Map 时，collection 属性值为注解指定的参数名，即 params，且 item 是 Map 的键值，index 是键名。

```xml
<update id="updateUserById">
  UPDATE imooc_user
  SET
  <foreach collection="params" item="val" index="key" separator=",">
    ${key} = #{val}
  </foreach>
  WHERE id = #{id}
</update>
```

> 注意： 由于 key 是字段名称，因此不能使用#{}作为占位符，只能使用${}在字符串中替换。

updateUserById 生成的 SQL 语句大致如下：

```mysql
UPDATE imooc_user SET score = ? , age = ? WHERE id = ? 
```
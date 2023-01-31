# JPA

jpa是一个全ORM的框架。是基于Hibernate 的框架，进行了封装，由spring生成的一个框架

## 使用jpa

> 1.使用JPA可以直接创建数据库中的表
>
> 2.当列明改变后，自动更新到表中
>
> 3.提供了java的接口，快速完成crud的操作
>
> 4.提供了注解的方式，完成sql的手动编写

1.添加依赖

```
		 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
```

2.配置文件

```properties
#jpa的配置，告诉jpa使用的是那个数据库
spring.jpa.database=mysql
#告诉Jpa是否自动生成表,当字段更新时，会自动更新到数据库
spring.jpa.generate-ddl=true
#是否打印sql语句
spring.jpa.show-sql=true

```

3.编写实体类

```java
package com.qf.pojo;

import lombok.Data;

import javax.persistence.*;
import java.util.Date;

@Data
//标识该类为jpa的实体类对象
@Entity
//标识当前类关联的表是那张表,默认当前类名小写与数据库中的表名对应.如果不一致，加入name属性，标识表的名称
@Table(name = "tb_games")
public class Games {
    //标识当前属性为数据库库中的主键
    @Id
    //主键使用自增的策略
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    //将属性名与表中的列明进行对应
    @Column(name = "game_name")
    private String gameName;
    @Column(name = "create_time")
    private Date creatTime;
    @Column(name = "game_type")
    private Integer type;

    private Double money;
}

```

# 单表查询用JPA，多表查询用MyBatis！！！


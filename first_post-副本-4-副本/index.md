# SpringBoot集成flyway


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

### SpringBoot集成flyway

在 `pom.xml` 加入如下依赖

```xml
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-mysql</artifactId>
        </dependency>
```

在`application.yml`中加入配置

```java
  flyway:
    #是否开启flyway，默认true
    enabled: false
    #当迁移时发现目标schema非空，而且没有元数据的表时，（即迭代中项目）是否自动执行基准迁移，默认false.
    baseline-on-migrate: true
    # 是否允许无序运行迁移, 默认false，建议开发环境开启，生成环境关闭
    out-of-order: true
    #设定SQL脚本的目录,可以配置多个，比如为classpath:db/migration,filesystem:/sql-migrations,默认classpath:db/migration
    locations:
      - classpath:db/migration
```

sql文件命名规范

- Prefix 前缀：V 代表版本迁移，U 代表撤销迁移，R 代表可重复迁移
- Version 版本号：版本号通常 `.` 和整数组成
- Separator 分隔符：固定由两个下划线 `__` 组成
- Description 描述：由下划线分隔的单词组成，用于描述本次迁移的目的
- Suffix 后缀：如果是 SQL 文件那么固定由 `.sql` 组成，如果是基于 Java 类则默认不需要后缀

例如`V1.0__init_db.sql`

执行了 `V1.0__init_db.sql` 脚本后，从而创建了 `user` 表，另外还自动创建了 `flyway_schema_history` 表，用于记录所有版本演化和状态，其表结构如下(以 MySQL 为例)：

![](/images/1701162964333.png)



注意



```vbnet
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'flywayInitializer' defined in class path resource [org/springframework/boot/autoconfigure/flyway/FlywayAutoConfiguration$FlywayConfiguration.class]: Invocation of init method failed; nested exception is org.flywaydb.core.api.FlywayException: Validate failed: Migration checksum mismatch for migration version 1.0
-> Applied to database : 1317299633
-> Resolved locally    : -1582367361
```

这个错误的原因就是 Flyway 会给脚本计算一个 checksum 保存在数据库中，用于在之后运行过程中对比 sql 文件是否有变化，如果发生了变化，则会报错，也就防止了误修改脚本导致发生问题。



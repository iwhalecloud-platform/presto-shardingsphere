# presto-shardingsphere

基于Presto插件框架实现的ShardingSphere连接器

## 简介

Presto-ShardingSphere是一款可实现对Apache ShardingSphere进行访问的Presto插件，用于在使用Apache ShardingSphere做数据分片而又需要进行跨库查询的运维管理场景。

## 关于Presto-ShardingSphere

随着互联网的高速发展，传统集中式数据存储越来越难以满足业务数据的急剧增长和峰值访问，不少企业已选用Apache ShardingSphere来实现数据库服务能力的水平扩展。然而，当前的Apache ShardingSphere主要擅长于OLTP场景，对于一些OLAP场景就显得不足，尤其是分库分表后的跨库关联查询就更难满足，为此，我们引入Presto，基于其插件机制实现了Presto-ShardingSphere，从而解决了上述问题。

## 可以做什么？

**Presto-ShardingSphere**插件主要用于使用Apache ShardingSphere后日常运维管理中的一些实时查询场景，包括：

- **复杂SQL查询**：支持一些Apache ShardingSphere不支持的且较复杂的SQL语句，如：多层嵌套查询，复杂的统计查询等；
- **跨库SQL查询**：可以支持同数据源下的单片表与分片表的关联查询，也支持不同数据源下的多表关联查询。

## 如何使用？

### 1. 构建生成此插件

可根据实际使用的shardingsphere和presto版本，修改pom中的版本属性：

```xml
<shardingsphere.version>5.2.0</shardingsphere.version>
<presto.version>0.275</presto.version>
```

然后，执行mvn package命令，得到target/shardingsphere即为此插件。

### 2. 安装配置插件

将生成的插件上传至presto的plugin目录下（Presto的安装可参考其官网https://prestodb.io/docs/current/installation/deployment.html）。

在presto的etc/catalog下新增shardingsphere.properties，其内容示例如下：

```properties
connector.name=shardingsphere
shardingsphere-rule-location=etc/sharding-test.yaml
```

其中shardingsphere-rule-location为连接Apache ShardingSphere数据源的配置文件。

配置好后，就可以启动Presto了。

### 3. 连接Presto执行SQL

可通过Presto的命令行接口去执行SQL，也可以用一些图形化界面工具去连接执行，如DBeaver。


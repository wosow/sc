# 创建分析型数据库MySQL版3.0结果表

本文为您介绍如何创建实时计算Flink版分析型数据库MySQL版3.0版本的结果表，以及创建过程中涉及的WITH参数。

**说明：**

-   分析型数据库MySQL版3.0结果表暂不支持注册存储功能。
-   本文仅适用于`Blink 3.3.0`及以上版本。
-   分析型数据库MySQL版2.0版本结果表创建说明，请参见[创建分析型数据库MySQL版2.0结果表](/intl.zh-CN/独享模式/Flink SQL参考/DDL语句/创建数据结果表/创建分析型数据库MySQL版2.0结果表.md)。

## DDL定义

实时计算Flink版支持使用分析型数据库MySQL版3.0版本作为结果表输出。示例代码如下。

```
CREATE TABLE rds_output (
id INT,
len INT,
content VARCHAR，
PRIMARY KEY(id,len)
) WITH (
type='ADB30',
url='jdbc:mysql://<yourNetworkAddress>:<PortId>/<yourDatabaseName>',
tableName='<yourDatabaseTableName>',
userName='<yourDatabaseUserName>',
password='<yourDatabasePassword>'
);
```

## 实现原理

实时计算Flink版写入分析型数据库MySQL版3.0版本结果表可以分为以下两个阶段：

1.  将实时计算Flink版每行的结果数据拼接为一行SQL。
2.  在目标数据库执行拼接后的SQL。

## WITH参数

|参数|注释说明|是否必选|备注|
|--|----|----|--|
|type|connector类型|是|固定值为`ADB30`。|
|url|jdbc连接地址|是|分析型数据库MySQL版数据库地址。示例：`url='jdbc:mysql://databaseName****-cn-shenzhen-a.ads.aliyuncs.com:10014/databaseName'`。 **说明：**

-   分析型数据库MySQL版数据库连接信息，请参见[URL地址查询](/intl.zh-CN/独享模式/Flink SQL开发指南/数据存储/注册数据存储/注册分析型数据库MySQL版.md)。
-   databaseName为分析型数据库MySQL版数据库名称。 |
|tableName|表名|是|无|
|username|账号|是|无|
|password|密码|是|无|
|maxRetryTimes|写入重试次数|否|默认值为3。|
|bufferSize|流入多少条数据后开始去重|否|默认值为1000，表示输入的数据达到1000条则开始输出。 **说明：** 需要指定主键后才能生效。 |
|batchSize|一次批量写入的条数|否|默认值为1000。 **说明：** 需要指定主键后才能生效。 |
|flushIntervalMs|清空缓存的时间间隔|否|单位为毫秒，默认值为3000。表示如果缓存中的数据在等待3秒后，依然没有达到输出条件，系统会自动输出缓存中的所有数据。|
|ignoreDelete|是否忽略DELETE操作|否|默认值为false，表示支持DELETE功能。|
|reserveMilliSecond|TimeStamp类型是否保留毫秒|否|默认值为false，不保留毫秒数值。|


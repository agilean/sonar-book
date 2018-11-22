# Code Review 检查清单

## 前言

###术语

API，特指面向服务架构中一项服务向下游提供的开放式接口，例如REST，ESB，SOAP

interface，指代码中编译时依赖的接口，例如java.util.List

### 目录

文档分成两部分

第一部分是功能实现检查项，确保代码能够实现业务需求，能够上线服务。

第二部分是常见的代码味道

##  文档完备

### API文档

API文档是否已经发布在公共的文档库中

RAP，WIKI等等

### Javadoc 

- class，interface 注明设计意图 
- 有@author

### 文档链接

- 版权声明
- 引用需求文档URL
- 引用Bug tracker

### 测试用例

关键算法的单元测试用例（用作实例化需求文档）

## Style检查

在代码review之前，个人需要自查，保证代码符合 style guide 规范。

例如：

- 缩进
- 断行
- Javadoc，Comment

提示：代码评审会上关注的是实质问题。style不是重点。

### 起名容易理解

class、field、method，param 起名符合词汇表中的要求

没有怪异的拼音串

### 需要删除的被注释的代码



## 实现逻辑

### 流程按照分层的要求

### 代码块顺序合理，容易理解业务流程

if else ，switch case，try catch，嵌套合理，分支没有遗漏

### 配置和开关

引用公共配置

业务（功能）开关

### 公共

- 优先使用公共library的方法，没有自己造轮子
- 复用了方法，没有复制


### 同步异步

## 功能入口(Controller)

- 对传入的参数进行校验，返回准确的错误信息或者代码

## 主要逻辑（Service）

- 封装
- 测试

## 数据存储(DAO,SQL)

- 缓存
- SQL
- 索引

## 领域模型

- Bean字段设计
- 方法封装

## Exception处理和message

- 日志消息准确，有上下文参数
- 使用异常，不使用返回值判断
- NPE检查
- logger.info ,warn,error 归类
- 重试机制

## 性能要求

- 并发限制
- SQL索引，分区表，序列容量
- 死循环
- 异步
- 缓存
- 缓存刷新
- RPC超时
- batch
- try catch finally

## 易修改性要求

- 逻辑分块合理
- 代码分层，定位
- 大方法重构
- 方法入参，出参紧密
- 容易使用Junit 测试
- MOCK环境，测试案例
- API测试案例
- 使用统一的词汇表

## FailSafe

- API幂等性
- RPC超时重试
- transaction 回滚

## 易维护性要求

- 监控日志
- 业务开关
- 异常降级
- 日志准确描述错误
- 配置项
- 错误码
- logger message

## 安全性要求

- logger message没有泄露密码，卡号……
- SQL注入，SQL参数化
- 入参字段检查
- 签名，key

## 生产准备，风险识别

- 开墙
- 流量
- 压力测试
- 回滚方案
- SQL
- 单点故障
- 超时


# SONAR检查项

## boolean方法

## void方法

## HashMap

## 好几个参数一样的method

## JSON处理

## 未删除的注释

## 重复的方法（复制）

## 使用jdbctemplate

## 使用sql 参数占位符

## 在循环中使用sql batch


## 使用Closer来关闭资源

  `org.apache.commons.io.IOUtils.closeQuietly(Closeable)`
  `org.springframework.jdbc.support.JdbcUtils.closeConnection(Connection)`
  `com.google.common.io.Closeables.close(Closeable, boolean)`
  `com.google.common.io.Closeables.closeQuietly(InputStream)`
  `com.google.common.io.Closer`
  
    Closer closer = Closer.create();
     try {
       InputStream in = closer.register(openInputStream());
       OutputStream out = closer.register(openOutputStream());
       // do stuff
     } catch (Throwable e) {
       // ensure that any checked exception types other than IOException that could be thrown are
       // provided here, e.g. throw closer.rethrow(e, CheckedException.class);
       throw closer.rethrow(e);
     } finally {
       closer.close();
     }}
     
    public static void writeToFileZipFileContents(String zipFileName,
                                               String outputFileName)
                                               throws java.io.IOException {
        java.nio.charset.Charset charset =
             java.nio.charset.StandardCharsets.US_ASCII;
        java.nio.file.Path outputFilePath =
             java.nio.file.Paths.get(outputFileName);
    
        try (
            java.util.zip.ZipFile zf =
                 new java.util.zip.ZipFile(zipFileName);
            java.io.BufferedWriter writer = 
                java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
        ) {
            // Enumerate each entry
            for (java.util.Enumeration entries =
                                    zf.entries(); entries.hasMoreElements();) {
                // Get the entry name and write it to the output file
                String newLine = System.getProperty("line.separator");
                String zipEntryName =
                     ((java.util.zip.ZipEntry)entries.nextElement()).getName() +
                     newLine;
                writer.write(zipEntryName, 0, zipEntryName.length());
            }
        }
    }
    
## 使用NPE检查工具    


## 使用JdbcTemplate
    
  

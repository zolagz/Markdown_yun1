# Log4j配置文件


log4j.properties

```
log4j.rootLogger=info,stdout,R 
log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
log4j.appender.stdout.layout.ConversionPattern=%5p - %m%n 
log4j.appender.R=org.apache.log4j.RollingFileAppender 
log4j.appender.R.File=mapreduce_test.log 
log4j.appender.R.MaxFileSize=1MB 
log4j.appender.R.MaxBackupIndex=1 
log4j.appender.R.layout=org.apache.log4j.PatternLayout 
log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n 
log4j.logger.com.codefutures=DEBUG
```

<!--
create time: 2018-03-02 21:11:13
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


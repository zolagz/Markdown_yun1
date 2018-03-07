# Hive自定义函数

新建一个Maven工程；引入pom依赖

建立一个类，继承UDF，并重载 evaluate 方法；



pom依赖：

```
    <dependencies>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>2.7.4</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.2</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <filters>
                                <filter>
                                    <artifact>*:*</artifact>
                                    <excludes>
                                        <exclude>META-INF/*.SF</exclude>
                                        <exclude>META-INF/*.DSA</exclude>
                                        <exclude>META-INF/*.RSA</exclude>
                                    </excludes>
                                </filter>
                            </filters>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

```

写一个java类：

```

import org.apache.hadoop.hive.ql.exec.UDF;
import org.apache.hadoop.io.Text;

public class HelloUDF extends UDF{

 <!--由于hive最终会将这个类转化为mapreduce程序，所以在这里传入的数据类型必须是Hadoop能够接收的类型-->
 
 <!--这里不能是String，因为String类型不能在hadoop集群间进行通讯-->

    public Text evaluate(Text input){

        if (input == null) {return  null;}

        return new Text(input.toString().toLowerCase());
    }
    
    public Text evaluate(Text input1,Text input2){

        return new  Text(input1.toString() + " ** AND ** " + input2.toString());

    }
}

```

打成jar包，上传到服务器

add jar /jar文件路径

```
hive> add jar /root/guan_soft/myText-1.0.jar;

Added [/root/guan_soft/myText-1.0.jar] to class path

Added resources: [/root/guan_soft/myText-1.0.jar]

hive> create temporary function myfunc as 'com.itcast.dm.HelloUDF';

OK
Time taken: 0.871 seconds

```

创建临时函数与开发好的 java class 关联

create temporary function 自定义函数名 as '自定义类的全路径';

```
create temporary function myfunc as 'com.itcast.dm.HelloUDF';

```
然后通过；show functions; 可以查到到导入的自定义函数；

测试函数：

```
hive> select myfunc('Hello','China');

OK

Hello ** AND ** China

Time taken: 0.162 seconds, Fetched: 1 row(s)

hive> 

```



<!--
create time: 2018-03-06 19:50:46
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


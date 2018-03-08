# MapReduce入门1

day04 wordCount案例

1.需求 
	将流量汇总统计结果按照手机归属地不同省份输出到不同文件中
	

pom依赖

```
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>2.7.4</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-hdfs</artifactId>
            <version>2.7.4</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.4</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-mapreduce-client-core</artifactId>
            <version>2.7.4</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <classpathPrefix>lib/</classpathPrefix>
                            <mainClass>cn.itcast.hadoop.mr.WordCountDriver</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

```

新建WordCountMapper文件

```
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

/**
 * Created by Itcast .
 *
 * 这里就是mapreduce程序  mapper阶段业务逻辑实现的类
 *
 * Mapper<KEYIN, VALUEIN, KEYOUT, VALUEOUT>
 *
 * KEYIN：表示mapper数据输入的时候key的数据类型，在默认的读取数据组件下，叫InputFormat,它的行为是一行一行的读取待处理的数据
 *        读取一行，返回一行给我们的mr程序，这种情况下  keyin就表示每一行的起始偏移量  因此数据类型是Long
 *
 * VALUEIN:表述mapper数据输入的时候value的数据类型，在默认的读取数据组件下 valuein就表示读取的这一行内容  因此数据类型是String
 *
 * KEYOUT 表示mapper数据输出的时候key的数据类型  在本案例当中 输出的key是单词  因此数据类型是 String
 *
 * VALUEOUT表示mapper数据输出的时候value的数据类型  在本案例当中 输出的key是单词的次数  因此数据类型是 Integer
 *
 * 这里所说的数据类型String Long都是jdk自带的类型   在序列化的时候  效率低下 因此hadoop自己封装一套数据类型
 *   long---->LongWritable
 *   String-->Text
 *   Integer--->Intwritable
 *   null-->NullWritable
 *
 *
 */
public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable>{

    /**
     *  这里就是mapper阶段具体的业务逻辑实现方法  该方法的调用取决于读取数据的组件有没有给mr传入数据
     *  如果有的话  每传入一个《k,v》对  该方法就会被调用一次
     * @param key
     * @param value
     * @param context
     * @throws IOException
     * @throws InterruptedException
     */
    @Override
    protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        
        //拿到传入进来的一行内容，把数据类型转化为String 
        String line = value.toString();

        //将这一行内容按照分隔符进行一行内容的切割 切割成一个单词数组
        String[] words = line.split(" ");

        //遍历数组，每出现一个单词  就标记一个数字1  <单词，1>
        for (String word : words) {
            //使用mr程序的上下文context 把mapper阶段处理的数据发送出去
            //作为reduce节点的输入数据
            context.write(new Text(word),new IntWritable(1));
            //hadoop hadoop spark -->   <hadoop,1><hadoop,1><spark,1>
        }
    }
}

```

新建WordCountReducer文件

```
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

/**
 * Created by Itcast
 *
 * 这里是MR程序 reducer阶段处理的类
 *
 * KEYIN：就是reducer阶段输入的数据key类型，对应mapper的输出key类型  在本案例中  就是单词  Text
 *
 * VALUEIN就是reducer阶段输入的数据value类型，对应mapper的输出value类型  在本案例中  就是单词次数  IntWritable
 * .
 * KEYOUT就是reducer阶段输出的数据key类型 在本案例中  就是单词  Text
 *
 * VALUEOUTreducer阶段输出的数据value类型 在本案例中  就是单词的总次数  IntWritable
 */
public class WordCountReducer extends Reducer<Text,IntWritable,Text,IntWritable> {

    /**
     * 这里是reduce阶段具体业务类的实现方法
     * @param key
     * @param values
     * @param context
     * @throws IOException
     * @throws InterruptedException
     *
     * reduce接收所有来自map阶段处理的数据之后，按照key的字典序进行排序
     * <hello,1><hadoop,1><spark,1><hadoop,1>
     * 排序后：
     * <hadoop,1><hadoop,1><hello,1><spark,1>
     *
     *按照key是否相同作为一组去调用reduce方法
     * 本方法的key就是这一组相同kv对的共同key
     * 把这一组所有的v作为一个迭代器传入我们的reduce方法
     *
     * <hadoop,[1,1]>
     */
    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        //定义一个计数器
        int count = 0;
        //遍历一组迭代器，把每一个数量1累加起来就构成了单词的总次数

        for(IntWritable value:values){
            count +=value.get();
        }

        //把最终的结果输出
        context.write(key,new IntWritable(count));
    }
}

```

新建WordCountDriver文件

```
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

/**
 * Created by Itcast .
 *
 * 这个类就是mr程序运行时候的主类，本类中组装了一些程序运行时候所需要的信息
 * 比如：使用的是那个Mapper类  那个Reducer类  输入数据在那 输出数据在什么地方
 */
public class WordCountDriver {
    public static void main(String[] args) throws Exception{
        //通过Job来封装本次mr的相关信息
        Configuration conf = new Configuration();
//        conf.set("mapreduce.framework.name","local");
        Job job = Job.getInstance(conf);

        //指定本次mr job jar包运行主类
        job.setJarByClass(WordCountDriver.class);

        //指定本次mr 所用的mapper reducer类分别是什么
        job.setMapperClass(WordCountMapper.class);
        job.setReducerClass(WordCountReducer.class);

        //指定本次mr mapper阶段的输出  k  v类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        //指定本次mr 最终输出的 k v类型
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

//        job.setNumReduceTasks(3);
        //如果业务有需求，就可以设置combiner组件
        job.setCombinerClass(WordCountReducer.class);


        //指定本次mr 输入的数据路径 和最终输出结果存放在什么位置
        FileInputFormat.setInputPaths(job,"/Users/alfred/Desktop/mydir/wordCount/input");
        FileOutputFormat.setOutputPath(job,new Path("/Users/alfred/Desktop/mydir/wordCount/output"));


//        job.submit();
        //提交程序  并且监控打印程序执行情况
        boolean b = job.waitForCompletion(true);
        System.exit(b?0:1);
    }
}

```


<!--
create time: 2018-02-28 22:02:12
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


# 案例-MapReduce数据排序

##问题描述

对输入文件中数据进行排序。输入文件中的每行内容均为一个数字，即一个数据。要求在输出中每行有两个间隔的数字，其中，第一个代表原始数据在原始数据集中的位次，第二个代表原始数据。

样例输入：

file1

```
    2

    32

    654

    32

    15

    756

    65223
```

file2

```
    5956

    22

    650

    92
```

file3

```
    5956

    22

    650

    92
```


样例输出：

```
    1    2

    2    6

    3    15

    4    22

    5    26

    6    32

    7    32

    8    54

    9    92

    10    650

    11    654

    12    756

    13    5956

    14    65223

```

###三、设计思路

这个实例仅仅要求对输入数据进行排序，熟悉MapReduce过程的读者会很快想到在MapReduce过程中就有排序，是否可以利用这个默认的排序，而不需要自己再实现具体的排序呢？答案是肯定的。

但是在使用之前首先需要了解它的默认排序规则。它是按照key值进行排序的，如果key为封装int的IntWritable类型，那么MapReduce按照数字大小对key排序，如果key为封装为String的Text类型，那么MapReduce按照字典顺序对字符串排序。

了解了这个细节，我们就知道应该使用封装int的IntWritable型数据结构了。也就是在map中将读入的数据转化成IntWritable型，然后作为key值输出（value任意）。reduce拿到<key，value-list>之后，将输入的key作为value输出，并根据value-list中元素的个数决定输出的次数。输出的key（即代码中的linenum）是一个全局变量，它统计当前key的位次。需要注意的是这个程序中没有配置Combiner，也就是在MapReduce过程中不使用Combiner。这主要是因为使用map和reduce就已经能够完成任务了。


###程序代码


SortMapper

```
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

public class SortMapper extends Mapper <Object ,Text,IntWritable,IntWritable>{

 //map将输入中的value化成IntWritable类型，作为输出的key
 
    private static  IntWritable data = new IntWritable();

    @Override
    protected void map(Object key, Text value, Context context) throws IOException, InterruptedException {
        String line = value.toString().trim();
        if (!line.equals("")){
            data.set(Integer.parseInt(line));
            context.write(data,new IntWritable(1));
        }
    }
}

```


```
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.mapreduce.Reducer;
import java.io.IOException;
public class SortReducer extends 
Reducer<IntWritable,IntWritable,IntWritable,IntWritable> {
 //reduce将输入中的key复制到输出数据的key上，
    //然后根据输入的value-list中元素的个数决定key的输出次数
    //用全局linenum来代表key的位次
private static IntWritable linenum = new IntWritable(1);


//实现reduce函数
    @Override
    protected void reduce(IntWritable key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        for (IntWritable val:values) {
            context.write(linenum,key);
            linenum = new IntWritable(linenum.get()+1);
        }
    }
}

```


```


import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;


public class SortExcuter {


    public static void main(String[] args) throws Exception {

        Configuration conf = new Configuration();
        Job wcjob = Job.getInstance(conf);
        wcjob.setJarByClass(SortExcuter.class);
        wcjob.setMapperClass(SortMapper.class);
        wcjob.setReducerClass(SortReducer.class);

//设置输出类型
        wcjob.setOutputKeyClass(IntWritable.class);
        wcjob.setOutputValueClass(IntWritable.class);

//设置输入和输出目录
        FileInputFormat.setInputPaths(wcjob,"/Users/alfred/Desktop/mydir/myfile");
        FileOutputFormat.setOutputPath(wcjob, new Path("/Users/alfred/Desktop/mydir/out1"));
        wcjob.waitForCompletion(true);
    }
}
```

[参考链接](http://www.aboutyun.com/forum.php?mod=viewthread&tid=7046&extra=page%3D1)

<!--
create time: 2018-03-02 21:13:55
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


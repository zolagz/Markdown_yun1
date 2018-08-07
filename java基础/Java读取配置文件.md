# Java读取配置文件


![](http://p2ehgqigv.bkt.clouddn.com/18-8-6/5987086.jpg)

```

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class Hello {

    //读取配置文件信息
    private static InputStream inputStream = null;
    private static Properties properties = null;
    private static String server;
    private static String code;
    private static String address;
    private static String protocol;
    static {
        try{
            properties = new Properties();
            inputStream = Thread.class.getResourceAsStream("/user.properties");
            properties.load(inputStream);
            server = properties.getProperty("server");
            code = properties.getProperty("code");
            protocol = properties.getProperty("protocol");
            address = properties.getProperty("address");
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            if(inputStream != null){
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public static void main(String[] args) throws  Exception{
        System.out.println(properties.getProperty("server"));
        System.out.println(properties.getProperty("address"));
        System.out.println(properties.getProperty("code"));
    }
}

```

<!--
create time: 2018-08-06 13:20:55
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


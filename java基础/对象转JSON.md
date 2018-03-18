# 对象转JSON


###Gson操作方式

pom依赖

```
        <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.1</version>
        </dependency>


```
1. Object与Json的互相转换

```
        Person p1 = new Person();
        p1.setName("张三");
        p1.setAddress("北京中关村");
        p1.setAge(20);
        p1.setMobile("4567890");


        Gson gson = new Gson();

		<!--对象生成JSON-->
        String json = gson.toJson(p1);
        System.out.println(json);

		<!-- JSON 转对象 -->
        Person p2 = gson.fromJson(json,Person.class);
        System.out.println(p2.toString());
```


2. 集合泛型与Json的互相转换

```
        Gson gson = new Gson();
        Person p2 = new Person("李四","上海111","hhh",23);
        ArrayList<Person> list = new ArrayList<Person>();
        list.add(p2);
        list.add(p2);
        list.add(p2);
        
        String json = gson.toJson(list);
        System.out.println(json);
        System.out.println("---------");

        Type type = new TypeToken<ArrayList<Person>>() {
        }.getType();
        ArrayList<Person> alist = gson.fromJson(json,type);
        System.out.println(alist);
```

使用Gson.toJson()方法,以把任何对象或者是集合放进去,完美的转换成json格式。

如果想把json格式的数据转换成我们想要的对象，我们只需要调用Gson.fromJson()即可。


###Jackson的使用


* JAVA对象转JSON[JSON序列化]

```
import java.io.IOException; 
import java.text.ParseException; 
import java.text.SimpleDateFormat; 
  
import com.fasterxml.jackson.databind.ObjectMapper; 
  
public class JacksonDemo { 
  public static void main(String[] args) throws ParseException, IOException { 
    User user = new User(); 
    user.setName("小民");  
    user.setEmail("xiaomin@sina.com"); 
    user.setAge(20); 
      
    SimpleDateFormat dateformat = new SimpleDateFormat("yyyy-MM-dd"); 
    user.setBirthday(dateformat.parse("1996-10-01"));     
      
    /** 
     * ObjectMapper是JSON操作的核心，Jackson的所有JSON操作都是在ObjectMapper中实现。 
     * ObjectMapper有多个JSON序列化的方法，可以把JSON字符串保存File、OutputStream等不同的介质中。 
     * writeValue(File arg0, Object arg1)把arg1转成json序列，并保存到arg0文件中。 
     * writeValue(OutputStream arg0, Object arg1)把arg1转成json序列，并保存到arg0输出流中。 
     * writeValueAsBytes(Object arg0)把arg0转成json序列，并把结果输出成字节数组。 
     * writeValueAsString(Object arg0)把arg0转成json序列，并把结果输出成字符串。 
     */
    ObjectMapper mapper = new ObjectMapper(); 
      
    //User类转JSON 
    //输出结果：{"name":"小明","age":20,"birthday":844099200000,"email":"xiaomin@sina.com"} 
    String json = mapper.writeValueAsString(user); 
    System.out.println(json); 
      
    //Java集合转JSON 
    //输出结果：[{"name":"小明","age":20,"birthday":844099200000,"email":"xiaomin@sina.com"}] 
    List<User> users = new ArrayList<User>(); 
    users.add(user); 
    String jsonlist = mapper.writeValueAsString(users); 
    System.out.println(jsonlist); 
  } 
} 

```


2 JSON转Java类[JSON反序列化]

```
import java.io.IOException; 
import java.text.ParseException; 
import com.fasterxml.jackson.databind.ObjectMapper; 
  
public class JacksonDemo { 
  public static void main(String[] args) throws ParseException, IOException { 
    String json = "{\"name\":\"小明\",\"age\":20,\"birthday\":844099200000,\"email\":\"xiaomin@sina.com\"}"; 
      
    /** 
     * ObjectMapper支持从byte[]、File、InputStream、字符串等数据的JSON反序列化。 
     */
    ObjectMapper mapper = new ObjectMapper(); 
    User user = mapper.readValue(json, User.class); 
    System.out.println(user); 
  } 
} 

```

[参考链接](http://www.jb51.net/article/77966.htm)

[参考链接2](http://www.jb51.net/article/42853.htm)


<!--
create time: 2018-03-18 11:38:31
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


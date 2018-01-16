# json-lib的使用


引入依赖

```

<dependency>    
    <groupId>net.sf.json-lib</groupId>    
    <artifactId>json-lib</artifactId>    
    <version>2.4</version>    
    <classifier>jdk15</classifier>    
</dependency> 

```


###数据封装

List集合转换成json代码

```
　　List list = new ArrayList();
　　list.add( "first" );
　　list.add( "second" );
　　JSONArray jsonArray2 = JSONArray.fromObject( list );
　　
```

Map集合转换成json代码

```
Map map = new HashMap();
　　map.put("name", "json");
　　map.put("bool", Boolean.TRUE);
　　map.put("int", new Integer(1));
　　map.put("arr", new String[] { "a", "b" });
　　map.put("func", "function(i){ return this.arr[i]; }");
　　JSONObject json = JSONObject.fromObject(map);
　　
```


Bean转换成json代码

```
SONObject jsonObject = JSONObject.fromObject(new JsonBean());
```

数组转换成json代码

```
　　boolean[] boolArray = new boolean[] { true, false, true };
　　JSONArray jsonArray1 = JSONArray.fromObject(boolArray);
　　
```
一般数据转换成json代码

```
JSONArray jsonArray3 = JSONArray.fromObject("['json','is','easy']" );

```

JSONObject对象使用

JSON-lib包是一个beans,collections,maps,java arrays 和XML和JSON互相转换的包。在本例中，我们将使用JSONObject类创建JSONObject对象，然后我们打印这些对象的值。为了使用JSONObject对象，我们要引入”net.sf.json”包。为了给对象添加元素，我们要使用put()方法。


```
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

public class JSONObjectSample {

    //创建JSONObject对象   

    private static JSONObject createJSONObject(){   

        JSONObject jsonObject = new JSONObject();   

        jsonObject.put("username","huangwuyi");   

        jsonObject.put("sex", "男");   

        jsonObject.put("QQ", "999999999");   

        jsonObject.put("Min.score", new Integer(99));   

        jsonObject.put("nickname", "梦中心境");   

        return jsonObject;   

    }   

    public static void main(String[] args) {   

        JSONObject jsonObject = JSONObjectSample.createJSONObject();   

        //输出jsonobject对象   

        System.out.println("添加属性前的对象jsonObject==>\n"+jsonObject);   

        //判读输出对象的类型   

        boolean isArray = jsonObject.isArray();   

        boolean isEmpty = jsonObject.isEmpty();   

        boolean isNullObject = jsonObject.isNullObject();   

        System.out.println("isArray:"+isArray+" isEmpty:"+isEmpty+" isNullObject:"+isNullObject);  

        //添加属性   

        jsonObject.element("address", "福建省厦门市");   

        System.out.println("添加属性后的对象jsonObject==>\n"+jsonObject);   



        //返回一个JSONArray对象   

        JSONArray jsonArray = new JSONArray();   

        jsonArray.add(0, "this is a jsonArray value");   

        jsonArray.add(1,"another jsonArray value");   

        jsonObject.element("jsonArray", jsonArray);   

        JSONArray array = jsonObject.getJSONArray("jsonArray");   

        System.out.println("返回一个JSONArray对象==>\n"+array);   

        //添加JSONArray后的值   

        System.out.println("结果==>\n"+jsonObject);   



        //根据key返回一个字符串   

        String username = jsonObject.getString("username");   

        System.out.println("username==>"+username);  



        //把字符转换为 JSONObject

        String temp=jsonObject.toString();

        JSONObject object = JSONObject.fromObject(temp);

        //转换后根据Key返回值

        System.out.println("qq==>"+object.get("QQ"));

    }   

}

```

<!--
create time: 2018-01-16 17:16:49
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


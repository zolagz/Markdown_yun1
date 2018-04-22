# redis jedis存储对象简单操作

[来源](https://blog.csdn.net/mingtianhaiyouwo/article/details/51725235)


要求对象实现Serializable接口

public class Person implements Serializable{...}

```
@Test
    public void test3(){

        Person p1 = new Person();
        p1.setName("张三");
        p1.setAddress("北京中关村");
        p1.setAge(20);
        p1.setMobile("4567890");


// 存入一个 user对象
        jedis.set("user1".getBytes(), SerializationUtil.serialize(p1));

        // 获取
        byte[] bs = jedis.get("user1".getBytes());

        Person desUser = (Person) SerializationUtil.deserialize(bs);
        System.out.println(desUser.getAge() + ":" + desUser.getName());
    }
```

SerializationUtil实现

```
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class SerializationUtil {
    /**
     * 序列化
     *
     * @param object
     * @return
     */
    public static byte[] serialize(Object object) {
        ObjectOutputStream oos = null;
        ByteArrayOutputStream baos = null;
        try {
            baos = new ByteArrayOutputStream();
            oos = new ObjectOutputStream(baos);
            oos.writeObject(object);
            byte[] bytes = baos.toByteArray();
            return bytes;
        } catch (Exception e) {
        }
        return null;
    }

    /**
     * 反序列化
     *
     * @param bytes
     * @return
     */
    public static Object deserialize(byte[] bytes) {
        ByteArrayInputStream bais = null;
        try {
            bais = new ByteArrayInputStream(bytes);
            ObjectInputStream ois = new ObjectInputStream(bais);
            return ois.readObject();
        } catch (Exception e) {

        }
        return null;
    }

```





<!--
create time: 2018-04-21 10:14:54
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


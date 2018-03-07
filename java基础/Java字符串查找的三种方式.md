# Java字符串查找的三种方式


###indexof方法：

注解：indexOf 方法返回一个整数值，指出 String 对象内子字符串的开始位置。如果没有找到子字符串，则返回-1。

```	
public class IndexOf{
    public static void main(String[] args){
        String s="李宏#王海#林巧#陆寻#唐梅";
        String q="#"; //需要查找的字符串
        String err="*"; //不存在的字符串
        int i=0;
        for(int j=0;j<s.length();j++){ //循环所有字符串
            String get=s.substring(j,j+1); //打印所有字符串
            if(get.equals(q)){ //判断#字是否出现
                i++; //#字出现次数
            }
        }
        System.out.println("总共有"+s.length()+"个字符串");
        System.out.println("#字共出现了"+i+"次"); //#字符总共出现的次数
        System.out.println("第一个#字出现在字符串的"+s.indexOf(q)+"个位置"); 
        if(s.indexOf(err)==-1){ //返回-1则表示字符不存在字符串中
            System.out.println("*字在字符串中不存在");
        }
    }
}
```
运行结果：

    总共有14个字符串
    #字共出现了4次
    第一个#字出现在字符串的2个位置
    *字在字符串中不存在

###startsWith方法：

注解：startsWith() 方法用于检测字符串是否以指定的前缀开始。

```
	
public class StartWith{
    public static void main(String[] args){
        String id[]= {"53011198902280308","52011198711038269","53011197701328291"};
        int number = 0;
        System.out.println("符合条件的字符串有");
        for(int i=0;i<id.length;i++) {
            if(id[i].startsWith("530") == true) {
                number++;
                System.out.println(id[i]);
            }
        }
        System.out.println("前面3个字符为‘530'的身份证有："+number+"个");
    }
}
```

运行结果：

    符合条件的字符串有
    53011198902280308
    53011197701328291
    前面3个字符为‘530'的身份证有：2个

###regionMatches方法：

注解：regionMatches() 方法用于检测两个字符串在一个区域内是否相等。

```
	public class RegionMatches{
    public static void main(String[] args) {
        int number = 0;
        String s = "student;entropy;ENgage,English,client,eye";
        String q="en"; //需要查找的字符串
        for (int k=0;k<s.length();k++){
        //true为不区分大小写，k为所有字符串，q为需要查找的字符串，0为从字符串1的位置开始，2为需要查找的字符串长度为2
        if(s.regionMatches(true, k, q, 0, 2)){ 
                number++;
                System.out.println("en字符在字符串的第"+k+"个位置");
            }
        }
        System.out.println("含有‘en'子串的字符串的总数有："+number);
    }
}
```
运行结果：

    en字符在字符串的第4个位置
    en字符在字符串的第8个位置
    en字符在字符串的第16个位置
    en字符在字符串的第23个位置
    en字符在字符串的第34个位置
    含有‘en'子串的字符串的总数有：5



<!--
create time: 2018-03-03 17:20:51
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


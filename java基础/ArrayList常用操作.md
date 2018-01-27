###ArrayList常用操作

1.数组长度使用.size();
2.增加数组的元素.add(1," ");
             .add(" ")；
        两种方法的区别
3.修改某个元素的方法.set(1,"B");
4.删除元素的 方法.remove(2," ");
               .remove(" ");
          两种方法的区别
5.遍历数组的方法

		 ArrayList<String> array = new ArrayList<String>();
		 
		 array.add("Hello");
		 array.add("Java");
		 array.add("Hello");
		 array.add("World");
		 
		 
###		遍历数组

```
 //		方法一
		 Iterator<String> itr = array.iterator();
		 while (itr.hasNext()) {
			 String str =  itr.next();
			 System.out.println(str);
		 }
		 
		 System.out.println("-------------");
 //		方法 2 普通 for 循环
		 for(int i = 0 ;i < array.size();i++) {
			 System.out.println(array.get(i));
		 }
		 System.out.println("*********");
		 
```
###		方法 3 增强 for 循环

```
		 for (String str : array) {
			 System.out.println(str);
		 } 
```
### ListIterator<E>  listIterator();返回此元素的列表迭代器
 ```
 * 
 * 特有功能：
 * 		E 	previous();返回列表中的前一个元素
 * 		boolean  hasPrevious()；如果以逆向遍历列表，列表迭代器有多个元素，，则返回true
 * 		注意： ListIterator 可以实现逆向遍历，但前提要求先正向遍历，才能逆向遍历

	 ArrayList<String> array = new ArrayList<String>();
		 array.add("Hello");
		 array.add("Java");
		 array.add("One");
		 array.add("World");
		 
 //		先正向遍历
		 ListIterator<String> lsitr = array.listIterator();
		 while (lsitr.hasNext()) {
			 String str =  lsitr.next();
			 System.out.println(str);
		 }
		 System.out.println("---1----");
		 

 ```
 		 
###		在逆向遍历(要先正向遍历)
		 while (lsitr.hasPrevious()) {
			 String str =  lsitr.previous();
			 System.out.println("vv - >" + str);
		 }

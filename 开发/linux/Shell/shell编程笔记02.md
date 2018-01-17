###创建函数
第一种方式：
>function name {
>
>	commands
>
>}

```
#!/bin/bash
# using a function in a script

function func1 {
	echo "This is an example of a fuction"
}
count=1
while [ $count -ls 5 ]
do
	func1
	count=$[ $count + 1 ]
done
echo "This is the end of the loop"
func1
echo "Now this is the end of the script"
```
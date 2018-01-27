 Linux命令：cp (copy)复制文件或目录

复制文件，只有源文件较目的文件的修改时间新时，才复制文件
     cp -u -v file1 file2

    将文件file1复制成文件file2
     cp file1 file2

    采用交互方式将文件file1复制成文件file2
     cp -i file1 file2

    将文件file1复制成file2，因为目的文件已经存在，所以指定使用强制复制的模式
     cp -f file1 file2

    将目录dir1复制成目录dir2
     cp -R file1 file2

    同时将文件file1、file2、file3与目录dir1复制到dir2
　　 cp -R file1 file2 file3 dir1 dir2

    复制时保留文件属性
     cp -p atxt tmp/

    复制时保留文件的目录结构
     cp -P  /var/tmp/atxt  /temp/

    复制时产生备份文件
     cp -b atxt tmp/

    复制时产生备份文件，尾标 ~1~格式
     cp -b -V t   atxt /tmp   
 
    指定备份文件尾标   
     cp -b -S _bak atxt /tmp
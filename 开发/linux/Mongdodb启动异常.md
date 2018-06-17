# Mongdodb启动异常


Mongodb 出现异常 exception in initAndListen: IllegalOperation...


2018-04-26T18:33:31.670+0800 I STORAGE  [initandlisten] exception in initAndListen: IllegalOperation: Attempted to create a lock file on a read-only directory: /data/db, terminating

出现的原因是因为 /data/db的权限不足导致的。


解决办法：修改权限就好  

 chmod 777 -R data
 
 

<!--
create time: 2018-06-17 11:03:08
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


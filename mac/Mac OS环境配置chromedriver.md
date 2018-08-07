# Mac OS环境配置chromedriver

[原文链接](https://www.jianshu.com/p/e137031bc7db)

chromedriver文件放在“/usr/local/bin”目录下，然后运行下面的代码

```
from selenium import webdriver

driver = webdriver.Chrome()
base_url = 'https://www.baidu.com'
driver.get(base_url)
```



<!--
create time: 2018-06-18 18:37:44
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


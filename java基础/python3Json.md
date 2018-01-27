###python3中key=value的使用

在python3中urllib2被改为urllib库下request模块。编码使用urllib库下parse模块

from urllib import parse
### url转码操作，只有转码后浏览器才会识别该url
kw = {'name': '中国'}

res = parse.urlencode(kw)

print(res)

res2 = parse.unquote(res)

print(res2)

### 结果如下：

 name=E5%B0%8F%E5%8F%AF

 name=中国

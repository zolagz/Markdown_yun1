1,简述

所谓网页抓取，就是把URL地址中指定的网络资源从网络流中抓取出来。在Python中有很多库可以用来抓取网页。

在python2中自带urllib和urllib2。二者区别如下：

   1，urllib 模块仅可以接受URL，不能创建 设置headers 的Request 类实例；

   2，但是 urllib 提供 urlencode 方法用来产生GET查询字符串，而 urllib2 则没有。（这是 urllib 和 urllib2 经常一起使用的主要原因）

   3，编码工作使用urllib的urlencode()函数，帮我们将key:value这样的键值对，转换成"key=value"这样的字符串，解码工作可以使用urllib的unquote()函数。（ 注意，不是urllib2.urlencode())

在python3中urllib2被改为urllib库下request模块。编码使用urllib库下parse模块

```

from urllib import parse
# url转码操作，只有转码后浏览器才会识别该url
kw = {'name': '中国'}
res = parse.urlencode(kw)
print(res)
res2 = parse.unquote(res)
print(res2)
# 结果如下：
# name=E5%B0%8F%E5%8F%AF
# name=中国

注意：本文采用python3.5的python爬虫环境。
2，urlopen发送请求，返回响应案例

# 导入urllib 库下request模块
from urllib import request

# 向指定的url发送请求，并返回服务器响应的类文件对象
response = request.urlopen("http://www.baidu.com")

# 类文件对象支持 文件对象的操作方法，如read()方法读取文件全部内容，返回字符串
html = response.read()

# 打印字符串
print(html)

```

3，Request构造请求对象

```
# 导入urllib 库下request模块
from urllib import request

# url 作为Request()方法的参数，构造并返回一个Request对象
req = request.Request("http://www.baidu.com")

# Request对象作为urlopen()方法的参数，发送给服务器并接收响应
response = request.urlopen(req)

html = response.read()

print(html)

```
注意：
新建Request实例，除了必须要有 url 参数之外，还可以设置另外两个参数：

   1，data（默认空）：提交的Form表单数据，同时 HTTP 请求方法将从默认的 "GET"方式 改为 "POST"方式。

   2，headers（默认空）：参数为字典类型，包含了需要发送的HTTP报头的键值对。

4，User-Agent伪装成浏览器

   浏览器 就是互联网世界上公认被允许的身份，如果我们希望我们的爬
    虫程序更像一个真实用户，那我们第一步就是需要伪装成一个被浏览器。用不同的浏览器在发送请求的时候，会有不同的 User-Agent 报头。

   python2中的urllib2默认的User-Agent头为：Python-urllib/x.y （x和y 是Python 主.次 版本号，例如 Python-urllib/2.7）

```
from urllib import request

def loadPage():
    # 包含一个请求报头的headers, 发送的请求会附带浏览器身份发送
    headers = {"User-Agent": "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Trident/5.0)"}
    # 构造一个请求对象
    req = request.Request("http://www.baidu.com/", headers=headers)
    # 发送http请求，返回服务器的响应内容
    # response响应是一个类文件对象
    response = request.urlopen(req)
    # 类文件对象支持文件操作的相关方法，比如read()：读取文件所有内容，返回字符串
    return response.read()


if __name__ == "__main__":
    # print __name__
    html = loadPage()
    # 打印字符串，也就是整个网页的源码
    print(html)
    
```

5，添加更多的Header信息

在 HTTP Request 中加入特定的 Header，来构造一个完整的HTTP请求消息。

可以通过调用Request.add_header() 添加/修改一个特定的header 也可以通过调用Request.get_header()来查看已有的header。

```

# 导入urllib 库下request模块
from urllib import request

url = "http://www.baidu.com"

# IE 9.0 的 User-Agent
user_agent = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)"}
req = request.Request(url, headers=user_agent)

# 也可以通过调用Reques.add_header() 添加/修改一个特定的header为长链接
req.add_header("Connection", "keep-alive")

# 也可以通过调用Request.get_header()来查看header信息
# req.get_header(header_name="Connection")

response = request.urlopen(req)

print(response.code)  # 可以查看响应状态码
# 网页源代码
html = response.read()

print(html)

```

6，随机添加/修改User-Agent

```

from  urllib import request
import random


def loadPage(url):
    # User-Agent 列表
    useragent_list = [
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/22.0.1207.1 Safari/537.1",
        "Mozilla/5.0 (X11; CrOS i686 2268.111.0) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.57 Safari/536.11",
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.6 (KHTML, like Gecko) Chrome/20.0.1092.0 Safari/536.6",
        "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/536.6 (KHTML, like Gecko) Chrome/20.0.1090.0 Safari/536.6",
        "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/19.77.34.5 Safari/537.1",
        "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/536.5 (KHTML, like Gecko) Chrome/19.0.1084.9 Safari/536.5",
        "Mozilla/5.0 (Windows NT 6.0) AppleWebKit/536.5 (KHTML, like Gecko) Chrome/19.0.1084.36 Safari/536.5",
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3",
        "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3",
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_0) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3"
    ]
    # 随机选择一个User-Agent字符串
    # random.choice()这个方法可以从列表中随机的取出一个元素
    user_agent = random.choice(useragent_list)

    # 构造请求对象
    req = request.Request(url)
    # 添加一个请求报头,添加的时候是User-Agent
    req.add_header("User-Agent", user_agent)
    # 查看指定请求报头，获取的时候一定是User-agent
    print(req.get_header("User-agent"))

    # 发送请求，返回服务器响应
    reseponse = request.urlopen(req)
    # 返回响应的html内容
    return reseponse.read()


if __name__ == "__main__":
    url = input("请输入需要抓取的网页:")
    html = loadPage(url)
    print(html)
    
```

7,GET方式请求，爬取贴吧案例

```
# 处理HTTP请求,在python2中是urllib2,在python3中是urllib.request
from urllib import request, parse


def loadPage(url, filename):
    '''
           作用：根据url发送请求，获取服务器响应文件
           url：需要爬取的url地址
           filename: 文件名
    '''
    print("[LOG]: 正在下载" + filename)
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/536.6 (KHTML, like Gecko) Chrome/20.0.1090.0 Safari/536.6"}
    # 构造请求对象
    req = request.Request(url, headers=headers)
    # 发送请求，返回响应结果
    response = request.urlopen(req)
    if response.getcode() == 200:
        return response.read()
    else:
        print("[ERR]: 下载失败...")


def writePage(html, filename):
    """
          作用：保存服务器响应文件到本地磁盘文件里
          html: 服务器响应文件
          filename: 本地磁盘文件名
    """
    print("[LOG]: 正在写入" + filename)
    with open(filename, "wb") as f:
        f.write(html)


def tiebaSpider(url, beginPage, endPage):
    """
            作用：负责处理url，分配每个url去发送请求
            url：需要处理的第一个url
            beginPage: 爬虫执行的起始页面
            endPage: 爬虫执行的截止页面
    """
    for page in range(beginPage, endPage + 1):
        pn = (page - 1) * 50
        # 对pn进行转码后，才能在浏览器中识别
        keyword = parse.urlencode({"pn": pn})
        fullurl = url + "&" + keyword
        print(fullurl)
        # 爬取到的数据存放的文件名称
        filename = "第" + str(page) + "页.html"
        html = loadPage(fullurl, filename)
        writePage(html, filename)


if __name__ == "__main__":
    tiebaName = input("请输入需要爬取的贴吧名：")
    beginPage = int(input("请输入爬取的起始页："))
    endPage = int(input("请输入爬取的终止页: "))

    # "https://tieba.baidu.com/f? kw=%E6%9D%8E%E6%AF%85&pn=50"
    baseURL = "http://tieba.baidu.com/f?"
    keyword = {"kw": tiebaName}
    kw = parse.urlencode(keyword)
    url = baseURL + kw
    tiebaSpider(url, beginPage, endPage)

```

8，POST方式爬取有道词典案例

```

from urllib import request, parse


def loadPage():
    # 新版本的有道翻译的接口
    # url = "http://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule&sessionFrom=null"
    # 老版本的有道翻译的接口
    url = "http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule&sessionFrom=null"

    # i = bytes(input("请输入需要翻译的句子：").encode('utf-8'))
    i = input("请输入需要翻译的句子：")
    # form表单数据，通过网页源代码可查看携带的请求参数
    formdata = {
        "i": i,
        "from": "AUTO",
        "to": "AUTO",
        "smartresult": "dict",
        "client": "fanyideskweb",
        # "salt" : "1500253430162",
        # "sign" : "db86dafb01c61b4c5bc85bc0868ff7f6",
        "doctype": "json",
        "version": "2.1",
        "keyfrom": "fanyi.web",
        "action": "FY_BY_CL1CKBUTTON",
        "typoResult": "true",
    }

    # 将数据转为url编码，注意这个地方要加encode（）编码方式，否则会报错如下：
    # "POST data should be bytes or an iterable of bytes. It cannot be of type str.
    data = parse.urlencode(formdata).encode(encoding='utf8')

    # 请求报头
    headers = {"User-Agent": "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Trident/5.0)"}

    # 构造一个请求，因为有data参数，所有请求会从默认的get变成post
    # 发送post请求同时，会把data数据传输到指定的url地址
    req = request.Request(url, data=data, headers=headers)

    # 发送请求，返回响应
    response = request.urlopen(req)
    # 去除字符串首尾的空格
    content = response.read().strip()

    # print content
    # 将字符串格式的字典，转换为真正的字典
    content = eval(content)

    print(content)

    # 取出字典的值
    print(content["translateResult"][0][0]["tgt"])


if __name__ == "__main__":
    loadPage()
  
```

# 结果如下：

```
# 请输入需要翻译的句子：hello

# 返回的json字符串如下：    
# {'type': 'EN2ZH_CN', 
#  'translateResult': [[{'src': 'hello', 'tgt': '你好'}]], 
#  'smartResult': {'type': 1, 'entries': ['', 'n. 表示问候， 惊奇或唤起注意时的用语', 'int. 喂；哈罗', 'n. (Hello)人名；(法)埃洛']}, 
#  'errorCode': 0, 
#  'elapsedTime': 1 }

# 最终结果：
# 你好

9，处理HTTPS请求 SSL证书验证

现在随处可见 https 开头的网站，urllib2可以为 HTTPS 请求验证SSL证书，就像web浏览器一样，如果网站的SSL证书是经过CA认证的，则能够正常访问，如：https://www.baidu.com/等...

如果SSL证书验证不通过，或者操作系统不信任服务器的安全证书，比如浏览器在访问12306网站如：https://www.12306.cn/mormhweb/的时候，会警告用户证书不受信任。（据说 12306 网站证书是自己做的，没有通过CA认证）

如果没有ssl证书认证，会报如下错误：

urllib.error.URLError: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:645)>

from urllib import request
import ssl

# 表示忽略网站的不合法证书认证
context = ssl._create_unverified_context()

headers = {"User-Agent": "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Trident/5.0)"}

req = request.Request("https://www.12306.cn/mormhweb/", headers=headers)

# 在urlopen()添加context参数，请求将忽略网站的证书认证
response = request.urlopen(req, context=context)

print(response.read())

```

关于CA数据证书认证

CA(Certificate Authority)是数字证书认证中心的简称，是指发放、管理、废除数字证书的受信任的第三方机构，如北京数字认证股份有限公司、上海市数字证书认证中心有限公司等...

CA的作用是检查证书持有者身份的合法性，并签发证书，以防证书被伪造或篡改，以及对证书和密钥进行管理。

现实生活中可以用身份证来证明身份， 那么在网络世界里，数字证书就是身份证。和现实生活不同的是，并不是每个上网的用户都有数字证书的，往往只有当一个人需要证明自己的身份的时候才需要用到数字证书。

普通用户一般是不需要，因为网站并不关心是谁访问了网站，现在的网站只关心流量。但是反过来，网站就需要证明自己的身份了。

比如说现在钓鱼网站很多的，比如你想访问的是www.baidu.com，但其实你访问的是www.daibu.com”，所以在提交自己的隐私信息之前需要验证一下网站的身份，要求网站出示数字证书。

一般正常的网站都会主动出示自己的数字证书，来确保客户端和网站服务器之间的通信数据是加密安全的。

作者：晓可加油
链接：http://www.jianshu.com/p/2c6844a7c2ee
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
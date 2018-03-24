# HttpClient使用


####1.1.1	Java网络请求原生API-Get请求

```
import java.io.BufferedReader;import java.io.InputStream;import java.io.InputStreamReader;import java.net.HttpURLConnection;import java.net.URL;/** 使用JDK的api进行get请求  1.在使用httpurlconnection时，默认就是get请求。如何改成post请求？ 2.http协议中，可以指定header，想添加user-agent  */public class BasicHttpGet {   public static void main(String[] args) throws Exception {      //1.指定一个url      String domain = "http://www.itcast.cn";      //2.发起一个请求      URL url = new URL(domain);      HttpURLConnection conn = (HttpURLConnection)url.openConnection();            //添加请求方式      conn.setRequestMethod("GET");            //添加请求头------如果编写爬虫，真实浏览器发送的header都拷贝      conn.setRequestProperty("Accept", "text/html");      /**       Accept:text/html       **/            //3.获取返回值      InputStream inputStream = conn.getInputStream();      //3.1 将输入流转换字符串      BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));      //3.2 一次读取bufferReader的数据      String line =null;      while((line=bufferedReader.readLine())!=null){         System.out.println(line);      }      //4.关闭流      inputStream.close();   }  }

```


Java网络请求原生POST

```
import java.io.BufferedReader;import java.io.InputStream;import java.io.InputStreamReader;import java.io.OutputStream;import java.net.HttpURLConnection;import java.net.URL;/** 使用JDK的api进行POST请求  1.在使用httpurlconnection时，默认就是get请求。如何改成post请求？  第一步：设置请求方法 setRequestMethod("POST") 第二步：设置doOutPut(true)  2.http协议中，可以指定header，想添加user-agent */public class BasicHttpGet {   public static void main(String[] args) throws Exception {      //1.指定一个url      String domain = "http://www.itcast.cn";      //2.发起一个请求      URL url = new URL(domain);      HttpURLConnection conn = (HttpURLConnection)url.openConnection();            //2.1 添加请求方式      conn.setRequestMethod("POST");      //2.2 添加请求头------如果编写爬虫，真实浏览器发送的header都拷贝      conn.setRequestProperty("Accept", "text/html");      /**       Accept:text/html       **/      //2.3 发送一些数据      conn.setDoOutput(true);      OutputStream outputStream = conn.getOutputStream();      // 编写什么样格式的数据？  username=zhangsan&passwd=123      outputStream.write("username=zhangsan&passwd=123".getBytes());      outputStream.flush();      outputStream.close();      //3.获取返回值      InputStream inputStream = conn.getInputStream();      //3.1 将输入流转换字符串      BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));      //3.2 一次读取bufferReader的数据      String line =null;      while((line=bufferedReader.readLine())!=null){         System.out.println(line);      }      //4.关闭流      inputStream.close();   }}
```
####1.1.1	 使用HttpClient

HttpClient 是 Apache Jakarta Common 下的子项目，可以用来提供高效的、最新的、功能丰富的支持 HTTP 协议的客户端编程工具包，并且它支持 HTTP 协议最新的版本和建议。HtppClient 官网[](http://hc.apache.org/)为什么有HttpClient？超文本传输协议（HTTP）可能是当今互联网上使用的最重要的协议虽然java.net包提供了通过HTTP访问资源的基本功能，但它并没有提供许多应用程序所需的完全灵活性或功能使用HttpClient的maven依赖```<dependencies>    <dependency>        <groupId>org.apache.httpcomponents</groupId>        <artifactId>httpclient</artifactId>        <version>4.5.3</version>    </dependency>    <dependency>        <groupId>org.apache.httpcomponents</groupId>        <artifactId>fluent-hc</artifactId>        <version>4.5.3</version>    </dependency></dependencies>

```####1.1.2	 使用HttpClient进行Get请求

```package cn.itcast.spider.httpClient;import java.nio.charset.Charset;import org.apache.http.client.methods.CloseableHttpResponse;import org.apache.http.client.methods.HttpGet;import org.apache.http.impl.client.CloseableHttpClient;import org.apache.http.impl.client.HttpClients;import org.apache.http.util.EntityUtils;/** 使用 httpclient 来进行get请求 */public class HCGet {   public static void main(String[] args) throws Exception {      // 1.拿到一个httpclient的对象      CloseableHttpClient httpClient = HttpClients.createDefault();      // 2.设置请求方式和请求信息      HttpGet httpGet = new HttpGet("http://www.itcast.cn");      // 3.执行请求      CloseableHttpResponse response = httpClient.execute(httpGet);      // 4.获取返回值      String html = EntityUtils.toString(response.getEntity(),Charset.forName("utf-8"));      // 5.打印      System.out.println(html);   }}

```####1.1.3	 使用HttpClient进行Post请求

```package cn.itcast.spider.httpClient;import org.apache.http.client.entity.UrlEncodedFormEntity;import org.apache.http.client.methods.CloseableHttpResponse;import org.apache.http.client.methods.HttpPost;import org.apache.http.impl.client.CloseableHttpClient;import org.apache.http.impl.client.HttpClients;import org.apache.http.message.BasicNameValuePair;import org.apache.http.util.EntityUtils;import java.nio.charset.Charset;import java.util.ArrayList;/** 使用 httpclient 来进行post请求 */public class HCPost {   public static void main(String[] args) throws Exception {      // 1.拿到一个httpclient的对象      CloseableHttpClient httpClient = HttpClients.createDefault();      // 2.设置请求方式和请求信息//    HttpGet httpGet = new HttpGet("http://www.itcast.cn");      HttpPost httpPost = new HttpPost("http://www.itcast.cn");            //2.1 提交header头信息      httpPost.addHeader("user-agent", "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36");      //2.1 提交请求体      //提交方式1:一般用在原生ajax请求//    httpPost.setEntity(new StringEntity("username=zhangsan&passwd=123"));      //提交方式2：大多数的情况下用这种方式      ArrayList<BasicNameValuePair> parameters = new ArrayList<BasicNameValuePair>();      parameters.add(new BasicNameValuePair("username", "zhangsan"));      parameters.add(new BasicNameValuePair("passwd", "123"));      httpPost.setEntity(new UrlEncodedFormEntity(parameters));            // 3.执行请求      CloseableHttpResponse response = httpClient.execute(httpPost);      // 4.获取返回值      String html = EntityUtils.toString(response.getEntity(),Charset.forName("utf-8"));      // 5.打印      System.out.println(html);   }}

```
####1.1.4	 使用HttpClient的流畅API（了解）```package cn.itcast.spider.httpClient;import java.nio.charset.Charset;import org.apache.http.client.fluent.Request;/** 流畅的api * */public class FluentHttpClient {   public static void main(String[] args) throws Exception {      String html = Request.Get("http://www.itcast.cn").execute().returnContent().asString(Charset.forName("utf-8"));      System.out.println(html);      //    Request.Post("http://targethost/login")//          .bodyForm(Form.form().add("username", "vip").add("password", "secret").build()).execute()//          .returnContent();   }}

```

###参考

```

//        请求路径
        String url = "https://www.mi.com/?client_id=180100041086&masid=17489.0001&kwd=%E5%B0%8F%E7%B1%B3";

        //初始化一个httpclient
        CloseableHttpClient httpClient = HttpClients.createDefault();
    
//        指定请求方式
        HttpGet hget = new HttpGet(url);
        
//        执行请求
        CloseableHttpResponse response = httpClient.execute(hget);

// 获取请求响应
        HttpEntity entity = response.getEntity();
        
//        解析请求
        String enStr = EntityUtils.toString(entity, Charset.forName("utf-8"));

        System.out.println(enStr);

```


<!--
create time: 2018-01-12 18:48:17
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->


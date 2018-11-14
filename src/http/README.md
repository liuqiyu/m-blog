# http常见面试题

### 一、http与https的区别

* HTTP 的URL 以http:// 开头，而HTTPS 的URL 以https:// 开头
* HTTP 是不安全的，而 HTTPS 是安全的
* HTTP 标准端口是80 ，而 HTTPS 的标准端口是443
* 在OSI 网络模型中，HTTP工作于应用层，而HTTPS 的安全传输机制工作在传输层
* HTTP 无法加密，而HTTPS 对传输的数据进行加密
* HTTP无需证书，而HTTPS 需要CA机构wosign的颁发的SSL证书

### 二、http无状态协议？怎么解决http协议无状态

* 无状态协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息
    * 就是说，当客户端一次HTTP请求完成以后，客户端再发送一次HTTP请求，HTTP并不知道当前客户端是一个”老用户“。
* 可以使用Cookie来解决无状态的问题，Cookie就相当于一个通行证，第一次访问的时候给客户端发送一个Cookie，
当客户端再次来的时候，拿着Cookie(通行证)，那么服务器就知道这个是”老用户“。

### 三、常用的http方法

* GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器
* POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
* PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
* HEAD： 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。
* DELETE：删除文件，与PUT方法相反，删除对应URI位置的文件。
* OPTIONS：查询相应URI支持的HTTP方法。

### 四、常用的http响应状态码

### 五、http1.0和http1.1区别？

`http1.0`中，建立连接后，客户端发送一个请求，服务器端返回一个信息后就关闭连接，当浏览器再次请求又要建立连接。

### 六、http请求步骤

* 1、域名解析
    * 1、搜索浏览本身的DNS缓存  || yes 解析结束  no  下一步
    * 2、搜索操作系统自身的DNS缓存  || yes 解析结束  no  下一步
    * 3、读取host文件  || yes 解析结束  no  下一步
    * 4、向本地配置的首选DNS服务器发起域名解析  || yes 解析结束  no  下一步
* 2、发起TCP三次握手
* 3、发起http请求
* 4、服务器响应`http`请求
* 5、浏览器解析html代码，并请求HTML里面的资源（js、css、图片等）
* 6、浏览器对页面进行渲染呈现给用户



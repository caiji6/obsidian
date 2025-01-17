# 👌Tomcat性能优化如何做？

[此处为语雀卡片，点击链接查看](https://www.yuque.com/jingdianjichi/xyxdsi/bwhb8e4v6idr2ocq#ScIei)

<font style="color:rgb(47, 48, 52);">Tomcat默认参数是为开发环境制定，而非适合生产环境，尤其是内存和线程的配置，默认都很低，容易成为性能瓶颈。所以有些公司可能会问到这个问题。鸡哥把一些 tomcat 的优化方式来给大家放在这里讲解。我要求大家记住几个常用的即可。</font>

### <font style="color:rgb(47, 48, 52);">Tomcat连接器协议优化</font>
<font style="color:rgb(47, 48, 52);">Tomcat 连接器的三种方式： bio、nio 和 apr，三种方式性能差别很大，apr 的性能最优， bio 的性能最差。而 Tomcat 7 使用的 Connector 默认就启用的 Apr 协议，但需要系统安装 Apr 库，否则就会使用 bio 方式。</font>

#### <font style="color:rgb(47, 48, 52);">nio如何配置</font>
<font style="color:rgb(47, 48, 52);">进入到tomcat的server.xml找到connector。更改其中的protocol属性即可。</font>

```plain
<Connector port="80" protocol="org.apache.coyote.http11.Http11NioProtocol" 
	connectionTimeout="20000" 
	URIEncoding="UTF-8" 
	useBodyEncodingForURI="true" 
	enableLookups="false" 
	redirectPort="8443" />
```

#### <font style="color:rgb(47, 48, 52);">apr如何配置</font>
<font style="color:rgb(47, 48, 52);">apr的配置需要安装依赖</font>

```plain
yum -y install openssl-devel
yum -y install apr-devel
```

<font style="color:rgb(47, 48, 52);">安装之后，去tomcat官网下载native组件，native可以看成是tomcat和apr交互的中间环节，下载地址是：http://tomcat.apache.org/download-native.cgi 这里下载最新的版本1.2.10</font>

<font style="color:rgb(47, 48, 52);">解压并安装</font>

```plain
tar -xvzf tomcat-native-1.2.10-src.tar.gz
cd tomcat-native-1.2.10-src/native/
./configure
```

<font style="color:rgb(47, 48, 52);">  
</font><font style="color:rgb(47, 48, 52);">至此apr安装成功，进入server.xml。更改协议将默认的protocol="HTTP/1.1"修改为protocol="org.apache.coyote.http11.Http11AprProtocol"。</font>

### <font style="color:rgb(47, 48, 52);">Tomcat配置文件方面的优化</font>
<font style="color:rgb(47, 48, 52);">配置文件方面是我们主要的tomcat优化的地方。我们将常见的优化直接在配置文件中放置。</font>

<font style="color:rgb(47, 48, 52);">1、connectionTimeout="30000"：网络连接超时，单位：毫秒，设置为 0 表示永不超时，这样设置有隐患的。通常可设置为 30000 毫秒，可根据检测实际情况，适当修改</font>

<font style="color:rgb(47, 48, 52);">2、enableLookups="false"：是否反查域名，以返回远程主机的主机名，取值为：true 或 false，如果设置为false，则直接返回IP地址，为了提高处理能力，应设置为 false。</font>

<font style="color:rgb(47, 48, 52);">3、disableUploadTimeout="false"：上传时是否使用超时机制。</font>

<font style="color:rgb(47, 48, 52);">4、connectionUploadTimeout="150000"：上传超时时间，毕竟文件上传可能需要消耗更多的时间，这个根据你自己的业务需要自己调，以使Servlet有较长的时间来完成它的执行，需要与上一个参数一起配合使用才会生效。</font>

<font style="color:rgb(47, 48, 52);">5、acceptCount="300"：指定当所有可以使用的处理请求的线程数都被使用时，可传入连接请求的最大队列长度，超过这个数的请求将不予处理，默认为100个。</font>

<font style="color:rgb(47, 48, 52);">6、keepAliveTimeout="120000"：长连接最大保持时间（毫秒），表示在下次请求过来之前，Tomcat 保持该连接多久，默认是使用 connectionTimeout 时间，-1 为不限制超时。</font>

<font style="color:rgb(47, 48, 52);">7、maxKeepAliveRequests="1"：表示在服务器关闭之前，该连接最大支持的请求数。超过该请求数的连接也将被关闭，1表示禁用，-1表示不限制个数，默认100个，一般设置在100~200之间。</font>

<font style="color:rgb(47, 48, 52);">8、compression="on"：是否对响应的数据进行 GZIP 压缩，off：表示禁止压缩；on：表示允许压缩（文本将被压缩）、force：表示所有情况下都进行压缩，默认值为off，压缩数据后可以有效的减少页面的大小，一般可以减小1/3左右，节省带宽。</font>

<font style="color:rgb(47, 48, 52);">9、compressionMinSize="2048"：表示压缩响应的最小值，只有当响应报文大小大于这个值的时候才会对报文进行压缩，如果开启了压缩功能，默认值就是2048。</font>

<font style="color:rgb(47, 48, 52);">10、compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain,image/gif,image/jpg,image/png"：压缩类型，指定对哪些类型的文件进行数据压缩。</font>

### <font style="color:rgb(47, 48, 52);">Tomcat的字符集配置优化</font>
<font style="color:rgb(47, 48, 52);">Tomcat 的语言编码，配置起来很慢，要经过多次设置才可以了，否则中文很有可能出现乱码情况。譬如汉字“中”，以 UTF-8 编码后得到的是 3 字节的值 %E4%B8%AD，然后通过 GET 或者 POST 方式把这 3 个字节提交到 Tomcat 容器，如果你不告诉 Tomcat 我的参数是用 UTF-8编码的，那么 Tomcat 就认为你是用 ISO-8859-1 来编码的，而 ISO8859-1（兼容 URI 中的标准字符集 US-ASCII）是兼容 ASCII 的单字节编码并且使用了单字节内的所有空间，因此 Tomcat 就以为你传递的用 ISO-8859-1 字符集编码过的 3 个字符，然后它就用 ISO-8859-1 来解码。</font>

<font style="color:rgb(47, 48, 52);"></font>

<font style="color:rgb(47, 48, 52);">设置起来不难使用“ -D<名称>=<值> ”来设置系统属性：</font>

<font style="color:rgb(47, 48, 52);">-Djavax.servlet.request.encoding=UTF-8</font>

<font style="color:rgb(47, 48, 52);">-Djavax.servlet.response.encoding=UTF-8 </font>

<font style="color:rgb(47, 48, 52);">-Dfile.encoding=UTF-8 </font>

<font style="color:rgb(47, 48, 52);">-Duser.country=CN </font>

<font style="color:rgb(47, 48, 52);">-Duser.language=zh</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/bwhb8e4v6idr2ocq>
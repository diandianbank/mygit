Tomcat开发web站点lia

## 1.web开发的相关知识

### 1.1.c/s与b/s

## 1.2通讯协议

浏览器向web服务器发送一个请求，web服务器会对请求进行处理，并将结果返回。在这个交互过程中，浏览器是通过URL地址来访问服务器，并将结果在传输过程中遵循http

#### 1.2.1 URL地址

放置在Internet上的web服务器中的每个网页文件都有一个访问标记符，用来标识它的访问地址（URL），在URL中包括web服务器的主机名、端口号、资源名以及所使用的的网络协议。例如：

http://www.itcost.cn:80/index.HTML中http表示传输数据所使用的的协议，www.itcost.cn表示要请求的服务器主机名，80表示要请求的端口号，index.html表示要请求的资源名称

## 1.2.2 http请求地址

http是一种超文本传输协议，用于定义浏览器与web服务器之间交换数据的格式  http是一种规范。通信过程

1.浏览器与web服务器建立tcp连接

2.客户端向服务器发送http请求

3.服务器向客户端发送http响应

4.关闭TCP连接

### 1.2.3web资源

web资源可分为静态资源与动态资源 静态资源中的页面内容不会发生改变，包括HTML、CSS、jpg等

动态资源是由系统动态生成的资源，动态资源通常包括JSP、servlet等

## 2.Tomcat的目录介绍

bin：用于存放Tomcat的可执行的文件和脚本文件(扩展名以.bat的文件)

conf:用于存放Tomcat的各种配置文件

lib:用于存放Tomcat服务器和所有web应用程序需要访问的jar文件

temp：用于存放Tomcat运行时产生的临时文件

logs:用于存放Tomcat的日志文件

webapps： web应用程序的主要发布目录，通常将要发布的应用程序放到这个目录下

work: Tomcat的工作目录，JSP编译生成的servlet源文件和字节码文件放到这个目录下

##  2.1常见的问题

1.启动Tomcat时，一闪而过，可能的原因是jdk没有配置正确

解决问题：通过命令行进入Tomcat的bin目录下，执行startup.bat的命令，根据报错，进一步解决。

如果报jdk的问题，需要查找jdk的配置是否正确。

2.启动时会出现启动失败 ，可能的原因是Tomcat服务器使用的网络监听端口被其他服务程序占用所导致的。通过在命令行中输入“netstat -na"命令，查看本机运行的端口号，

   1.如果有，将进程杀死，重新启动Tomcat服务。

   2.如果不能将进程杀死，通过修改Tomcat监听的端口号，在conf中server.xml中修改配置文件，在server.xml中有一个<connector>元素中有一个port属性，将其端口号修改成其他的。在浏览器中输入URL中端口号是修改过的端口号。

http默认的端口号80，https默认的端口号是443

3.将文件放置在Tomcat安装目录之外的地方，在浏览器中打开报404错误

在conf中server.xml 中的<host>元素中添加一个<context>元素，在path中输入指定web应用的虚拟路径，docBase属性用于指定该虚拟路径所映射到本地文件系统目录。可以使用绝对路径或相对与<tomcat>安装目录的webapps的相对路径。修改之后，重启Tomcat服务器。重新再浏览其中打开

## 3.将应用配置到Tomcat中

### 3.1在server.xml文件中配置虚拟目录

将文件放置在Tomcat安装目录之外的地方，在浏览器中打开报404错误

在conf中server.xml 中的<host>元素中添加一个<context>元素，在path中输入指定web应用的虚拟路径，docBase属性用于指定该虚拟路径所映射到本地文件系统目录。可以使用绝对路径或相对与<tomcat>安装目录的webapps的相对路径。修改之后，重启Tomcat服务器。重新再浏览其中打开。



![微信图片_20190904160305](C:\Users\Administrator\Desktop\web学习\微信图片_20190904160305.jpg)

### 3.2在自定义xml文件中配置虚拟目录

优点：不用向修改server.xml那种，修改完成需要重启Tomcat服务器，重新在网页中打开

自定义xml配置的步骤

在Tomcat安装目录->conf->catalina->localhost目录下，创建一个以.xml的配置文件，然后将server.xml中的配置好的<Context>中的元素进行复制，重新启动tomca服务器。

不仅可以配置虚拟目录，还可以配置默认的web应用，配置将文件名改为ROOT.xml,重启服务器，在浏览器中输入http://localhost:80/文件夹,就可以进行访问了。

### 3.3配置web应用默认页面

要在WEB-INF目录下的web.xml中进行配置，将<welcome-file>中配置，然后重启tomca服务

### 3.4tomca控制台

在界面中可以看到每个web应用的启动、停止、卸载的功能以及运行的状态。

在浏览器中输入http://localhost:8080,在tomcat的首页中点击Manager App链接进入tomcat管理控制台。由于首次进入，没有密码与用户名。点击取消，浏览器会跳转到一个401的界面。

在Tomcat安装目录下的conf的tomcat-users.xml文件中添加具有管理员权限的账号。Tomcat中定义了4中不同的角色。

manager-gui: 允许访问html图形管理界面控制台和状态页面

manager-script：允许访问文本接口和状态页面

manager-jmx:允许访问JMX代理和状态页面

manager-status:只允许访问状态页面

例如：在Tomcat管理控制台中添加manager-gui角色，并创建一个拥有该角色的用户，用户名为ITcast，密码为123

<?xml version="1.0" encoding="utf-8">

<tomcat-users>

<role  rolename="manager-gui"/>

<user  username="itcast"  password="123" roles="manager-gui"/>

</tomcat-users>

## 4.tomcat配置虚拟主机

tomcat服务器允许用户在同一台计算机上配置多个web站点，在这种情况下，需要给每个web站点配置不同的主机名，这就是虚拟主机。好处：提高硬件资源的利用率，实现服务器之间的共享。

#### 4.1配置步骤

1.在tomcat的安装目录下的server.xml文件，

2.使用<host>元素来配置虚拟主机。它的属性name和appBase分别表示主机名和路径。添加新的在虚拟主机，在server.xml中<Engine>元素中增加一个<host>元素，将网站存放的目录配置为对应名称的主机即可，例如：将d:\itcast目录配置为一个名为itcast的主机名，

```
<Engine name="Catalina" defaulthost="localhost">
<host  name="itcast" appBase="d:\itcast">
</host>
</Engine>
```

### 4.2修改默认虚拟主机

在host节点的上面有一个Engine的父类，在其的属性中有一个defaulthost属性，将其的属性修改为其他的。如果访问的主机不存在，就会访问默认的虚拟主机。（Engine元素用于构建一个处理客户端请求的引擎，它接受tomcat的连接器传来的访问请求，进行具体的结果处理将结果返回给连接器）。例如，将ITcast配置成默认主机

```
<Engine  name="Catalina" defaulthost="itcast">
<host  name="itcast"  appBase="d:\itbase">
</host>
</engine>
```

在修改默认主机名后，要想被外界防伪，还要在DNS服务器或windows系统进行注册。当虚拟主机配置完毕，还要在hosts文件中配置虚拟主机与IP地址的映射关系。在system32\drivers\etc子目录下，将host文件中的127.0.0.1 localhost 修改为修改为默认的主机名。


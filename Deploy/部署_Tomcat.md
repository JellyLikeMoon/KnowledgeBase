# Java

### 官网

> [https://adoptium.net/zh-CN/temurin/releases/](https://adoptium.net/zh-CN/temurin/releases/)

### 安装步骤

```bash
tar zxvf OpenJDK17U-jdk_x64_linux_hotspot_17.0.8_7.tar.gz -C /opt
mv /opt/jdk_17.0.8.7 /opt/jdk17

#添加环境变量方式一
echo 'export JAVA_HOME=/opt/jdk-17' | sudo tee /etc/profile.d/java17.sh		# 
echo 'export PATH=$JAVA_HOME/bin:$PATH'|sudo tee -a /etc/profile.d/java17.sh 
source /etc/profile.d/java17.sh

#方式二
echo 'JAVA_HOME=/opt/jdk17' >> /etc/profile
echo 'PATH=$JAVA_HOME/bin:$PATH' >> /etc/profile
source /etc/profile

java -version

#查看默认java版本,enter切换默认java版本
update-alternatives --config java
update-alternatives --config javac
```

# Tomcat

### 官网

> [https://archive.apache.org/dist/tomcat/]()
>
> [https://tomcat.apache.org/]()

### 安装步骤

```bash
tar zxvf apache-tomcat-10.1.11.tar.gz -C /opt
mv /opt/tomcat-10.1.11 /opt/tomcat10

./opt/tomcat-10/bin/startup.sh                              #开启tomcat
./opt/tomcat-10/bin/shutdown.sh                             #关闭tomcat

#默认只能本机访问
http://localhost:8080

#配置文件
/opt/tomcat10/conf/server.xml                               #服务器相关设置
/opt/tomcat10/conf/web.xml                                  #网站相关设置
/opt/tomcat10/conf/tomcat-users.xml                         #添加登录账户可以登录tomcat manage管理界面
/opt/tomcat-10/webapps/manager/META-INF/context.xml         #添加IP允许跨主机访问，通过|间隔
```
### 命令

### 配置文件

`/opt/tomcat-10/bin/catalina.sh`

```bash
#                   Example (all one line)
#                   LOGGING_MANAGER="-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager"
#
#   UMASK           (Optional) Override Tomcat's default UMASK of 0027
#
#   USE_NOHUP       (Optional) If set to the string true the start command will
#                   use nohup so that the Tomcat process will ignore any hangup
#                   signals. Default is "false" unless running on HP-UX in which
#                   case the default is "true"
# -----------------------------------------------------------------------------

# OS specific support.  $var _must_ be set to either true or false.
JAVA_HOME=/opt/jdk-17.0.8						#首行添加java路径
CATALINA_HOME=/opt/tomcat-10					#首行添加tomcat路径
```

`/opt/tomcat10/conf/tomcat-users.xml`

```bash
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">

        <role rolename="admin-gui"/>
        <role rolename="manager-gui"/>
        <role rolename="admin-script"/>
        <role rolename="manager-script"/>
        <user username="admin" password="YUNtian@2023" roles="admin-gui,manager-gui，admin-script,manager-script"/>

</tomcat-users>
```

`/opt/tomcat10/conf/server.xml`

```bash
      <Host name="aaa.com" >							#域名或地址

        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
	<Context path="/book" docBase="/root/workland">		#path为使用URL访问路径，docBase为网站根目录

      </Host>
```

`/opt/tomcat-10/webapps/manager/META-INF/context.xml`

```bash
<Context antiResourceLocking="false" privileged="true" >
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|172.21.100.10" />		# 在“|”后添加允许访问的地址：： 
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```

‍

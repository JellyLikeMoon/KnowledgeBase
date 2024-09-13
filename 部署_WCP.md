- [安装必要环境](#安装必要环境)
  - [Maven](#maven)
  - [JDK](#jdk)
  - [Mysql](#mysql)
  - [Tomcat](#tomcat)
- [环境配置及部署](#环境配置及部署)
  - [源码下载](#源码下载)
  - [eclipse加载依赖](#eclipse加载依赖)
  - [数据库导入](#数据库导入)
  - [mysql -uroot -p123456 \< runoob.sql](#mysql--uroot--p123456--runoobsql)
  - [启动Tomcat](#启动tomcat)
  - [打包为war包](#打包为war包)

---

# 安装必要环境

## Maven

* URL

  ```bash
  https://maven.apache.org/download.cgi
  ```
* 环境变量

  ```bash
  MAVEN_HOME C:\Tools\apache-maven-3.9.0
  %MAVEN_HOME%\bin
  ```
* 验证

  ```bash
  mvn -v
  ```
* 国内源

  ```bash
  https://developer.aliyun.com/mvn/view

  ${maven_home}\conf\settings.xml

  <div>
  <mirror>
  <id>aliyun-public</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun public</name>
  <url>https://maven.aliyun.com/repository/public</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-central</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun central</name>
  <url>https://maven.aliyun.com/repository/central</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-spring</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun spring</name>
  <url>https://maven.aliyun.com/repository/spring</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-spring-plugin</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun spring-plugin</name>
  <url>https://maven.aliyun.com/repository/spring-plugin</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-apache-snapshots</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun apache-snapshots</name>
  <url>https://maven.aliyun.com/repository/apache-snapshots</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-google</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun google</name>
  <url>https://maven.aliyun.com/repository/google</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-gradle-plugin</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun gradle-plugin</name>
  <url>https://maven.aliyun.com/repository/gradle-plugin</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-jcenter</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun jcenter</name>
  <url>https://maven.aliyun.com/repository/jcenter</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-releases</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun releases</name>
  <url>https://maven.aliyun.com/repository/releases</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-snapshots</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun snapshots</name>
  <url>https://maven.aliyun.com/repository/snapshots</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-grails-core</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun grails-core</name>
  <url>https://maven.aliyun.com/repository/grails-core</url>
  </mirror>
  </div>

  <div>
  <mirror>
  <id>aliyun-mapr-public</id>
  <mirrorOf>*</mirrorOf>
  <name>aliyun mapr-public</name>
  <url>https://maven.aliyun.com/repository/mapr-public</url>
  </mirror>
  </div>
  ```

## JDK

* URL

  ```
  https://adoptium.net/zh-CN/temurin/archive/
  ```
* 环境变量

  ```bash
  JAVA_HOME C:\Tools\jdk-17.0.4.1+1
  %JAVA_HOME%\bin
  %JAVA_HOME%\jre\bin
  ```
* 验证

  ```bash
  java -v
  ```

## Mysql

* URL

  ```
  https://dev.mysql.com/downloads/mysql/
  ```
* 环境变量

  ```bash
  MYSQL_HOME C:\Tools\mysql-5.7.40-winx64
  %MYSQL_HOME%\bin

  create C:\Tools\mysql-5.7.40-winx64\data
  create my.ini

  [mysql]
  default-character-set=utf8
  [mysqld]
  port=3306
  basedir=C:\WCP\mysql-5.7.40-winx64
  datadir=C:\WCP\mysql-5.7.40-winx64\data
  max_connections=200
  character-set-server=utf8
  default-storage-engine=INNODB
  ```
* 命令

  ```bash
  mysqld --initialize-insecure
  mysqld -install
  mysqld -remove
  net start mysql
  net stop mysql
  ```

## Tomcat

* URL

  ```
  https://archive.apache.org/dist/tomcat/
  ```
* 环境变量

  ```bash
  CATALINE_HOME C:\Tools\apache-tomcat-7.0.99
  %CATALINE_HOME%\bin
  ```
* 启动

  ```bash
  C:\Tools\apache-tomcat-7.0.99\bin\startup.exe
  ```

# 环境配置及部署

## 源码下载

https://gitee.com/macplus/WCP

## eclipse加载依赖

- 导入源码
- 修改数据库接口文件

  wcp-web->java Resources->src\main\resources\jdbc.properties
- 修改eclipse的maven配置文件路径为

  ${maven_home}\conf\settings.xml
- 修改maven配置文件，仓库地址修改为与eclipse的maven仓库地址一致

  <localRepository>${user}\.m2\repository</localRepository>
- 导入install_IKAnalyzer.jar

  mvn install:install-file -DgroupId=org.wltea -DartifactId=IKAnalyzer -Dversion=2012 -Dpackaging=jar -Dfile=c:\IKAnalyzer-2012.jar
- 选中所有工程，更新maven

  maven  /update project

## 数据库导入

- 命令导入

  ## mysql -uroot -p123456 < runoob.sql

  $ mysqlimport -u root -p --local mytbl dump.txt

## 启动Tomcat

tomcat /publish
tomcat /start

http://localhost:8080/wcp

## 打包为war包

- 修改tomcat的server.xml文件，标签Engine之后添加

  <div>
  <Context docBase="wcp-web" path="/wcp" reloadable="true" source="org.eclipse.jst.jee.server:wcp-web"/>
  </div>
- 将war包放至tomcat\webapps目录下

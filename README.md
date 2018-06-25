# etoolkitsweb

## it is a deploy document of etoolkitsweb

* 部署环境
1. jdk1.6.0_20 64-Bit
2. jboss-4.0.2
3. os:Linux NA-eRating-37-191 2.6.32-696.6.3.el6.x86_64 #1 SMP Wed Jul 12 14:17:22 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

* 配置jboss环境变量
1. 修改 /etc/profile,添加如下内容

```
JBOSS_HOME=/home/jboss_gm/jboss-4.0.2

export PATH=$PATH:$JBOSS_HOME

````
2. 配置完成后验证
```
source /etc/profile

echo $JBOSS_HOME

输出：/home/jboss_gm/jboss-4.0.2 即表示jboss环境变量设置成功
```


* 在mysql中创建gmweb所需数据库和表，脚本见附件 init_gmweb.sql


* 修改/home/jboss_gm/jboss-4.0.2/server/default/deploy/gmApplicationDs.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	     xmlns:aop="http://www.springframework.org/schema/aop"
	     xmlns:tx="http://www.springframework.org/schema/tx"
	     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.0.xsd">
	
	<!-- 数据源定义-->
	<bean id="dataSource_0" class="org.springframework.jndi.JndiObjectFactoryBean">       
    	<property name="jndiName" value="java:gmweb2_gm"/>       
	</bean> 
	 <!-- 将xxx替换成游戏申请的gameid -->
	<bean id="dataSource_xxx" class="org.springframework.jndi.JndiObjectFactoryBean"> 
    	<property name="jndiName" value="java:gmweb2_linekong_xxx"/> 
	</bean>

</beans>

```


* 修改/home/jboss_gm/jboss-4.0.2/server/default/deploy/mysql-ds.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<!-- $Id: mysql-ds.xml,v 1.3.2.1 2004/12/01 11:46:00 schrouf Exp $ -->
<!--  Datasource config for MySQL using 3.0.9 available from:
http://www.mysql.com/downloads/api-jdbc-stable.html
-->

<datasources>
 
  <local-tx-datasource>
     <!-- gmweb数据库由上一步init_gmweb.sql初始化 -->
    <jndi-name>gmweb2_gm</jndi-name>
    <connection-url>jdbc:mysql://ip:port/egamemaster</connection-url>
    <driver-class>com.mysql.jdbc.Driver</driver-class>
      <user-name>xxx</user-name>
          <password>xxxx</password>
   <exception-sorter-class-name>org.jboss.resource.adapter.jdbc.vendor.MySQLExceptionSorter</exception-sorter-class-name>
    <metadata>
       <type-mapping>mySQL</type-mapping>
    </metadata>
  </local-tx-datasource>

  <!-- erating数据库库IP、用户名、密码配置需要跟erating开发同事确认 -->
  <local-tx-datasource>
    <!-- ???替换成申请的gameid -->
    <jndi-name>gmweb2_linekong_???</jndi-name>
    <connection-url>jdbc:mysql://ip:port/erating库名</connection-url>
    <driver-class>com.mysql.jdbc.Driver</driver-class>
    <user-name>xxx</user-name>
    <password>xxx</password>
    <exception-sorter-class-name>org.jboss.resource.adapter.jdbc.vendor.MySQLExceptionSorter</exception-sorter-class-name>
    <metadata>
       <type-mapping>mySQL</type-mapping>
    </metadata>
  </local-tx-datasource>

</datasources>

```


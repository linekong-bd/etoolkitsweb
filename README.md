# etoolkitsweb

## it is a deploy document of etoolkitsweb

* 部署环境要求
1. jdk1.6.0_20 64-Bit
2. jboss-4.0.2
3. os:Linux NA-eRating-37-191 2.6.32-696.6.3.el6.x86_64 #1 SMP Wed Jul 12 14:17:22 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

* 使用root用户创建jboss_gm用户

```
useradd -d /data/jboss_gm jboss_gm
```

* 将提供的jboss_gm.zip上传后解压
```
cd /data/jboss_gm
unzip jboss_gm.zip -d /data/jboss_gm
```

* 配置jboss环境变量
1. 修改 /etc/profile,添加如下内容

```
JBOSS_HOME=/data/jboss_gm/jboss-4.0.2
export PATH=$PATH:$JBOSS_HOME

````
2. 配置完成后验证
```
source /etc/profile

echo $JBOSS_HOME

输出：/data/jboss_gm/jboss-4.0.2 即表示jboss环境变量设置成功
```

* 修改 /data/jboss_gm/.bashrc 追加如下内容

```
export NLS_LANG=American_America.utf8
export NLS_DATE_FORMAT="YYYY-MM-DD HH24:MI:SS"
export LD_LIBRARY_PATH=$HOME/gmserver/lib/
unset ORACLE_HOME
export TNS_ADMIN=$PWD 
```

* 修改完成后执行 source /data/jboss_gm/.bashrc 使修改生效

* 在mysql中创建gmweb所需数据库和表，脚本见附件 egamemaster.sql

* 修改/data/jboss_gm/jboss-4.0.2/server/default/deploy/gmApplicationDs.xml 按照注释修改对应内容
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
	
	<bean id="dataSource_Oversea" class="org.springframework.jndi.JndiObjectFactoryBean">
        <property name="jndiName" value="java:gmweb2_oversea"/>
        </bean>
	
	<bean id="dataSource_bi" class="org.springframework.jndi.JndiObjectFactoryBean">
        <property name="jndiName" value="java:gmweb2_mysql_bi"/>
        </bean>
	
	 <!-- 将xxx替换成游戏申请的gameid -->
	<bean id="dataSource_xxx" class="org.springframework.jndi.JndiObjectFactoryBean"> 
    	<property name="jndiName" value="java:gmweb2_linekong_xxx"/> 
	</bean>

</beans>

```


* 修改/data/jboss_gm/jboss-4.0.2/server/default/deploy/mysql-ds.xml 按照注释修改对应内容
```
<?xml version="1.0" encoding="UTF-8"?>

<!-- $Id: mysql-ds.xml,v 1.3.2.1 2004/12/01 11:46:00 schrouf Exp $ -->
<!--  Datasource config for MySQL using 3.0.9 available from:
http://www.mysql.com/downloads/api-jdbc-stable.html
-->

<datasources>
 
  <local-tx-datasource>
    <jndi-name>gmweb2_gm</jndi-name>
     <!-- gmweb数据库由上一步1.egamemaster.sql初始化 请填写对应的ip和port-->
    <connection-url>jdbc:mysql://ip:port/egamemaster</connection-url>
    <driver-class>com.mysql.jdbc.Driver</driver-class>
      <!-- 用户名-->
      <user-name>xxx</user-name>
      <!-- 密码 -->
      <password>xxxx</password>
   <exception-sorter-class-name>org.jboss.resource.adapter.jdbc.vendor.MySQLExceptionSorter</exception-sorter-class-name>
    <metadata>
       <type-mapping>mySQL</type-mapping>
    </metadata>
  </local-tx-datasource>
  
  <local-tx-datasource>
    <jndi-name>gmweb2_oversea</jndi-name>
    <connection-url>jdbc:mysql://ip:port/oversea</connection-url>
    <driver-class>com.mysql.jdbc.Driver</driver-class>
    <user-name>xxx</user-name>
    <password>xxx</password>
    <exception-sorter-class-name>org.jboss.resource.adapter.jdbc.vendor.MySQLExceptionSorter</exception-sorter-class-name>
    <metadata>
       <type-mapping>mySQL</type-mapping>
    </metadata>
  </local-tx-datasource>
  
  <!-- bi数据库库IP、用户名、密码、以及数据库名配置需要跟bi开发同事确认 -->
  <local-tx-datasource>
    <jndi-name>gmweb2_mysql_bi</jndi-name>
    <connection-url>jdbc:mysql://ip:port/xxx</connection-url>
    <driver-class>com.mysql.jdbc.Driver</driver-class>
    <user-name>xxx</user-name>
    <password>xxx</password>
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


* 修改/data/jboss_gm/jboss-4.0.2/server/default/deploy/linekong-config.xml 按照注释修改对应内容

```
<?xml version="1.0" encoding="UTF-8"?>
<linekong>
<project name="egmweb2">
        <property name="activityCodeFilePath">/data/jboss_gm/jboss-4.0.2/activitycode</property>
        <property name="pwdPath">/data/jboss_gm/jboss-4.0.2/server/default/conf/gm/</property>
	<property name="gameKey">198=linekonghy</property>
	<property name="legalIP">79.110.87.85</property>
	<!-- ownerType参数与数据库中的sys_game表中的OWNER_TYPE 字段需要一致 -->
	<property name="ownerType">2</property>
	<property name="iosPushPath960">/home/pustking.p12</property>
	<property name="iosPushPwd960">lineking</property>
	<!-- 聊天监控需要找相关同事确认后修改 -->
        <property name="chatViewerUrl">http://gm.linekongsea.com:8089/eChatMonitor/</property>
        <property name="chatViewerKey">linekongline</property>
	<property name="phpkey">linekongkong</property>
	<!-- 将 gm.linekongsea.com:8082 修改成你们部署后gm的访问地址 -->
        <property name="unChargeUrl201">http://gm.linekongsea.com:8082/union_mid/unCharging.do</property>
        <property name="unChargeKey201">08b14732254d1e2187062a33dc828de5</property>
        <property name="unChargeUnionCode201">sszj_210</property>
</project>
</linekong>
```


* 修改 /data/jboss_gm/gmserver/gmserver/erating_mysql_db.ini

```
#Server连接mysql数据库时，必须要配置的erating数据源,需跟erating开发同事确认
#数据源信息 host，port，user，pwd，db
#格式：xxx_游戏ID=xxx
#密码格式为解密后的密码

host_210=xxxx
port_210=xxxx
user_210=xxxx
pwd_210=xxxx
db_210=xxxx
```

* 修改 /data/jboss_gm/gmserver/gmserver/gmserver.ini中如下对应的内容，其余内容保持不变

```
game_id=xxx   //游戏申请的gameid                                       
host_name=xxxx  //egamemaster数据库ip                                       
db_port=xxxx    //egamemaster数据库port                                   
user_name=xxxx  //egamemaster数据库用户名                                 
password=xxxxx  //egamemaster数据库密码             
```

* 启动GMSvr

```
cd /data/jboss_gm/gmserver/gmserver/
./GMSvr2.50 `pwd`
```

* 启动gmweb

```
cd /data/jboss_gm/jboss-4.0.2/bin
./run.sh &
```

* 更新gmweb应用程序
```
注意使用jboss_gm用户操作
将war包上传到 /data/jboss_gm/war/update 目录
/data/jboss_gm/update_jboss_2.sh update 60f6d2173a43d9e5e1a31006a37cf9e0（每次更新war包的md5值都会变）
```

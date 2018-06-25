# etoolkitsweb

## it is a deploy document of etoolkitsweb

* 部署环境
1. jdk1.6.0_20
2. jboss-4.0.2

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


* 修改/home/jboss_gm/jboss-4.0.2/server/default/deploy/mysql-ds.xml
```

```

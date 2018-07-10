
### Mysql兼容性问题 出现版本5.1与5.7

```
  描述：在5.1上创建的视图在5.7版本的数据库上,建表sql运行出错
  
  error: 2 of SELECT list is not in GROUP BY clause and contains nonaggregated column

  解释：select字段必须都在group by分组条件内（含有函数的字段除外）。（如果遇到order by也出现这个问题，同理，order by字段也都要在group by内）

  link:http://blog.csdn.net/u010429286/article/details/64444271
  
  解决方法
   1.更换mysql版本
   2,mysql>seelect @@sql_mode  将字符中的ONLY_FULL_GROUP_BY去除，重新设置,set @@sql_mode，重启服务(推荐使用配置文件去修改)
   3.mysql配置文件修改或添加 sql_mode = "STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION" (原理与2一样)，重启服务。
``` 
        

### centos7 mysql安装

```
1.yum -y install wget(安装wget)
2.wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
3.yum -y install mysql57-community-release-el7-10.noarch.rpm
4.yum -y install mysql-community-server
5.systemctl start  mysqld.service 
6.grep "password" /var/log/mysqld.log(查看root初始密码)
7.mysql -u root -p
8.设置密码
9.GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION; (所有机器访问mysql)
10.flush privileges;
11.防火墙端口开放 或者 直接关闭防火墙

取消密码复杂度:
set global validate_password_policy=0; (取消密码字符数据复杂度)
set global validate_password_length=1; (设置密码长度)(需要先修改root初始密码)
```

### Mysql 5.7中用来代替触发器的功能

```
# 5.7 新增计算列功能
# 5.7之前依靠触发器
CREATE TABLE t (
	id INT auto_increment NOT NULL,
	c1 INT DEFAULT NULL,
	c2 INT DEFAULT NULL,
	c3 INT DEFAULT NULL,
	PRIMARY KEY (id)
)
create TRIGGER inst_t before insert on t for each row set new.c3=new.c1+new.c2;
show triggers;
insert into t(c1,c2)values(1,2);
create trigger upd_t before update on t for each row set new.c3=new.c1+new.c2;
update t set c1=4 where id=1;
# 或创建view去计算
create view vm_t as select id,c1,c2,c1+c2 as c3 from t;

#5.7的做法
create table  b(
 id int auto_increment not null,
 	c1 INT DEFAULT NULL,
	c2 INT DEFAULT NULL,
	c3 INT as(c1+c2),
PRIMARY key(id)
)
insert into b(c1,c2)values(2,4);
```

### mysql允许root用户远程连接
```
update mysql.user set Host='%' where HOST='localhost' and User='root';
flush privileges;
```

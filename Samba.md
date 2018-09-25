### Samba

[http://blog.51cto.com/12173069/2068817]

1.安装
yum install -y samba

2.创建共享目录
mkdir /home/share

3.禁用selinux
setenforce  0

4.修改配置文件：
vim  /etc/samba/smb.conf  #添加
```
[root]
comment = root
path = /home/share
browseable = yes
guest ok = no
writable = yes
```

5.配置账号
smbpasswd  -a  root

```angularjs
smbpasswd命令说明：
-a  添加
-x  删除
-d  禁用
-e  启用
```

6.启动服务：
systemctl  start  smb

7.开放端口
```angularjs
iptables  -I  INPUT  -p  tcp  --dport  139  -j  ACCEPT
iptables  -I  INPUT  -p  tcp  --dport  445  -j  ACCEPT
iptables  -I  INPUT  -p  udp  --dport  137  -j  ACCEPT
```

8.window打开
\\ip



9.安装客户端
yum install -y samba-client

10. 查看服务器的共享信息：
smbclient  -L  //ip

11.连接服务器的共享目录
smbclient -U root //ip/home/share

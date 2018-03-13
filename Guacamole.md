### 开源项目Guacamole安装过程中遇到的坑

guacd日志 /var/log/message

```
guacd[1615]: Failed to load guacdr plugin. Drive redirection and printing will not work. Sound MAY not work.
guacd[1615]: Failed to load guacsnd alongside guacdr plugin. Sound will not work. Drive redirection and printing MAY not work.

解决方案1（用于源码安装freerdp）:
ln /usr/local/lib/freerdp/guac*.so /usr/local/lib64/freerdp/
或
ln /usr/local/lib/freerdp/guac*.so  /usr/lib64/freerdp/
(实际只需要guacdr 和guacsnd)

解决方案2（用于yum安装freerdp）:
cp /home/guacamole-server-0.9.13-incubating/src/protocols/rdp/.libs/guacdr.so /usr/lib64/freerdp/
cp /home/guacamole-server-0.9.13-incubating/src/protocols/rdp/.libs/guacsnd.so /usr/lib64/freerdp/
```

```
0.9.14版guacamole-server在安装后，freerdp内组件不完全导致clipboard等不完全，推荐使用源码安装freerdp
```




  
  

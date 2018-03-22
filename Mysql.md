
### Mysql兼容性问题 出现版本5.1与5.7

描述：在5.1上创建的视图在5.7版本的数据库上,建表sql运行出错
error: 2 of SELECT list is not in GROUP BY clause and contains nonaggregated column
解释：select字段必须都在group by分组条件内（含有函数的字段除外）。（如果遇到order by也出现这个问题，同理，order by字段也都要在group by内）
link:http://blog.csdn.net/u010429286/article/details/64444271
解决方法：1.更换mysql版本
         2,mysql>seelect @@sql_mode  将字符中的ONLY_FULL_GROUP_BY去除，重新设置,set @@sql_mode，重启服务
         3.mysql配置文件修改或添加 sql_mode = "STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION" (原理与2一样)，重启服务。
        

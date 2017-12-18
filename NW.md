#### NW在xp上的解决方案
```
1.nw版本限制在0.14.7与0.13.4两个版本
2.xp电脑中查看ipconfig -all 查看是否存在DNS设置，若无，nw应用会因为调用http，log4js，request等模块发生闪退的状况，这是由nw的缺陷造成的.
3.详情 https://github.com/nwjs/nw.js/issues/4855
```

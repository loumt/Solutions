#### nodejs解决cors问题,设置header方式

```
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", ['http://192.168.16.4:3005','http://192.168.16.3:3005']);
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1');
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});
```

#### nodejs项目package.json变量文档

```
[link]https://docs.npmjs.com/misc/scripts#packagejson-vars
```
[link]https://docs.npmjs.com/misc/scripts#packagejson-vars

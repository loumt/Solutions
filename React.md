### 1.React数据获取为什么一定要在componentDidMount里面调用？
```
 不建议在constructor和componentWillMount里面调用的原因：
  1- 会阻碍组件的实例化，阻碍组件的渲染
  2- 如果用setState，在componentWillMount里面触发setState不会重新渲染
```
 


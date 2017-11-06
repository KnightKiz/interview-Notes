# 前端笔试面试笔记
### es6中箭头函数中的this
es6的箭头函数中没有this对象，所以在箭头函数中引用this时实际引用的是外层代码块的this，而且箭头函数中的this是在函数定义的时候绑定的，不是在调用的时候绑定。
```
let x = 11;
let obj = {
  x: 22,
  say: () => {console.log(this.x);}
};
obj.say();
// 11
```
输出22，因为say函数由obj对象定义，此时this指向window，因此输出11

### 前端路由实现原理
#### history api
html5新增了history对象的两个函数，pushState(),replaceState(),两个函数都接收三个变量
- 状态对象（state object）
- 标题（title）传空值最好
- 地址（url）新url
例
```
window.history.pushState(null, null, "https://www.baidu.com/name/orange");
//url: https://www.baidu.com/name/orange
window.history.pushState(null, null, "?name=orange");
//url: https://www.baidu.com?name=orange
```
通过历史记录切换时触发的popState事件修改dom(调用popState,replaceState不会触发，历史记录改变才会触发,如history.go,history.back...)
```
onpopState = function(event){
  setPage(event.state);
}
function setPage(page){
  currentPage = page;
  document.title = 'Line Game - ' + currentPage;
  document.getElementById('coord').textContent = currentPage;
  document.links[0].href = '?x=' + (currentPage+1);
  document.links[0].textContent = 'Advance to ' + (currentPage+1);
  document.links[1].href = '?x=' + (currentPage-1);
  document.links[1].textContent = 'retreat to ' + (currentPage-1);
}
```
#### hash
url上的#及后的值为hash，window.location.hash可以修改url上的#及后面的值，而不会引起页面重新渲染，通过hashchage事件可以监听hash值的变化，从而动态改变页面。

在低版本的ie中可以通过setInterval长轮询监听hash值是否发生变化。

大型前端路由系统都是用hash来实现的


### 移动端点投问题
移动端的click事件会延迟300ms，会后于touchstart,touchend事件发生，因此可能会导致触屏时触发到当前元素后的元素的click事件

解决：采用touch事件代替click事件，或者采用faskclick等插件

### 浏览器缓存
#### cache-control/expires
两个都规定了最大约定时间
cache-control中max-age设定了缓存有效时间，expires设定了缓存失效时间

max-age优先级比expries高

#### Last-Modified/If-Modified-Since
需要配合Cache-control使用
Last-Modified 存放上一次更新时间
If-Modified-Since 表示发送最后一次更新时间，服务器查看和文件最后一次修改时间是否相同，相同返回304，不同200和文件数据，并修改当前值

#### ETag/If-None-Match
需要配合Cache-control使用
Etag 表示该文件的唯一字符串的hash值，若缓存有效时间超过了，请求头则会发送If-None-Match，值为Etag相同值，服务器用这个值进行对比，判断文件是否修改，修改了则返回200和文件数据，更新Etag值，没修改则返回304

### vue和react,Angular对比


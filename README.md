# interview-Notes
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


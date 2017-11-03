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

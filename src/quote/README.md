# 引用变量的坑

什么是引用类型变量？

答:引用类型的值都是对对象的引用，即一个指向对象的指针。



然而，too young too simple,事实这个坑很大，首先来看个例子
```js
const obj = {a:1,b:2};
obj = 2;
// Uncaught TypeError: Assignment to constant variable.
obj.a = 3;
console.log(obj);
// {a: 3, b: 2}
```
尼玛，说好的const声明的变量不能被更改的吗？

细细一分析其实obj的引用地址一直没变，哇擦，这个坑我认。



再来看个例子：
```js
const obj = {a:1,b:2};
setTimeout(()=>{
    obj.a = 3;
},0);
console.log(obj.a);
// 1
```
明明a已经被改成3了，尼玛，为什么打印出来还是1？

其实这个坑是因为js是单线程,而setTimeout是js空闲时运行，所以即使setTimeout设置为0秒后立即运行还是得等js空闲时运行的。

所以上面的执行顺序实际是这样子的：
```js
const obj = {a:1,b:2}; // 顺序1
console.log(obj.a);    // 循序2
setTimeout(()=>{       // 顺序3
    obj.a = 3;
},0);
// 1
```



那我要怎么才能看到a已经被改成3了呢？
```js
const obj = {a:1,b:2};
setTimeout(()=>{
    obj.a = 3;
    console.log(obj.a,'001');
},0);
console.log(obj.a,'002');
// 1 "002"
// 3 "001"
```



要想把引用类型彻底搞懂就要不断深入，再来个例子
```html
// html
const obj = {a:1,b:2};
console.log(obj.a);
obj.a = 3;
console.log(obj.a);

// {a:3,b:2}
// {a:3,b:2}
```
```js
// 控制台
const obj = {a:1,b:2};
console.log(obj.a);
obj.a = 3;
console.log(obj.a);

// {a:1,b:2}
// {a:3,b:2}
```
把这段代码放到页面里面和放到chrome控制台上执行，看到的结果竟然是不一样的。

this is why?

为什么？


[因为代码在运行的时候控制台没有打开。
首先明确一点，obj所储存的是一个引用类型值的地址，所有对obj的操作都会具体到这个地址所对应的那个对象上。其次，console并不是JavaScript提供的对象，而是浏览器的控制台提供的。
这具体到不同的浏览器，比如Chrome中是由Devtool的控制台提供，Firefox中是由Firebug的控制台提供。
在Chrome中，console.log在控制台打开后才起作用，也就是说，当你打开控制台时，console.log才会将之前被传进去的参打印出来。](https://www.cnblogs.com/sevenskey/p/5476386.html)


那么该如何解决这个问题？只能在执行的过程中创建新对象来曲线救国了：
```js
const obj = {a:1,b:2};
console.log(JSON.parse(JSON.stringify(obj.a)));
obj.a = 3;
console.log(JSON.parse(JSON.stringify(obj.a)));
```



=========================

如有错误，望指正。谢谢。
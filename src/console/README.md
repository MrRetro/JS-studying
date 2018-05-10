# console的用法

### console.log()
```js
//控制台打印信息
console.log(1)
```

### console.dir()
```js
//控制台打印,能把属性和方法打印出来 --以json格式输出
console.dir([1,2,3]);
```

### console.debug()[CHROME没效果]、console.info()、console.warn() 与 console.error()
```js
//用法和console.log一样就是颜色不一样
console.log("log");
console.debug("debug");
console.info("info");
console.warn("warn");
console.error("error");

```

### console.table()
```js
//以表格的形式打印出
const people = {
    "person1": {"fname": "san", "lname": "zhang"},
    "person2": {"fname": "si", "lname": "li"},
    "person3": {"fname": "wu", "lname": "wang"}
};
console.table(people);
```

### console.time() 与 console.timeEnd()
```js
//在调试时，我们经常需要知道一段代码执行时间，我们可以使用这两行代码来实现。
console.time("for-test");
const arr = [];
for(let i = 0; i < 100000; i++) {
    arr.push({"key": i});
}
console.timeEnd("for-test");
```

### console.assert()
```js
//console.assert() 类似于单元测试中的断言，当表达式为 false 时，输出错误信息
const arr = [1, 2, 3];
console.assert(arr.length === 4);
```

### console.count()
```js
//调试代码时，我们经常需要知道一段代码被执行了多少次，我们可以使用 console.count() 来方便的达到我们的目的
function func() {
    console.count("label");
}

for(let i = 0; i < 3; i++) {
    func();
}
```

### console.group、console.groupEnd() 与 console.groupCollapsed()
```js
//一般的 console.log() 方法的输出没有层级关系，在需要一些显示层级关系的输出中显得苍白无力，使用 console.group() 可以达到我们的目的
//把 "group" 换成 "groupCollapsed"，则默认为折叠运行结果(测试没效果)。
console.group("1");
console.log("1-1");
console.log("1-2");
console.log("1-3");
console.groupEnd();
console.group("2");
console.log("2-1");
console.log("2-2");
console.log("2-3");
console.groupEnd();
```

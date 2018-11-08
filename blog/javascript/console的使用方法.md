<center><h2>console的使用方法</h2></center>

#### 基本输出

```javascript
console.log('打印字符串')     // 在控制台打印自定义字符串
console.error('我是个错误')   // 在控制台打印自定义错误信息
console.info('我是个信息')    // 在控制台打印自定义信息
console.warn('我是个警告')    // 在控制台打印自定义警告信息
console.debug('我是个调试')   // 在控制台打印自定义调试信息
console.clear()             // 清空控制台
```

![1](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/1.jpg)

不知道为什么我在Chrome中 `console.log('我是个调试')` 有问题



#### 格式化输出

`console`还支持自定义样式和类似c语言的 print形式

```javascript
console.log('%s'年, 2018)            // %s表示字符串
console.log('%d年%d月', 2018, 11)    // %d表示整数
console.log('%f', 3.1415)           // %f表示小数
console.log('%o', console)          // %o表示对象
console.log('%c自定义样式', 'font-size: 30px; color: #00f')
console.log('%c我是%c自定义样式', 'font-size: 30px; color: green', 'font-size: 10px; color: red')
```

![2](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/2.png)



#### DOM 输出

```javascript
let ul = document.getElementsByTagName("ul")
console.dirxml(ul) //树形输出ul节点，即<ul>和它的innerHTML，由于document.getElementsByTagName是动态的，所以这个得到的结果肯定是动态的
```

经测试和 `console.log`输出DOM节点是一样的



#### 对象输出

```javascript
let o = {
    name: 'lele',
    age: '20'
}
console.dir(o) //显示对象自有属性和方法
```

经测试和 `console.log`输出对象是一样的



多个对象的集合，可以这样输出

```javascript
let stu = [{name: 'simple', age: 14}, {name: 'jack', age: 23}, {name: 'lucy', age: 23}]
console.log(stu)
console.table(stu)
```

![3](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/3.png)



#### 成组输出

```javascript
// 建立一个参数组
console.group('groupName')       // 引号里是组名
console.log('sub1')
console.log('sub2')
console.log('sub3')
console.groupEnd('end')
```

![4](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/4.png)



#### 函数计数和跟踪

```javascript
function fib (n) {  // 输出前n个斐波那契数列值
    if (n == 0) return;
    console.count('调用次数')     // 放在函数里，每当这句代码运行 输出所在函数执行次数
    console.trace()              // 显示函数调用轨迹(访问调用栈）
    let a = arguments[1] || 1
    let b = arguments[2] || 1
    console.log('fib=' + a);
    [a, b] = [b, a + b]
    fib(--n, a, b)
}
fib(6)
```

![5](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/5.png)



**真正会导致上下行解析出问题的 token 有 5 个：括号，方括号，正则开头的斜杠，加号，减号, 所以上述代码中[a, b] = [a, a+b]的上一句一定要加分号，否则会导致上下行解析出问题**



#### 计时

```javascript
console.time()           // 计时开始
fib(100)                 // 用上述函数计算100个斐波那契数
console.timeEnd()        // 计时结束并输出时长
```

![6](/Users/simple/Documents/code/git/git_my/markDownImages/images/javascript/console的使用方法/6.png)


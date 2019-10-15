#javaScript学习笔记

##基础用法

##函数
```js
//定义函数
function abs(x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```
由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：
若不传入参数，则返回NaN

####arguments关键字
只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数（类似Array但不是Array）
```js
funciton abs(){
    if(arguments.length ===0){
        return 0;
    }
    var x = arguments[0];
    return x>=0?x:-x;
}

abs(); //0
abs(10); //10
abs(-9); //9
```

####rest参数
```js
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```
###方法

在一个对象中绑定函数，称为方法。

```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
```
this是一个特殊变量，始终指向当前对象。若函数在对象内部声明，则指向该对象。若在外部声明，则指向调用该方法的对象。

####apply
要指定函数的this指向哪个对象，可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。

```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

另一个与apply()类似的方法是call()，唯一区别是：

+ apply()把参数打包成Array再传入；

+ call()把参数按顺序传入。

###高阶函数

####map

map()方法定义在JavaScript的Array中，我们调用Array的map()方法，传入我们自己的函数，就得到了一个新的Array作为结果

```js
function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
```
####reduce
Array的reduce()把一个函数作用在这个Array的[x1, x2, x3...]上，这个函数必须接收两个参数，reduce()把结果继续和序列的下一个元素做累积计算

```js
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
```

####filter(把Array的某些元素过滤掉)

Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

```js
//删掉偶数，只保留奇数
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

关于回调函数

```js
var arr = ['A', 'B', 'C'];
var r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});
//-------------------------
//利用filter去重
var
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
    
    
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

####sort

自定义实现排序算法

```js
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]
```

####闭包
```js
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count();
var f1 = results[0];//16
var f2 = results[1];//16
var f3 = results[2];//16
```
返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。

```js
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9
//---------------------------
//创建一个匿名函数并立刻执行
(function (x) {
    return x * x;
})(3); // 9
```

用闭包实现私有变量
```js
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}

var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13
```

在返回的对象中，实现了一个闭包，该闭包携带了局部变量x，并且，从外部代码根本无法访问到变量x。换句话说，闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来。

####generator（生成器）

```js
//实现斐波那契数列
function* fib(max) {
    var
        t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a + b];
        n ++;
    }
    return;
}
-------
//调用方式一：
var f = fib(5);
f.next(); // {value: 0, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 2, done: false}
f.next(); // {value: 3, done: false}
f.next(); // {value: undefined, done: true}
//调用方式二：
for (var x of fib(10)) {
    console.log(x); // 依次输出0, 1, 1, 2, 3, ...
}
```
###标准对象
+ 不要使用new Number()、new Boolean()、new String()创建包装对象；

+ 用parseInt()或parseFloat()来转换任意类型到number；

+ 用String()来转换任意类型到string，或者直接调用某个对象的toString()方法；

+ 通常不必把任意类型转换为boolean再判断，因为可以直接写if (myVar) {...}；

+ typeof操作符可以判断出number、boolean、string、function和undefined；

+ 判断Array要使用Array.isArray(arr)；

+ 判断null请使用myVar === null；

+ 判断某个全局变量是否存在用typeof window.myVar === 'undefined'；

+ 函数内部判断某个变量是否存在用typeof myVar === 'undefined'。

####JSON
序列化
```js
var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp']
};
//将对象转化为json形式
var s = JSON.stringify(xiaoming);
console.log(s);
```
反序列化
```js
var obj = JSON.parse('{"name":"小明","age":14}', function (key, value) {
    if (key === 'name') {
        return value + '同学';
    }
    return value;
});
console.log(JSON.stringify(obj)); // {name: '小明同学', age: 14}
```

##浏览器

####浏览器对象
+ window  不但充当全局作用域，而且表示浏览器窗口
+ navigator 表示浏览器的信息
+ screen 表示屏幕的信息
+ location 表示当前页面的URL信息（一个完整的rul,可以用location.href获取）
```
其他属性
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```
+ document 表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，document对象就是整个DOM树的根节点。
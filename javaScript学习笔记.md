#javaScript学习笔记

##基础用法

##函数
```
定义函数
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
```$javaScript
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
```$xslt
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

```$xslt
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
```
this是一个特殊变量，始终指向当前对象。若函数在对象内部声明，则指向该对象。若在外部声明，则指向调用该方法的对象。

####apply
要指定函数的this指向哪个对象，可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。

```$xslt
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

```$xslt
function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
```
####reduce
Array的reduce()把一个函数作用在这个Array的[x1, x2, x3...]上，这个函数必须接收两个参数，reduce()把结果继续和序列的下一个元素做累积计算

```$xslt
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
```

####filter(把Array的某些元素过滤掉)

Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

```$xslt
删掉偶数，只保留奇数
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

关于回调函数

```$xslt
var arr = ['A', 'B', 'C'];
var r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});
-------------------------
利用filter去重
var
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
    
    
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

####sort

自定义实现排序算法

```$xslt
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

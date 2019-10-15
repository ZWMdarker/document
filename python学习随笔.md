#Python 学习随笔
## python基本语法方法
###通用方法
+ range()							-------生成一些列数字
+ list(range(1,9))				-------生成数字列表(1-8)
###字符串
+ name.title()					-------字符串首字母大写
+ name.upper()					-------字符串全部大写
+ name.lower()					-------字符串全部小写
+ name.split(xxx)				-------以xxx返回分析文本(不传为空格)，返回列表

###列表[]
+ cars.append(”bicycle“)		-------列表后追加元素
+ cars.insert(0，”bicycle“)	-------列表插入元素
+ del cars[0]						-------删除列表元素
+ bicycle = car.pop(0)			-------取出列表中某个位置元素（默认列表尾部）
+ cars.remove("bicycle")		-------根据值删除列表某些元素
+ cars.sort()						-------列表永久性排序
+ cars.sorted()					-------列表临时排序
+ cars.reverse()					-------列表倒序
+ len(cars)						-------确定列表长度
+ cars[0:3]						-------列表切片（未指定即为从头或者从尾，负数为从右侧起）
+ carlist = cars[:]				-------复制列表

####数字列表
+ min(list)						-------数字列表最小值
+ max(list)						-------数字列表最大值
+ sum(list)						-------数字列表求和

+ 元组()							-------与列表基本一样但不可编辑修改

###if语句
+ if list: 						-------判断列表是否为空

###字典（map）
```
定义字典
user = {"name": "zhang", "age": 16}
遍历字典所有键值对
for key,value in user.items():
	print("key:" + key + ",value:" + value)
遍历字典所有的key
for name in user.keys():
	pring(name)
遍历字典所有的value
for name in user.values():
	pring(name)
```

+ user["sex"] = "男"				-------添加键值对
+ user["age] = 20				-------修改值
+ del user["age"]				-------删除键值对

###While 循环
message = input("请输入文字") 	-------用户输入数据并赋值给message
age = int(message)					-------将输入的值转化为int类型

##关于函数
###特殊用法
```
设置参数默认值
def describe_pet(name,animal='dog'):
	print(”The pet‘s name is "+ name + ",pei is" + animal)
	
传入不定数量参数(可传入一个或多个参数，函数可把形参转换为一个元组)
def make_pizza(*toppings):
	for topping in toppings:
		print(topping)

传入不定数量键值对
def build_proflie(first, last , **user_info):
	profile={}
	profile['first_name']= first
	profile['last_name']= last
	for key,value in user_info:
		profile[key] = value
		
	return profile
```

###函数在其他文件
```
pizza.py
-------
def make_pizza(){
	print(“做一个pizza”)
	
main.py
-------
import pizza

pizza.make_pizza()
```

+ from pizza import make_pizza as mp 	------- 导入方法时添加别名

+ from pizza import * 					-------导入模块所有函数

+ import pizza as p						-------导入模块并添加别名	
###Python标准库
+ collections模块的OrderdDict类  		-------记录字典的顺序

##文件和异常
###读文件
```
读取整个文件（会产生一个空行，可以用contents.rstrip()方法去除）(with作用是在合适的时候关闭文件)
with open("/Users/zhangwenmeng/Public/code/test/test.txt") as file:
    contents = file.read()
    print(contents)
    
逐行读取文件
with open("/Users/zhangwenmeng/Public/code/test/test.txt") as file:
    for line in file:
	    print(line)

逐行读取并形成列表
with open("/Users/zhangwenmeng/Public/code/test/test.txt") as file:
	lines = file.readlines()
```


###写文件
```
写入文件（打开文件时可指定模式，“r”为读取模式、“w”为写入模式、“a”为附加模式--追加写入文件）
with open("/Users/zhangwenmeng/Public/code/test/test.txt","w") as file:
    file.write("loveyou")
```

###关于异常
```
try:
	print(9/0)
except ZeroDivisionError:
	print("除数不为零“)
else:
	print("成功了")
```

###关于json数据转化
```
转化成json格式并写入文件
nums=[2,4,3,6,5,0]
with open("/Users/zhangwenmeng/Public/code/test/test.json","w") as file:
    json.dump(nums,file)
    
读取文件并解析json
with open("/Users/zhangwenmeng/Public/code/test/test.json") as file:
	nums = json.load(file)
	print(nums)
```

###列表生成式
```
写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来，十分有用，多写几次，很快就可以熟悉这种语法。

for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方：

>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
还可以使用两层循环，可以生成全排列：

>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```


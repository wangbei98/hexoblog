title: python函数的参数类型
author: bellick
tags:
  - python
categories:
  - Notebook
  - Python
  - ''
date: 2018-11-14 10:23:00
---
#### 函数的参数
##### 默认参数
	
	def enroll(name, gender, age=6, city='Beijing'):
	    print('name:', name)
	    print('gender:', gender)
	    print('age:', age)
	    print('city:', city)
   
！定义默认参数要牢记一点：默认参数必须指向不变对象！
	
	def add_end(L=None):
	    if L is None:
	        L = []
	    L.append('END')
	    return L
	    
##### 	可变参数

1. 要定义出这个函数，我们必须确定输入的参数。由于参数个数不确定，我们首先想到可以把a，b，c……作为一个list或tuple传进来，这样，函数可以定义如下：
```
      def calc(numbers):
          sum = 0
          for n in numbers:
              sum = sum + n * n
          return sum

      >>> calc([1, 2, 3])
      14
      >>> calc((1, 3, 5, 7))
      84
```
 
2. 如果利用可变参数，调用函数的方式可以简化成这样：

```
      def calc(*numbers):
          sum = 0
          for n in numbers:
              sum = sum + n * n
          return sum
      >>> calc(1, 2, 3)
      14
      >>> calc(1, 3, 5, 7)
      84
      >>> nums = [1, 2, 3]
      >>> calc(*nums)
      14
```

###### *nums表示把nums这个list的所有元素作为可变参数传进去。这种写法相当有用，而且很常见。

##### 关键字参数

	def person(name, age, **kw):
	    print('name:', name, 'age:', age, 'other:', kw)
	    
	>>> person('Bob', 35, city='Beijing')
	name: Bob age: 35 other: {'city': 'Beijing'}
	>>> person('Adam', 45, gender='M', job='Engineer')
	name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
	
	>>> person('Jack', 24, city=extra['city'], job=extra['job'])
	name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
当然，上面复杂的调用可以用简化的写法：

	>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
	>>> person('Jack', 24, **extra)
	name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
\*\*extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra。

##### 命名关键字函数

	def person(name, age, *, city, job):
	    print(name, age, city, job)
	    和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。

调用方式如下：

	>>> person('Jack', 24, city='Beijing', job='Engineer')
	Jack 24 Beijing Engineer
命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错
##### 参数组合
在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

比如定义一个函数，包含上述若干种参数：

	def f1(a, b, c=0, *args, **kw):
	    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)
	
	def f2(a, b, c=0, *, d, **kw):
	    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
# 【Python】类变量和实例变量
#人工智能/python

> 我们知道，在Java里面类变量(static变量)和类属性(普通属性)是有严格的区分的，那么在Python里面，如何定义类变量和类属性的呢？  


先看一个code demo
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*
"""
:author:
    Lou yuting
:create_date:
    2017
:descrition:
        1. python的类变量与实例变量以及__dict__属性
"""
class cls:
    # 类变量
    clsvar = 1
    def __init__(self, val=1):
        # 实例变量
        self.insvar = val

ins1 = cls(2)
ins2 = cls(2)

print(cls.__dict__)
print(ins1.__dict__)
print(ins2.__dict__)

# 用实例1为类变量重新赋值并打印
print("#" * 10)
ins1.clsvar = 20
print(cls.clsvar)# 输出1
print(ins1.clsvar)# 输出20
print(ins2.clsvar)# 输出1

#用类名为类变量重新赋值并打印
print("#" * 10)
cls.clsvar = 10
print(cls.clsvar)# 输出10
print(ins1.clsvar)# 输出20
print(ins2.clsvar)# 输出10

#这次直接给实例1没有在类中定义的变量赋值
print('#'*10)
ins1.x = 11
print(ins1.x)#输出结果为11
#然后再用类名给类中没有定义的变量赋值
print('#'*10)
cls.m = 21
print(cls.m)#输出结果为21
#再创建一个实例ins3，然后打印一下ins3的变量
print('#'*10)
ins3 = cls()
print(ins3.insvar)    #输出结果为2
print(ins3.clsvar)    #输出结果为10
print(ins3.m)         #输出结果为21
print(ins3.x)         #报错AttributeError: cls instance has no attribute 'x'
```

不管是对于Python的类对象还是类的实例对象，其都有自己独立的__dict__ 对象，这个对象是一个字典，键是属性名，值为属性值。这里的属性名也就是我们定义在类里面的类变量或则是实例变量。

当我们 `print(__dict__)` 时候，对于类和Instance会有不同的值：
* 当打印类的__dict__属性时，列出了类所包含的属性，包括一些类内置属性和类变量以及构造方法__init__

* 当打印某个实例ins1的__dict__属性时，实例变量则包含在实例对象ins1的__dict__属性中，

 ::一个实例对象的属性查找顺序遵循：首先查找实例对象自己，然后是类，接着是类的父类。::

::从上面可以看出，对于类属性，只有类能够改变，类的实例可以访问类属性，当实例想去更改类的属性时，其实是在实例的__dict__里面新增了这个属性::









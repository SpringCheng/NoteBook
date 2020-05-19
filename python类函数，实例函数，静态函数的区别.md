**python类函数，实例函数，静态函数的区别**

一、实现方法

```python
class Function(object):
    # 在类定义中定义变量
    cls_variable = "class varibale"

    def __init__(self):
        # 在构造函数中创建变量
        self.__instance_variable = "instance variable"

    def instance_method(self):
        print(self.cls_variable)
        print(self.__instance_variable)
        print("this is a instance method")

    @staticmethod
    def static_method():
        print(Function.cls_variable)
        # print(Function.__instance_variable)    此处会报错，无法访问实例变量
        print("this is a static method")

    @classmethod
    def class_method(cls):
        print(cls.cls_variable)
        # print(cls.__instance_variable)   此处会报错，无法访问实例变量
        print("this is a class method")

    @classmethod
    def set_class_variable(cls):
        cls.cls_variable = 'new class variable'

    def set_instace_varibale(self):
        self.__instance_variable = 'new instance varibale'

#  类实例可以调用类方法和静态方法
function1 = Function()
function1.set_class_variable()
function1.class_method()
function1.instance_method()
function1.static_method()

function2 = Function()
function2.set_instace_varibale()
function2.class_method()
function2.instance_method()
function2.static_method()
```

1、从代码定义中，可以看到只是在默认传入参数的不同。

```python
Function.class_method()
Function.static_method()
# 可以调用实例函数，只不过需要传入实例变量
Function.instance_method(function1)
```

2、从代码访问中，通过实例访问这三种方法是一样的。但是同时类访问时，不一样，实例函数需要传入实例。

3、函数访问变量中，有很大不同。

```python
@classmethod
    def class_method(cls):
        print(cls.cls_variable)
        # print(cls.__instance_variable)   此处会报错，无法访问实例变量
```

在init函数定义的是实例变量，因为变量前缀添加了self。在类开始时定义的类变量，不需要添加前缀。

在变量访问中，发现类函数和静态函数是无法直接访问实例变量的，因为在后续调用中，不知道是那个实例的。但是实例函数是可以访问类变量的。

4、修改变量的范围

```python
new class variable
this is a class method
new class variable
instance variable
this is a instance method
new class variable
this is a static method
```

上图是function1中输出的结果。

```python
new class variable
this is a class method
new class variable
new instance varibale
this is a instance method
new class variable
this is a static method
new class variable
```

这是function2的结果，则class variable都变化了。

如果通过类方法修改变量，则所有实例中的类变量都会修改，这个类似静态变量了

如果通过实例修改变量，只是修改对应的实例变量。

**二、三者的选择原则**

1. 类方法和静态方法

```python
class Function(object):
    X = 1
    Y = 2

    @staticmethod
    def averag(*mixes):
        return sum(mixes) / len(mixes)

    @staticmethod
    def static_method():
        # 通过function调用，如果类名修改，此处需要修改不太方便
        return Function.averag(Function.X, Function.Y)

    @classmethod
    def class_method(cls):
        return cls.averag(cls.X, cls.Y)

class Subclass(Function):
    X =3
    Y = 5

    @staticmethod
    def averag(*mixes):
        return sum(mixes) / 3

func = Subclass()
print(func.static_method())
print(func.class_method())

# 结果
1.5
2.6666666666666665
```

**不同点**

1. 子类的实例继承了父类的static_method静态方法，调用该方法，还是调用的父类的方法和类属性。
2. 子类的实例继承了父类的class_method类方法，调用该方法，调用的是子类的方法和子类的类属性。
   - 这就是最大的不同，静态方法在类没有初始化，已经加载了，后续继承和她就没有关系了。
     同时静态方法关键明确指明了调用了Function的方法了，所以就无法修改了。这是2本质。



2. 类方法和实例方法

```python
class DateTest():
    def __init__(self,year,month,day):
        self.year = year
        self.day = day
        self.month = month

    def print_date(self):
        print("{}:{}:{}".format(self.year,self.month,self.day))

    @classmethod
    def get_date(cls,string_date):
        year,month,day = map(int,string_date.split('-'))
        return cls(year,month,day)

t = DateTest(2017,9,10)
r = DateTest.get_date("2017-12-09")
r.print_date()
t.print_date()
```




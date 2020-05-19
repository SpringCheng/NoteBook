**一、什么是unittest**

1. **TestCase：**
   - 测试环境的搭建（setup）
   - 执行测试代码（run）
   - 测试环境的还原（tearDown）
2. **Test suite**：多个测试结合在一起就是TestSuit，TestSuit可以嵌套TestSuit
3. **Testrunner**:用来执行测试用例
4. **TestLoader**：是用来加载TestCase到TestSuit中的，其中有几个loadTestsFrom__()方法，就是从各个地方寻找TestCase，来创建他们的实例，然后add到TestSuit中，再放回一个TestSuit实例。
5. **Test fixture：**对一个测试用例环境的搭建和销毁，是一个fixture通过覆盖TestCase()和tearDown()方法来实现。

**二、步骤**

1. 编写一个python类，继承unittest模块中的TesetCase类，这就是一个测试类
2. 在上面编写的测试类中定义测试方法（这个就是指测试用例），每个方法的方法名都要求以test打头，没有额外的参数。在该测试方法中调用被测试代码，校验测试结果，TestCase类中提供了很多标准的检验方法，如常见的assertEqual。
3. 执行unittest.main()，该函数会负责运行测试，它会实例化所有testCase的子类，并运行其中所有以test打头的方法。

**三、简单用法**

1. 用import unittest导入unittest模块
2. 定义一个继承自unittest.TestCase的测试用例，如class xxx（unittest.TestCae)
3. 定义setUp和tearDown，这两个方法与junit相同，即如果定义了则会在每个测试case执行setUP方法，执行完毕后执行tearDown方法。
4. 定义测试用例，名字以test开头，unittest会自动将test开头的方法放入测试用例集中。
5. 一个测试用例应该只测试一个方面，测试目的和测试内容应很明确，主要是调用assertEqual、assertRaises等断言方法判断程序执行结果和预期值是否相符
6. 调用unittest.main()启动测试
7. 如果测试通过，则会显示e，并给出具体的错误，如果测试测试失败则显示为f，如果测试通过为.,如有多个testcase，则结果依次显示

四、unittest.main()常用方法

```python
assertEqual(a,b)     a==b

assertNotEqual(a,b)  a!=b

assertTrue(x)       bool(x) is True

assertFalse(x)      bool(x) is False

assertIs(a,b)       a is b

assertIsNot(a,b)    a is not b

asertIsNone(x) 		x is None

assertIn(a,b)		a in b

assertNotIn(a,b)	a not in b

assertIsInstance(a,b)  isinstance(a,b)

assertNotIsInstance(a,b) not isinstance(a,b)
```

![img](https://upload-images.jianshu.io/upload_images/11349666-80bb857d40e76800.png)

主要用到的函数有：

failedinfo表示不成立打印信息failedinfo，为可选参数

self.fail([msg])会无条件的导致测试失败，不推荐使用。

self.assertEqual(value1, value2, failedinfo) # 断言value1 == value2

self.assertTrue(表达式, failedinfo) # 断言value为真

self.assertFalse(表达式, failedinfo) # 断言value为假




## 创建数组

一般情况下，科学数据都是海量的、层次关系复杂的，是由数据服务机构提供的，不是我们构造出来的。我们创建数组的目的，很多时候是用来做原型验证和算法验证的。NumPy 为创建数组提供了非常丰富的手段，可以无中生有，可以移花接木，还可以举一反三。配合数据类型设置、结构设置，就可以构造出我们想要的任何形式的数组了。

根据工作经验，我把创建数组的方法，分成了创建简单数组和构造复杂数组两大类。其实简单数组和复杂数组并没有严格的分界线，大致上，无中生有创造出来的数组称为简单数组，通过移花接木、举一反三创造出来的数组称为复杂数组。

### 创建简单数组

![在这里插入图片描述](../../AppData/Roaming/Typora/draftsRecover/image/20191217161343475.png)

#### 蛮力构造法

蛮力构造法就是你想要什么结构，就直接用 Python 数组或元组写出来。这个方法看起来简单，但很容易出错，不适合构造体量较大的数组。

```python
numpy.array(object, dtype=None, copy=True, order=None, subok=False, ndmin=0)

a = np.array([[1,2,3],[4,5,6]]) # 创建2行3列数组
```

#### 特殊数值法

特殊值一般指0，1，空值等。特殊数值法适合构造全0、全1、空数组，或者由0、1组成的类似单位矩阵（主对角线为1，其余为0）的数组。特殊数值法使用的4个函数原型如下：

```python 
numpy.zeros(shape, dtype=float, order=‘C’)
numpy.ones(shape, dtype=float, order=‘C’)
numpy.empty(shape, dtype=float, order=‘C’)
numpy.eye(N, M=None, k=0, dtype=float, order='C’)

```

这几个函数配合 shape 和 dtype 参数，可以很方便的构造一些简单数组：

```python 
>>> np.zeros(6)
array([0., 0., 0., 0., 0., 0.])
>>> np.zeros((2,3))
array([[0., 0., 0.],
       [0., 0., 0.]])
>>> np.ones((2,3),dtype=np.float)
array([[1., 1., 1.],
       [1., 1., 1.]])
>>> np.empty((2,3))
array([[1., 1., 1.],
       [1., 1., 1.]])
>>> np.eye(2)
array([[1., 0.],
       [0., 1.]])
```


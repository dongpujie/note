map(function, iterable, ...)、filter(function, iterable)、reduce(function, iterable[, initializer])		

map()对一（多）个可迭代对象中每个元素使用function，返回结果列表。多个可迭代对象时可对应位置进行操作。

filter()对可迭代对象中每个元素进行function判断，最后返回是True的列表。

reduce()对可迭代对象中每两个元素按顺序进行function操作，最后返回一个值。

## sorted()

### 语法

```python
sorted(iterable, cmp=None, key=None, reverse=False)
```

### 参数

- iterable -- 可迭代对象。
- cmp -- 比较的函数，这个具有两个参数，参数的值都是从可迭代对象中取出，此函数必须遵守的规则为，大于则返回1，小于则返回-1，等于则返回0。
- key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。可以为匿名函数。
- reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。

### 返回值

返回重新排序的列表。

## os.listdir()

### 语法

**listdir()**方法语法格式如下：

```python
os.listdir(path)
```

### 参数

- **path** -- 需要列出的目录路径

### 返回值

返回指定路径下的文件和文件夹列表。

## enumerate()

### 语法

以下是 enumerate() 方法的语法:

```
enumerate(sequence, [start=0])
```

### 参数

- sequence -- 一个序列、迭代器或其他支持迭代对象。
- start -- 下标起始位置。

### 返回值

返回 enumerate(枚举) 对象。

## getattr()

### 语法

getattr 语法：

```
getattr(object, name[, default])
```

### 参数

- object -- 对象。
- name -- 字符串，对象属性。
- default -- 默认返回值，如果不提供该参数，在没有对应属性时，将触发 AttributeError。

### 返回值

返回对象属性值。

## setarrt()

### 语法

setattr() 语法：

```
setattr(object, name, value)
```

### 参数

- object -- 对象。
- name -- 字符串，对象属性。
- value -- 属性值。

### 返回值

无。

## hasattr()

### 语法

hasattr 语法：

```
hasattr(object, name)
```

### 参数

- object -- 对象。
- name -- 字符串，属性名。

### 返回值

如果对象有该属性返回 True，否则返回 False。

## setdefault()

### 语法

setdefault() 方法语法：

```
dict.setdefault(key, default=None)
```

### 参数

- key -- 查找的键值。
- default -- 键不存在时，设置的默认键值。

### 返回值

如果字典中包含有给定键，则返回该键对应的值，否则返回为该键设置的值。

## zip()

### 语法

zip 语法：

```
zip([iterable, ...])
```

### 参数

- iterabl -- 一个或多个迭代器;

### 返回值

返回元组列表。

## reduce()

### 语法

reduce() 函数语法：

```python
reduce(function, iterable[, initializer])
```

### 参数

- function -- 函数，有两个参数
- iterable -- 可迭代对象
- initializer -- 可选，初始参数

### 返回值

返回函数计算结果。

## filter()

### 语法

以下是 filter() 方法的语法:

```
filter(function, iterable)
```

### 参数

- function -- 判断函数。
- iterable -- 可迭代对象。

### 返回值

返回列表。

## map()

### 语法

map() 函数语法：

```
map(function, iterable, ...)
```

### 参数

- function -- 函数
- iterable -- 一个或多个序列

### 返回值

Python 2.x 返回列表。

Python 3.x 返回迭代器。


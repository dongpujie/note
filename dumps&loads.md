json序列化的是字典类型的数据类型。

pickle序列化的是函数和类。





```python
import json


json.dumps(dict1, ensure_ascii=False)		# 将字典类型的数据序列化为str

json.dump(dict1, f)		# 将字典类型的数据序列化为str并写入到文件，f为文件对象
```

dump与dumps的区别就是dumps需要自己将序列化的字典write到文件中，而dump内部已经帮我们做了这一步



```python
import json


json.loads(str1)		# 将文本中读取的字符串反序列化为python中的字典数据类型

json.load(f)			# 将文件对象直接反序列化为python中的字典数据类型
```

load与loads的区别就是loads需要自己将文件中的信息反序列化到内存中再read，而load内部已经帮我们做了这一步





```python
import pickle 


def foo(): print('ok')
    
data = pickle.dumps(foo)	# 将函数或者类对象序列化为可写入文件的二进制
f = open('pickle_text', 'wb')
f.write(data)


def foo(): pass
f = open('PICKLE_text','rb')
data = f.read()
data = pickle.loads(data)	# 需要上面添加foo()函数才不会报错，因为反序列化之后，foo的引用地址会找不到foo函数，需要重新加载
```





shelve模块

shelve模块比pickle模块简单，只有一个open函数，返回类似于字典的对象，可读可写：key必须为字符串，而值可以是python所支持的数据类型

```python
import shelve
f = shelve.open(r'SHELVE_text.txt')
f['info'] = {'name':'ljj','age':18}
f['shopping'] = {'name':'手机','price':1000}
data = f.get('shopping')
print(data)
```



### demjson

demjson 可以解決不正常的json格式数据

```python
# -*- coding: utf-8 -*-
 
import demjson
 
js_json = "{x:1, y:2, z:3}"
 
py_json1 = "{'x':1, 'y':2, 'z':3}"
 
py_json2 = '{"x":1, "y":2, "z":3}'
 
data = demjson.decode(js_json)
print(data)
# {'y': 2, 'x': 1, 'z': 3}
 
data = demjson.decode(py_json1)
print(data)
# {'y': 2, 'x': 1, 'z': 3}
 
data = demjson.decode(py_json2)
print(data)
# {'y': 2, 'x': 1, 'z': 3}

```



#### demjson 的常用的两个方法， 一个是 encode， 一个是 decode

|        |                                          |
| ------ | ---------------------------------------- |
| encode | 将 Python 对象编码成 JSON 字符串         |
| decode | 将已编码的 JSON 字符串解码为 Python 对象 |


## 视图常用逻辑

### 返回JSON

在使用 Flask 写一个接口时候需要给客户端返回 JSON 数据，在 Flask 中可以直接使用 **jsonify** 生成一个 JSON 的响应

```python
# 返回JSON
@app.route('/demo4')
def demo4():
    json_dict = {
        "user_id": 10,
        "user_name": "laowang"
    }
    return jsonify(json_dict)
```

> 不推荐使用 json.dumps 转成 JSON 字符串直接返回，因为返回的数据要符合 HTTP 协议规范，如果是 JSON 需要指定 content-type:application/json

### 重定向

- 重定向到

```python
# 重定向
@app.route('/demo5')
def demo5():
    return redirect('http://www.itheima.com')
```

- 重定向到自己写的视图函数
  - 可以直接填写自己 url 路径
  - 也可以使用 url_for 生成指定视图函数所对应的 url

```python
@app.route('/demo1')
def demo1():
    return 'demo1'

# 重定向
@app.route('/demo5')
def demo5():
    return redirect(url_for('demo1'))
```

- 重定向到带有参数的视图函数
  - 在 url_for 函数中传入参数

```python
# 路由传递参数
@app.route('/user/<int:user_id>')
def user_info(user_id):
    return 'hello %d' % user_id

# 重定向
@app.route('/demo5')
def demo5():
    # 使用 url_for 生成指定视图函数所对应的 url
    return redirect(url_for('user_info', user_id=100))
```





# 正则匹配路由

在 web 开发中，可能会出现限制用户访问规则的场景，那么这个时候就需要用到正则匹配，根据自己的规则去限定请求参数再进行访问

具体实现步骤为：

- 导入转换器基类：在 Flask 中，所有的路由的匹配规则都是使用转换器对象进行记录
- 自定义转换器：自定义类继承于转换器基类
- 添加转换器到默认的转换器字典中
- 使用自定义转换器实现自定义匹配规则

代码实现

- 导入转换器基类

```python
from werkzeug.routing import BaseConverter
```

- 自定义转换器

```python
# 自定义正则转换器
class RegexConverter(BaseConverter):
    def __init__(self, url_map, *args):
        super(RegexConverter, self).__init__(url_map)
        # 将接受的第1个参数当作匹配规则进行保存
        self.regex = args[0]
```

- 添加转换器到默认的转换器字典中，并指定转换器使用时名字为: re

```python
app = Flask(__name__)

# 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: re
app.url_map.converters['re'] = RegexConverter
```

- 使用转换器去实现自定义匹配规则
  - 当前此处定义的规则是：3位数字

```python
@app.route('/user/<re("[0-9]{3}"):user_id>')
def user_info(user_id):
    return "user_id 为 %s" % user_id
```

> 运行测试：<http://127.0.0.1:5000/user/123> ，如果访问的url不符合规则，会提示找不到页面

### 自定义转换器其他两个函数实现

继承于自定义转换器之后，还可以实现 to_python 和 to_url 这两个函数去对匹配参数做进一步处理：

- to_python：
  - 该函数参数中的 value 值代表匹配到的值，可输出进行查看
  - 匹配完成之后，对匹配到的参数作最后一步处理再返回，比如：转成 int 类型的值再返回：

```python
class RegexConverter(BaseConverter):
    def __init__(self, url_map, *args):
        super(RegexConverter, self).__init__(url_map)
        # 将接受的第1个参数当作匹配规则进行保存
        self.regex = args[0]

    def to_python(self, value):
        return int(value)
```

> 运行测试，在视图函数中可以查看参数的类型，由之前默认的 str 已变成 int 类型的值

- to_url:
  - 在使用 url_for 去获取视图函数所对应的 url 的时候，会调用此方法对 url_for 后面传入的视图函数参数做进一步处理
  - 具体可参见 Flask 的 app.py 中写的示例代码：ListConverter

### 系统自带转换器

```python
DEFAULT_CONVERTERS = {
    'default':          UnicodeConverter,
    'string':           UnicodeConverter,
    'any':              AnyConverter,
    'path':             PathConverter,
    'int':              IntegerConverter,
    'float':            FloatConverter,
    'uuid':             UUIDConverter,
}
```

> 系统自带的转换器具体使用方式在每种转换器的注释代码中有写，请留意每种转换器初始化的参数。
## base.py 

### 基本类

三个基本类：ContextMixin、View、TemplateResponseMixin

#### ContextMixin

```python
class ContextMixin:
    """
    默认的上下文模板，传递从get_context_data()接收的关键字参数。
    - 将view与其对应的self放入kwargs中。
    """
    extra_context = None

    def get_context_data(self, **kwargs):
        kwargs.setdefault('view', self)
        if self.extra_context is not None:
            kwargs.update(self.extra_context)
        return kwargs

```

#### View

```python
class View:
    """
    有意为所有视图简化父类，只实现方法分派和简单的完整性检查。
    """

    http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']

    def __init__(self, **kwargs):
        """
        构造函数，在URLconf中被调用，可以包含有用的额外关键字参数和其他东西。
        """
        # 遍历关键字参数，并将它们的值保存到我们的实例中，或者引发一个错误。
        # -将传入的关键字参数赋值给实例对象，则该实例对象可以获取到这些值。
        for key, value in kwargs.items():
            setattr(self, key, value)

    @classonlymethod
    def as_view(cls, **initkwargs):
        """请求-响应流程的主入口点。"""
        for key in initkwargs:
            # -避免将http_method_names中的方法名作为关键字传递给该对象
            if key in cls.http_method_names:
                # -您尝试将%s(如get)方法名作为关键字参数传递给%s()。不要这样做。
                raise TypeError("You tried to pass in the %s method name as a "
                                "keyword argument to %s(). Don't do that."
                                % (key, cls.__name__))
            # -确保as_view接收的关键字参数都是类属性的参数。
            if not hasattr(cls, key):
                # -%s()接收到无效关键字%r。as_view只接受已经是类属性的参数。
                raise TypeError("%s() received an invalid keyword %r. as_view "
                                "only accepts arguments that are already "
                                "attributes of the class." % (cls.__name__, key))

        def view(request, *args, **kwargs):
            self = cls(**initkwargs)
            if hasattr(self, 'get') and not hasattr(self, 'head'):
                self.head = self.get
            self.setup(request, *args, **kwargs)
            if not hasattr(self, 'request'):
                # -实例没有'request'属性。你是否重写了setup()而忘记调用super()了?
                raise AttributeError(
                    "%s instance has no 'request' attribute. Did you override "
                    "setup() and forget to call super()?" % cls.__name__
                )
            return self.dispatch(request, *args, **kwargs)
        view.view_class = cls
        view.view_initkwargs = initkwargs

        # 从类中获取名称和文档字符串
        update_wrapper(view, cls, updated=())

        # 以及由诸如csrf_exempt之类的装饰器设置的可能属性，以进行分派
        update_wrapper(view, cls.dispatch, assigned=())
        return view

    def setup(self, request, *args, **kwargs):
        """初始化所有视图方法共享的属性。"""
        self.request = request
        self.args = args
        self.kwargs = kwargs

    def dispatch(self, request, *args, **kwargs):
        # 尝试去调度正确的方法，如果方法不存在，遵循错误处理。如果请求方法不在已批准列表里，也遵循错误处理
        if request.method.lower() in self.http_method_names:
            # -getattr(object, name[, default])
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)

    def http_method_not_allowed(self, request, *args, **kwargs):
        logger.warning(
            'Method Not Allowed (%s): %s', request.method, request.path,
            extra={'status_code': 405, 'request': request}
        )
        return HttpResponseNotAllowed(self._allowed_methods())

    def options(self, request, *args, **kwargs):
        """处理OPTIONS HTTP的请求响应。"""
        response = HttpResponse()
        response['Allow'] = ', '.join(self._allowed_methods())
        response['Content-Length'] = '0'
        return response

    def _allowed_methods(self):
        return [m.upper() for m in self.http_method_names if hasattr(self, m)]
```

#### TemplateResponseMixin

```python
class TemplateResponseMixin:
    """可用于渲染模板的mixin。"""
    template_name = None
    template_engine = None
    response_class = TemplateResponse
    content_type = None

    def render_to_response(self, context, **response_kwargs):
        """
        使用此视图的`response_class`返回响应，并使用给定的上下文呈现模板。

        将response_kwargs传递给响应类的构造函数。
        """
        response_kwargs.setdefault('content_type', self.content_type)
        return self.response_class(
            request=self.request,
            template=self.get_template_names(),
            context=context,
            using=self.template_engine,
            **response_kwargs
        )

    def get_template_names(self):
        """
        返回用于请求的模板名称列表。必须返回一个列表。如果render_to_response()被覆盖，则可能不会被调用。
        """
        if self.template_name is None:
            # TemplateResponseMixin需要定义template_name或实现get_template_names()
            raise ImproperlyConfigured(
                "TemplateResponseMixin requires either a definition of "
                "'template_name' or an implementation of 'get_template_names()'")
        else:
            return [self.template_name]
```

### 继承自基本类

#### TemplateView

继承三个基本类

```python
class TemplateView(TemplateResponseMixin, ContextMixin, View):
    """
    渲染模板。将关键字参数从URLconf传递到上下文。
    """
    def get(self, request, *args, **kwargs):
        context = self.get_context_data(**kwargs)
        return self.render_to_response(context)

```

#### RedirectView

```python 
class RedirectView(View):
    """提供任何GET请求的重定向。"""
    permanent = False
    url = None
    pattern_name = None
    query_string = False

    def get_redirect_url(self, *args, **kwargs):
        """
        返回URL重定向。URL pattern 中生成重定向请求的关键字参数作为参数提供给此方法。
        """
        if self.url:
            url = self.url % kwargs
        elif self.pattern_name:
            url = reverse(self.pattern_name, args=args, kwargs=kwargs)
        else:
            return None

        args = self.request.META.get('QUERY_STRING', '')
        if args and self.query_string:
            url = "%s?%s" % (url, args)
        return url

    def get(self, request, *args, **kwargs):
        url = self.get_redirect_url(*args, **kwargs)
        if url:
            if self.permanent:
                return HttpResponsePermanentRedirect(url)
            else:
                return HttpResponseRedirect(url)
        else:
            logger.warning(
                'Gone: %s', request.path,
                extra={'status_code': 410, 'request': request}
            )
            return HttpResponseGone()

    def head(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def options(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)
```


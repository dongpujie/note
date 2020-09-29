## setting

新创建的应用要在setting.INSTALLED_APPS中注册，不然无法生成迁移文件，无法访问。

setting.DATABASES数据库配置。

## 应用

### rbac权限组件

#### model中

models有CharField、IntegerField、DateTimeField、EmailField、ForeignKey、ManyToManyField、OneToOneField等等几种类型。

字段中verbose_name是字段显示的别名。

class Meta:	verbose_name_plural 是类显示的别名。

#### admin中

admin是后台管理系统。

admin.site.register(模型)来配置管理系统能修改的类，即数据库。





























客户  name、age、email、company

付费记录  customer、money、create_time


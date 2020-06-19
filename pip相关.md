文章目录
1. 前言
2. pip 背后的孙悟空和六耳猕猴
2.1 六耳猕猴（pip.exe）的栖息地及召唤术
2.2 孙悟空（pip模块）的栖息地及召唤术
3 pip 子命令与通用选项
3.1 pip 子命令速查表
3.2 pip 通用选项速查表
4. pip 使用详解
4.1 install 子命令
4.1.1 install 子命令选项
4.1.2 包索引选项
4.1.3 使用样例
4.2 uninstall 子命令
4.2.1 uninstall 子命令选项
4.2.2 使用样例
4.3 list 子命令
4.3.1 list 子命令选项
4.3.2 使用样例
4.4 freeze 子命令
4.4.1 freeze 子命令选项
4.4.2 使用样例
4.5 show 子命令
4.5.1 show 子命令选项
4.5.2 使用样例
4.6 check 子命令
4.6.1 使用样例
4.7 config 子命令
4.7.1 config 子命令选项
4.8 search 子命令
4.8.1 search 子命令选项
4.8.2 使用样例
4.9 wheel 子命令
4.9.1 wheel 子命令选项
4.9.1 包索引选项
4.9.3 使用样例
4.10 hash 子命令
4.10.1 hash 子命令选项
4.10.2 使用样例
4.11 completion 子命令
4.11.1 completion 子命令选项
1. 前言
pip 是 Python 目前最流行、最方便的包管理工具。早期的 pythoneer 使用 setup.py 安装模块，用起来比较麻烦，所以很多程序员都曾经用过把模块文件直接扔进 python 安装路径下的 Lib\site-packages 文件夹的“暴力”安装法。后来稍微进化了一点，使用 easy_install工具，对应的是.egg模块文件。再后来，终于进化到了 pip 时代，对应的是模块文件也变成.whl文件。

正因为 Python 的包管理工具在很长一段时间一直处于探索、成长期，模块下载和安装都非常不便，使得很多商业公司以提供便利的模块安装服务为卖点，推出了自己的发行版，比如 AnacondaPython 和 ActivePython。随着 pip 的横空出世，Python 包管理的暗黑时代终于结束了，但是，黎明并没有如期而至。由于Python的版本迭代不能兼容，导致开发者有时候需要同时保留多个版本，比如 py2 和 py3 并存，py3.3 和 py3.7 并存等，开发者在使用pip安装模块的时候，仍然会遇到各种各样的问题。

本文并不打算总结所有的应用问题，再给出解决方案，而是分析产生问题的根源，从而帮助初学者自已找到解决方案。本文最后附上 pip 命令的参数速查表，以便初学者查阅。

2. pip 背后的孙悟空和六耳猕猴
大多数人只知道 pip 是一个工具，却不知这里面隐藏着一个孙行者和六耳猕猴的故事。对面分明是六耳猕猴，很多人却以为是自己遇到了孙悟空。原来，使用 pip 工具安装模块有两种方式，一种是运行 pip.exe 可执行程序，一种是调用 python 解释器把 pip 模块当成脚本来运行。我们姑且把 pip.exe 称做六耳猕猴，把pip 模块叫做孙悟空，以py3.7环境下安装 numpy 模块为例，讲述一下这两个捣蛋鬼的故事。

2.1 六耳猕猴（pip.exe）的栖息地及召唤术
一般使用 pip 安装 numpy 模块是这样的：

pip install numpy

在这里，安装任务交给了谁呢？孙悟空还是六耳猕猴？答案是六耳猕猴，也就是 pip.exe。如何验证？打开C:\Users\xufiv\AppData\Local\Programs\Python\Python37\Scripts文件夹（默认安装路径下），会看到下图红框标注的 pip.exe 以及 pip3.exe 和 pip3.7.exe 三个文件。把 pip.exe 改名为 xpip.exe，就会发现，上面的安装命令无法执行了。这说明，这个安装命令中的 pip，就是Scripts文件夹中的 pip.exe。如果我们运行

pip3.exe install numpy

或者

pip3.7.exe install numpy

同样可以成功安装numpy模块。

可是，有些同学会说，我也是使用默认设置安装的python，使用 pip 命令安装模块时，却提示说“无法将“pip”项识别为 cmdlet、函数、脚本文件或可运行程序的名称”。六耳猕猴不听话，这究竟是咋回事呢？切莫慌张，只需以计而行，定能让六耳猕猴乖乖地干活。

先尝试运行下面的命令：

C:\Users\xufiv\AppData\Local\Programs\Python\Python37\Scripts\pip.exe numpy

如果还是无法识别该命令，请检查你的python安装路径下Scripts文件夹中是否存在 pip.exe 文件，或者是否已经被（其他工具）修改为别的什么名字了。如果该命令能够被识别，说明的系统环境变量没有设置好。请右键单击“我的电脑”，点击“属性”菜单，在弹出窗口中选择“高级系统设置”，在“高级”标签页点击“环境变量”按钮，在弹出的窗口中选中用户变量（仅当前用户使用python）或者系统变量（所有用户均可使用python）的中path变量，点“编辑”按钮，增加 C:\Users\xufiv\AppData\Local\Programs\Python\Python37\Scripts\ 项。之后，无论在哪个路径之下，都可以使用 pip 安装模块了。

实际上，每一个版本的 python 都会配备一个六耳猕猴，每个六耳猕猴都有自己的栖息地，同时也有可能将自己的栖息地写入环境变量中。此时，为避免冲突，最好的办法是，给每个六耳猕猴取一个携带版本信息的名字，比如，pip2.exe、pip3.exe，这样我们就可以使用不同的名字召唤想要驱使的六耳猕猴了。

2.2 孙悟空（pip模块）的栖息地及召唤术
学会了召唤六耳猕猴，你一定非常期待召唤孙悟空了。命令很简单：

python -m pip install numpy

和六耳猕猴的召唤术相比，命令前面多了 python -m两部分。这里的 python 是指 python 解释器，-m 是 python 解释器的参数，意思是解释器运行的不是脚本文件，而是把 pip 模块——也就是孙悟空当作脚本运行。通常，孙悟空藏身在 C:\Users\xufiv\AppData\Local\Programs\Python\Python37\Lib\site-packages\ 路径下， python 解释器自会找到它，我们无需费心。但是，能否找到 python 解释器，却是我们需要考虑的问题。安装 python 时，如果勾选了下图所示的复选框，我们就一定能轻松找到 python 解释器，否则就需要手工添加 C:\Users\xufiv\AppData\Local\Programs\Python\Python37\ 路径到环境变量中。


在系统复杂、安装混乱的情况下，相比较而言，召唤孙悟空要比召唤六耳猕猴的成功概率要高。这是因为，一来孙悟空的栖息地要比六耳猕猴安全，二来 python 解释器应用频繁，其路径早已被设置为环境变量了。

3 pip 子命令与通用选项
3.1 pip 子命令速查表
子命令	说明
install	安装软件
uninstall	卸载软件
list	列出已安装软件
freeze	按着一定格式输出已安装软件列表
show	显示指定模块的详细信息
check	检查包的依赖关系是否完整
config	管理配置
search	搜索包(搜索关键字)
wheel	打包
hash	计算包的hash值
completion	命令补全
help	查看命令的帮助信息
3.2 pip 通用选项速查表
通用选项	说明
-------------------------------------------	-----------------------------
-h, --help	显示帮助.
-v, --verbose	更多的输出，最多可以使用3次
-V, --version	现实版本信息然后退出.
-q, --quiet	最少的输出.
–log-file < path >	覆盖的方式记录verbose错误日志，默认文件：/root/.pip/pip.log
–log < path >	不覆盖记录verbose输出的日志.
–proxy < proxy >	设置代理 [user:passwd@]proxy.server:port
–retries < retries>	每个连接尝试的重试次数(默认为5次)
–timeout < sec >	连接超时时间 (默认15秒).
–exists-action < action >	路径已经存在时，此选项指定默认行为: (s)witch, (i)gnore, (w)ipe, (b)ackup.
–trusted-host < hostname>	标记可信任的网站
–cert < path >	证书.
–client-cert	SSL客户端证书的路径，一个包含该私钥的文件和PEM中的证书
–cache-dir < dir>	在< dir >中存储缓存数据
–no-cache-dir	禁用缓存
–disable-pip-version-check	不定期检查PyPI以确定是否一个最新的版本
–no-color	禁止彩色输出
4. pip 使用详解
4.1 install 子命令
4.1.1 install 子命令选项
选项	说明
-r, --requirement < file >	从给定文件要求安装。 此选项可以多次使用。
-c, --constraint < file>	使用给定的约束文件版本。此选项可用于多个
–no-deps	不安装软件包的相关性。
–pre	包括预发行版和开发版。默认情况下，pip只找到了下载稳定版.
-e, --editable < path/url>	在编辑模式下安装项目(即。 从本地后边添加或VC setuptools"发展模式")
-t, --target < dir>	将软件包安装到dir目录。默认情况下，这不会替换dir中的现有文件/文件夹。使用 --upgrade将dir中的现有包替换为新版本
–platform < platform>	仅使用与指定系统兼容的whl文件。默认为正在运行的系统平台
–python-version < python_version>	指定python版本号，使用最多三个点分隔的整数（3.7.0）
–implementation < implementation>	仅使用与Python解释器兼容的版本
–abi < abi>	仅使用与 Python abi 兼容的whl文件
–user	安装到电脑上Python用户安装目录
–root < dir>	安装与此备用根目录相关的所有内容。
–prefix < dir>	放置 lib、bin 和其他顶级文件夹的安装前缀
-b, --build < dir>	目录，用于将包解压缩指定路径。请注意，初始生成仍位于临时目录中。通过适当设置 TMPDIR 环境变量 （Windows 上的 TEMP），可以控制临时目录的位置。传递时，在发生故障时不会清除生成目录。
–src
要签出可编辑项目的目录
-U, --upgrade	将所有指定的包升级到最新的可用版本
–upgrade-strategy <upgrade_strategy>	确定应如何处理依赖项升级 [默认：仅在需要时]
–force-reinstall	强制重新安装
-I, --ignore-installed	忽略已安装的包（改为重新安装）
–ignore-requires-python	忽略需求 Python 信息
–no-build-isolation	构建现代源分布时禁用隔离。生成依赖项如果使用此选项，则必须安装 PEP 518 指定的方案。
–use-pep517	使用 PEP 517 构建源分布
–install-option	要提供给setup.py安装命令的额外参数
–global-option	在安装命令之前，要提供给setup.py调用的额外全局选项
–compile	将 Python 源文件编译为字节码
–no-compile	不要将 Python 源文件编译为字节码
–no-warn-script-location	在 PATH 外部安装脚本时，不要发出警告
–no-warn-conflicts	不要警告有关已中断的依赖项
–no-binary <format_control>	不要使用二进制包。可以多次提供，并且每次都会添加到现有值。接受 ：all：禁用所有二进制包，：none：清空集，或一个或多个包名称之间使用逗号。请注意，某些包在编译时很棘手，在它们上使用此选项时可能无法安装。
–only-binary <format_control>	不要使用源包。可以多次提供，并且每次都会添加到现有值。接受 ：all：禁用所有源包，但：清空集，或一个或多个包名称，其中带有逗号。当在二进制分发的软件包上使用此选项时，将无法安装这些包。
–prefer-binary	首选较旧的二进制包而不是较新的源包。
–no-clean	不要清理生成目录
–require-hashes	对于可重复的安装，需要哈希以检查每个要求。当需求文件中的任何包具有 --哈希选项时，将隐含此选项
–progress-bar <progress_bar>	指定要显示的进度类型[off
4.1.2 包索引选项
选项	说明
-i, --index-url	Python 包索引的基本 URL（默认https://pypi.org/simple）
–extra-index-url	除了–index-url 之外，要使用的包索引的额外 URL。应遵循与 --index-url 相同的规则
–no-index	忽略包索引（只查看 --find-links链接 URL）
-f, --find-links	如果URL 或路径指向的是html，则分析指向存档的链接。如果本地路径或file://目录的 url，然后在目录列表中查找存档。
4.1.3 使用样例
PS C:\Users\xufiv> pip install numpy # 默认从https://pypi.org/simple 下载
PS C:\Users\xufiv> pip install numpy https://pypi.tuna.tsinghua.edu.cn/simple/ # 从清华大学资源站点下载
PS C:\Users\xufiv> pip install numpy https://mirrors.aliyun.com/pypi/simple/ # 从阿里云资源站点下载
PS C:\Users\xufiv> pip install numpy https://pypi.mirrors.ustc.edu.cn/simple/ # 从中科大资源站点下载

4.2 uninstall 子命令
4.2.1 uninstall 子命令选项
选项	说明
-r, --requirement	卸载给定要求文件中列出的所有包。 此选项可以多次使用。
-y, --yes	确认卸载删除
4.2.2 使用样例
PS C:\Users\xufiv> pip uninstall numpy

4.3 list 子命令
4.3.1 list 子命令选项
选项	说明
-o, --outdated	列出过时的软件包
-u, --uptodate	列出最新软件包
-e, --editable	列出可编辑的项目
-l, --local	如果在具有全局访问权限的 virtualenv 中，请不要列出全局安装的包。
–user	仅输出在用户站点中的安装包.
–path	路径，罗列该路径下的安装包（可多次使用）
–pre	包括预发行版本和开发版本。默认情况下，pip 只找到稳定版本
–format <list_format>	输出格式：columns (default), freeze, or json
–not-required	列出不是已安装包依赖项的包
–exclude-editable	输出中排除可编辑的包
–include-editable	输出中包括可编辑的包
4.3.2 使用样例
PS C:\Users\xufiv> pip list -u
Package Version

altgraph 0.16.1
Pypubsub 4.0.3
pywin32-ctypes 0.2.0
tornado 6.0.3
torndb 0.3
tzlocal 2.0.0
wheel 0.33.6
xlrd 1.2.0

4.4 freeze 子命令
4.4.1 freeze 子命令选项
选项	说明
-r, --requirement	生成输出时，请使用给定需求文件中的顺序及其注释。此选项可多次使用
-f, --find-links	用于查找包的 URL，该 URL 将添加到输出中。
-l, --local	如果在具有全局访问权限的虚拟中，请不要输出全局安装的包.
–user	仅输出在用户站点中的安装包.
–path	路径，罗列该路径下的安装包（可多次使用）
–all	输出全部安装包，不要跳过这些包：setuptools, distribute, wheel, pip
–exclude-editable	输出中排除可编辑的包
4.4.2 使用样例
PS C:\Users\xufiv> pip freeze > requirements.txt # 输出到文件
PS C:\Users\xufiv> pip freeze --all # 列出所有的安装包
altgraph == 0.16.1
APScheduler == 3.6.1
basemap == 1.2.0
cycler == 0.10.0
freetype-py == 2.0.0.post6
ftp-client == 1.0.1

4.5 show 子命令
4.5.1 show 子命令选项
选项	说明
-f, --files	显示每个包的已安装文件的完整列表
4.5.2 使用样例
PS C:\Users\xufiv> pip show xlrd -f
Name: xlrd
Version: 1.2.0
Summary: Library for developers to extract data from Microsoft Excel ™ spreadsheet files
Home-page: http://www.python-excel.org/
Author: John Machin
Author-email: sjmachin@lexicon.net
License: BSD
Location: c:\users\12721\appdata\local\programs\python\python37\lib\site-packages
Requires:
Required-by:
Files:
…\Scripts_pycache_\runxlrd.cpython-37.pyc
…\Scripts\runxlrd.py
xlrd-1.2.0.dist-info\INSTALLER
xlrd-1.2.0.dist-info\LICENSE
xlrd-1.2.0.dist-info\METADATA
xlrd-1.2.0.dist-info\RECORD
xlrd-1.2.0.dist-info\WHEEL
xlrd-1.2.0.dist-info\top_level.txt
xlrd_init_.py
xlrd_pycache__init_.cpython-37.pyc
xlrd_pycache_\biffh.cpython-37.pyc
xlrd_pycache_\book.cpython-37.pyc
xlrd_pycache_\compdoc.cpython-37.pyc
xlrd_pycache_\formatting.cpython-37.pyc
xlrd_pycache_\formula.cpython-37.pyc
xlrd_pycache_\info.cpython-37.pyc
xlrd_pycache_\sheet.cpython-37.pyc
xlrd_pycache_\timemachine.cpython-37.pyc
xlrd_pycache_\xldate.cpython-37.pyc
xlrd_pycache_\xlsx.cpython-37.pyc
xlrd\biffh.py
xlrd\book.py
xlrd\compdoc.py
xlrd\formatting.py
xlrd\formula.py
xlrd\info.py
xlrd\sheet.py
xlrd\timemachine.py
xlrd\xldate.py
xlrd\xlsx.py

4.6 check 子命令
检查包的依赖关系是否完整

4.6.1 使用样例
PS C:\Users\xufiv> pip check xlrd
No broken requirements found.

4.7 config 子命令
4.7.1 config 子命令选项
选项	说明
–editor	用于编辑文件的编辑器
–global	仅使用系统范围的配置文件
–user	仅使用用户配置文件
–site	仅使用当前环境配置文件
–venv	[已弃用]仅在虚拟环境中使用当前环境配置文件
4.8 search 子命令
4.8.1 search 子命令选项
选项	说明
-i, --index	指定搜索地址URL（默认https://pypi.org/pypi）
4.8.2 使用样例
PS C:\Users\xufiv> pip search xlrd -i https://pypi.org/pypi
xlrd (1.2.0) - Library for developers to extract data from Microsoft Excel ™
spreadsheet files
INSTALLED: 1.2.0 (latest)
pythonetl-xlrd (0.9.3dev) - Library for developers to extract data from Microsoft Excel ™
spreadsheet files
xlrd-with-formulas (1.0.0) - Library for developers to extract data from Microsoft Excel ™
spreadsheet files
xlrd-ignore-writeaccess-corruption (1.2.0.1) - Library for developers to extract data from Microsoft Excel ™
spreadsheet files
xlrd-no-sector-corruption-check (1.2.0.post1) - Library for developers to extract data from Microsoft Excel ™
spreadsheet files
xlutils (2.0.0) - Utilities for working with Excel files that require both xlrd and
xlwt
xleditor (1.0.2) - Easy to open xls/xlsx file and edit it (merge xlrd and xlutils)
xlsimport (0.1) - Builton top of xlrd, package provides set of cell parsers that make
your import easier.

4.9 wheel 子命令
4.9.1 wheel 子命令选项
选项	说明
-w, --wheel-dir
指定whl文件存放路径，默认当前路径
–no-binary <format_control>	不要使用二进制包。可以多次提供，并且每次都会添加到现有值。接受：all：禁用所有二进制包，：none：清空集，或一个或多个包名称之间使用逗号
–only-binary <format_control>	不要使用源包。可以多次提供，并且每次都会添加到现有值。接受 ：all：禁用所有源包，但：清空集，或一个或多个包名称，其中带有逗号。当对它们使用此选项时，没有二进制分布的包将无法安装。
–prefer-binary	首选较旧的二进制包而不是较新的源包
–build-option	要提供给"setup.py bdist_wheel"的额外参数。
–no-build-isolation	构建现代源分布时禁用隔离。如果使用此选项，则必须已安装 PEP 518 指定的生成依赖项。
–use-pep517	使用 PEP 517 构建源分布
-c, --constraint < file>	使用给定的约束文件版本。此选项可用于多个
-e, --editable <path/url>	从本地项目路径或 VCS URL 以可编辑模式（即设置工具[开发模式]）安装项目
-r, --requirement	从给定的要求文件安装。此选项可多次使用.
–src
要签出可编辑项目的目录
–ignore-requires-python	忽略需求 Python 信息
–no-deps	不安装软件包的相关性。
-b, --build < dir>	目录，用于将包解压缩指定路径。请注意，初始生成仍位于临时目录中。通过适当设置 TMPDIR 环境变量 （Windows 上的 TEMP），可以控制临时目录的位置。传递时，在发生故障时不会清除生成目录。
–progress-bar <progress_bar>	指定要显示的进度类型 [off
–global-option	在"bdist_wheel"命令之前，要向setup.py调用提供额外的全局选项
–pre	包括预发行版本和开发版本。默认情况下，pip 只查找稳定版本
–no-clean	不要清理生成目录
–require-hashes	对于可重复的安装，需要哈希以检查每个要求。当需求文件中的任何包具有 --hash时，将隐含此选项
4.9.1 包索引选项
选项	说明
-i, --index-url	Python 包索引的基本 URL（默认https://pypi.org/simple）
–extra-index-url	除了–index-url 之外，要使用的包索引的额外 URL。应遵循与 --index-url 相同的规则
–no-index	忽略包索引（只查看 --find-links链接 URL）
-f, --find-links	如果URL 或路径指向的是html，则分析指向存档的链接。如果本地路径或file://目录的 url，然后在目录列表中查找存档。
4.9.3 使用样例
PS C:\Users\xufiv> pip wheel xlrd
Collecting xlrd
Using cached https://files.pythonhosted.org/packages/b0/16/63576a1a001752e34bf8ea62e367997530dc553b689356b9879339cf45a4/xlrd-1.2.0-py2.py3-none-any.whl
Saved d:\youthgit\youth_packages\ftp_client\xlrd-1.2.0-py2.py3-none-any.whl
Skipping xlrd, due to already being wheel.

4.10 hash 子命令
4.10.1 hash 子命令选项
选项	说明
-a, --algorithm	要使用的哈希算法：sha256或sha384或sha512
4.10.2 使用样例
PS C:\Users\xufiv> python -m pip hash d:/linhl/t.txt
d:/linhl/t.txt:
–hash=sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855

4.11 completion 子命令
4.11.1 completion 子命令选项
选项	说明
-b, --bash	为 bash 发出完成代码
-z, --zsh	为 zsh 发出完成代码
-f, --fish	为 fish 发出完成代码


版权声明：本文为CSDN博主「天元浪子」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xufive/article/details/102993579
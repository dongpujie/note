### vim安装

先执行以下命令来查询 CentOS 上的 Vim 是什么版本：

```bash
rpm -qa|grep vim
```

如果只有一个 vim-minimal，那就需要我们装功能更完善的 Vim了。执行以下命令即可：

```bash
yum -y install vim*
```

#### vi指向

```shell
# 查询 vi 和 vim 命令指向的程序
which vi vim
# 编辑 bashrc 配置文件
cd ~
vim .bashrc
# 在 .bashrc 文件里增加一条命令指向后保存退出
alias vi='vim'
# 重启 bash，让刚才的配置生效
source .bashrc
# 再次查询 vi 和 vim 的指向确认
which vi vim
```

#### 编辑vim配置

配置文件：`cd .vimrc` 

```
syntax on
autocmd InsertLeave * se nocul
set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
set termencoding=utf-8
set encoding=utf-8
set confirm
set wrap
set nu
set autoindent
set expandtab
set tabstop=4
set shiftwidth=4
set laststatus=2
set cmdheight=1
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%y年%m月%d日\ %H:%M\")}
set showmatch
set matchtime=1
set showcmd
set nowrapscan
set ignorecase
set incsearch
set hlsearch

```

#### 说明

```
" 开启原生代码高亮
syntax on

" 输入时的当前行状态显示
autocmd InsertLeave * se nocul
autocmd InsertEnter * se cul " 输入时当前行高亮

" 解决中文乱码问题，打开文档时逐一选择测试最终编码
set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
set termencoding=utf-8 " 其它编码工具打开也不会丢失乱码
set encoding=utf-8 " 编码格式

set confirm " 在处理未保存或只读文件的时候，弹出确认选择
set wrap " 自动换行
set nu " 显示行号
set autoindent " 自动对齐
set expandtab " 输入Tab自动转换称空格
set tabstop=4 " 设定tab长度为4
set shiftwidth=4 " 设定缩进的宽度为4

" 总是显示状态行
set laststatus=2
" 命令行（在状态行下）的高度，默认为1，这里是2
set cmdheight=1
" 我的状态行显示的内容（包括文件类型和解码）
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%y年%m月%d日\ %H:%M\")}

set showmatch " 高亮显示匹配的括号
set matchtime=1 " 匹配括号高亮的时间（单位是十分之一秒）
set nowrapscan " 禁止搜索到文件两端时重新搜索
set showcmd " 状态栏显示目前所执行的指令

" 查找时忽略大小写
set ignorecase
set incsearch
set hlsearch
```


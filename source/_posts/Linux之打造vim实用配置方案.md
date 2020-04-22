---
bashtitle: Linux之像IDE一样使用vim（C++版本/Python版本）
toc: true
recommend: 1
date: 2020-03-30 14:30:43
thumbnail: https://pic.downk.cc/item/5e887c1d504f4bcb04ee9d03.png
tags: Vim
categories: Linux
keywords: Vim
---

打造vim实用配置方案

 <!-- more --> 

这篇文章的目的，是打造让自己感觉舒服好用的[Vim](https://github.com/vim/vim)/[NeoVim](https://github.com/neovim/neovim)环境以便获得更好的编程体验，并将我所遇到的一些问题列出解决方案，不足之处还请见谅，欢迎与各位进行交流，邮箱是<kangzhiheng@live.cn>。

本文所有命令，在**CentOS7.6** 上全部测试通过，Mac系统相对来说配置比较容易，在此不做特别说明，每一步都有详细的步骤，敬请参考。

### 什么是Vim？

**[Vim](https://www.vim.org/)- the ubiquitous text editor**，一种无处不在的文本编辑器，维基百科是这么介绍**vim**的：

> **Vim**是从[vi](https://zh.wikipedia.org/wiki/Vi)发展出来的一个[文本编辑器](https://zh.wikipedia.org/wiki/文本编辑器)。其代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。和[Emacs](https://zh.wikipedia.org/wiki/Emacs)并列成为[类Unix系统](https://zh.wikipedia.org/wiki/类Unix系统)用户最喜欢的编辑器。
>
> Vim的第一个版本由[布莱姆·米勒](https://zh.wikipedia.org/wiki/布萊姆·米勒)在1991年发布。最初的简称是**V**i **IM**itation，随着功能的不断增加，正式名称改成了**V**i **IM**proved。现在是在[开放源代码](https://zh.wikipedia.org/wiki/开放源代码)方式下发行的[自由软件](https://zh.wikipedia.org/wiki/自由软件)。

**文件的增删查改**——熟悉Windows的朋友可能对文件增删查改习以为常，在此不做讨论。对于一名程序工作者，可能经常在Linux下进行程序的增删查改，很多人可能有这么一套方法：我在Windows环境下编写好了程序，然后通过Xftp、FileZilla等类似的工具将其丢到服务器上，一顿操作下来，发现程序报错了，怎么办？然后在Windows下修改，再丢到服务器上双十合十祈求老天保佑……当然像PyCharm这种强大的IDE，会提供远程接入服务器进行同步文件及程序运行等功能，这种也挺好，就是可能需要鼠标点来点去，也有一定的学习成本。现在有这么一个问题，PyCharm明天到期了……社区版？不好意思所有社区版鄙人都不会去尝试，别问为啥，问就是不喜欢。

**配置文件**——配置文件和工作效率有着紧密的联系。对于算法工程师或者后端工程师，在Github逛gai的时候终于发现了一个好用的模型，一顿`git clone repoyoulike`，然后按照配置文档进行修改，什么**tf**要升级啦，**gcc/g++**要降级啦，这个文档要修改路径，那个文档要改参数，都离不开Vim，如果是带桌面的Linux系统，可能会使用`gedit`来打开文件进行编辑，但是这样效率很低，尤其是大型程序修改起来特别费劲。对于大多数情况，可能用**ssh**连接远程服务器，这个时候，怎么也绕不开Vim，尤其是那些只能在服务器上运行的程序。

**知其然知其所以然**——使用Vim来处理程序方面的设定问题，不但可以让自己比较熟悉Linux的运行状况，也更加能保证你的修改可以在Linux下正确运行。

**Vim**适用于各种操作系统上，比如类Unix（种Linux发行版本、Mac 用户）以及Windwos系统。Unix可以使用包管理器安装 Vim；对于 Windows 用户，可以从[官网](https://www.vim.org/download.php)下载。 

### Vim哲学

简单概括：摆脱鼠标，一键到达。

即为全命令操作，能用键盘做到的事情，绝不用鼠标，在Vim下，键盘可以满足你的一切需求，效率是第一生产力。

### 升级支持python3的Vim8

在众多Linux发行版中，比如CentOS或者Ubuntu等，默认安装的是**vi**编辑器，Vim兼容vi。Vim已经存在20年了，时代在进步，[Vim8](https://github.com/vim/vim)在[NeoVim](https://github.com/neovim/neovim)的步步紧逼下，扩展了一些新的特征，比如支持时钟、异步操作、支持终端窗口**terminal**，尤其是支持**terminal**非常关键。本文最开始在Vim7下进行操作的，但是有些插件需要Vim8版本，尤其是[代码补全插件YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)，在升级了支持**Python3**的Vim8后（**在Vim版本中，不能同时支持Python2和Python3**），依然有些瑕疵，比如对Python3的第三方库的扩展不是很友好，对C/C++支持得还不错，不知道是不是我的插件安装有问题还是什么原因，折腾了两三天后，果断把Vim换成了[NeoVim](https://github.com/neovim/neovim)，[YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)换了另一个代码补全插件[coc-vim](https://github.com/neoclide/coc.nvim)，香得不行。在这一小节，介绍怎么**编译支持Python3的Vim8版本**，**前提是系统了安装Python3**，Python3的安装不做讨论。

1. **编译安装yum源里的vim最新版**

   [参考YouCompleteMe](https://github.com/ycm-core/YouCompleteMe/wiki/Building-Vim-from-source)里给出的方法，略作调整。

   首先检查系统里的Vim是否已经安装，在终端输入命令

   ```bash
   rpm -qa | grep vim
   ```

   如果有显示且没有提示任何缺包或者错误，则表示Vim已经被安装，如果没有安装，直接从第3步（**下载Vim最新版源代码**）开始阅读。

   一般情况下，运行下列命令安装Vim

   ```bash
   yum -y install yum*    // CentOS7.6系统
   ```

   查看vim版本

   ```bash
   vim --version
   ```

   可以看到，Vim版本一般是7.4，而现在Vim发行版已经发行到了第八代，包括很多新的功能，为了打造属于自己的IDE，必须升级Vim到最新版本。

   如果是Vim版本，查看Vim是否支持Python3

   ```bash
   vim --version | grep python
   ```

   如果结果显示python3前面是`+`号，则这一小节可以跳过，如果是`-`号，则需要自己编译Vim，需执行以下步骤。

2. **卸载Vim**

   在编译之前，需要卸载系统上的Vim版本

   - 如果使用`yum install vim*`命令安装的Vim，卸载Vim时执行

     ```bash
     yum remove vim
     ```

   - 如果Vim是使用`make`编译过的，在**CentOS7.6系统**下，进入到Vim源代码目录，执行

     ```bash
     sudo make uninstall
     ```

   如果Vim源代码已经删除，则需要重新[下载Vim源码](git clone https://github.com/vim/vim.git)，然后`cd vim`，再执行上述语句。

   如果是**Ubuntu系统**，卸载起来就比较方便

   ```bash
   sudo apt install checkinstall
   cd ~/vim       // vim是Vim源代码文件夹
   sudo checkinstall
   ```

3. **下载Vim最新版源代码**

   ```bash
   cd ~                                         // 进入到当前用户的根目录下
   git clone https://github.com/vim/vim.git     // 下载Vim源代码
   cd vim                                       // 进入到Vim源代码文件
   ```

4. **配置**

   ```bash
   ./configure --with-features=huge \
               --enable-multibyte \
               --enable-rubyinterp=yes \
               --enable-python3interp=yes \
               --with-python3-config-dir=$(python3-config --configdir) \
               --enable-perlinterp=yes \
               --enable-luainterp=yes \
               --enable-gui=gtk2 \
               --enable-cscope \
               --prefix=/usr/local
   ```

   命令[解释](https://www.jianshu.com/p/aac78ff576c5)：

   ` --with-features=huge`：支持最大特性

   `--enable-multibyte`：打开多字节支持，可以在Vim中输入中文

   `--enable-rubyinterp`：打开对ruby编写的插件的支持

   ` --enable-python3interp`：打开对python3编写的插件的支持

   `--with-python-config-dir`：Python3的路径

   `--enable-perlinterp`：打开对perl编写的插件的支持

   ` --enable-luainterp`：打开对lua编写的插件的支持

   `--enable-gui`：打开GUI支持

   `--enable-cscope`：打开对cscope的支持

   `--prefix=/usr/local/`：指定将要安装到的路径

5. **设置VIMRUNTIMEDIR**

   **VIMRUNTIMEDIR**是vim运行时候所需资源的目录，非常重要。

   ```bash
       make VIMRUNTIMEDIR=/usr/local/share/vim/vim82
   ```

6. **编译安装**

   ```bash
   sudo make install
   ```

7. **设置Vim为默认的编辑器**

   使用`update-alternative`设置vim为默认编辑器：

   ```bash
   sudo update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
   sudo update-alternatives --set editor /usr/local/bin/vim
   sudo update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
   sudo update-alternatives --set vi /usr/local/bin/vim
   ```

按照上述步骤完成后查看Vim**版本

```
vim --version
```

![vim版本](https://pic.downk.cc/item/5e89ec78504f4bcb042f0550.png)

可以看到Vim版本是8.2版本，已开启**python3支持**，需要进行验证，在终端输入`vim`，接着在**正常模式**下输入`:echo has("python3")`，按一下回车键，若屏幕最下面输出`1`，则配置正确，Vim支持Python3。

编译的时候可能会出现各种意想不到的问题，这个时候要多看专业的文档，比如官网文档、github及**Issues（非常有用）**、[stackoverflow](https://stackoverflow.com/)等，其次再考虑其它出处，尽可能多看**专业的英文文档**，很多问题我们习惯看一些野路子中文解答，看似走了捷径但是花费了更多的时间，切记！一些优秀的项目有对应的中文文档，首推！

该方法是全局设置方法。

- **安装中文vim帮助文档**

  进入Vim环境，按一下前缀键`esc`，在英文语法环境下，输入`:help`后（终端最下面显示），进入vim帮助文档，此界面是英文的，如果想要设置中文帮助文档，请执行以下步骤：

  - **下载中文安装包**

    ```bash
    wget https://github.com/yianwillis/vimcdoc/archive/v2.3.0.tar.gz
    ```

  - **解压**

    ```bash
    tar zxvf v2.3.0.tar.gz
    ```

  - **安装**

    ```bash
    cd vimcdoc-2.3.0/				# 进入解压后的目录
    ./vimcdoc.sh -i					# 安装
    ```

    命令会自动识别 Vim 的安装路径，并将中文的文档拷贝到相应的地方，原有的英文文档不受影响，查阅中文文档学习Vim。

### Vim8 —> NeoVim

完全是偶然。在上回讲到，在Vim下折腾了两三天的[代码补全插件YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)，使用起来并不是很顺手，比如对Python3的第三方库的扩展不是很友好，对C/C++支持得还不错，不知道是不是我的插件安装有问题还是什么原因，果断把Vim换成了[NeoVim](https://github.com/neovim/neovim)，[YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)换了另一个代码补全插件[coc-vim](https://github.com/neoclide/coc.nvim)，香得不行。

### 基本操作



基本模式

编辑模式

### 基础设置

[键盘映射技巧](https://zhuanlan.zhihu.com/p/38150203)

`"+p`可以将外部内容粘贴到终端vim文件中

### 插件管理

#### vim-plug插件管理器

Vim的插件管理有[Vundle](https://github.com/VundleVim/Vundle.vim)、[vim-plug](https://github.com/junegunn/vim-plug)、[SpaceVim](https://github.com/SpaceVim/SpaceVim)等，[Vundle](https://github.com/VundleVim/Vundle.vim)是一款非常出名且历史悠久的Vim插件管理工具，但随着安装的vim插件越来越多，使用Vundle来管理这些插件时效率变得越来越低，vim启动耗时也越来越大，且多年没有更新；[SpaceVim](https://github.com/SpaceVim/SpaceVim)挺酷炫，有时间可以尝试一下，在这里推荐使用[vim-plug](https://github.com/junegunn/vim-plug)，

**Unix安装**（包括CentOS和Mac）

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

该命令在Unix系统下无须进行修改，会自动创建目录`~/.vim/autoload/`。

更新vim-plug插件自身：

```bash
:PlugUpgrade
```

批量更新所有已安装的插件：

```bash
:PlugUpdate
```

查看插件状态

```bash
:PlugStatus
```

卸载插件：请先在**~/.vim/vimrc**或者**~/.config/nvim/init.vim**配置文件中注释或者删除对应插件的配置信息，然后再执行以下命令：

```
:PlugClean
```



#### 插件安装

假设系统已经安装了python3解释器，如没有安装，请参考这篇[文章]([https://github.com/kangzhiheng/GitLocalDoc#anaconda%E5%AE%89%E8%A3%85tensorflow-gpu](https://github.com/kangzhiheng/GitLocalDoc#anaconda安装tensorflow-gpu))

##### 美化启动界面  — vim-startify

https://github.com/mhinz/vim-startify

```bash
Plug 'mhinz/vim-startify'
```

![startigy](https://pic.downk.cc/item/5e8de221504f4bcb0466b21e.png)

在终端输入`nvim`后，显示nvim启动界面，列出最近打开的文件，很方便。

##### 美化状态栏 — ariline

https://github.com/vim-airline/vim-airline

```bash
Plug 'vim-airline/vim-airline'
```

![美化状态栏](https://github.com/vim-airline/vim-airline/wiki/screenshots/demo.gif)



##### 文件管理插件 — NERDTree

使用方法

Ctrl + w + w   窗口之间的跳转

当光标在NERDTree窗口时，

`shift+i`是打开/关闭隐藏文件

o - 展开/合并某个目录

C - 如果需要某个文件夹作为你的工作区，则需要按下大写C即可设置此目录为当前工作区

U - 跳转到上一层目录

cd - 设置该目录为家目录

CD - 可能在别的目录迷失了方向，直接输入`CD`跳转到已经设置好的家目录

##### 代码自动补全 — YouCompleteMe

```bash

call plug#begin('~/.vim/pluged')

" 插件配置

" 文件树
Plug 'scrooloose/nerdtree'

" 代码补全
Plug 'ycm-core/YouCompleteMe'

call plug#end()               
```

在Vim配置文件`~/.vim/vimrc`下的插件Vundle部分，加入[代码补全插件YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)，然后在**正常模式**下，键入`:PluginInstall`进行插件安装，安装完成后，会在命令行提出

```bash
The ycmd server SHUT DOWN (restart with ‘:YcmRestartServer’). YCM core library not detected; you need to compile YCM before using it. Follow the instructions in the documentation.
```

这是因为，**YouCompleteMe** 需要手工编译出库文件 ycm_core.so (以及依赖的libclang.so) 才可使用。**YouCompleteMe**支持的语言

- **C系列（C/C++/Objective-C/Objective-C++）**：编译时添加`--clangd-completer`
- **C#**：先安装[Mono](http://www.mono-project.com/docs/getting-started/install/linux/#debian-ubuntu-and-derivatives)，编译时添加`--cs-completer`
- **Go**：先安装[Go](https://golang.org/doc/install，编译时添加`--go-completer`
- **JavaScript**和**TypeScript**：先安装 [Node.js and npm](https://docs.npmjs.com/getting-started/installing-node#1-install-nodejs--npm)，编译时添加`--ts-completer`
- **Rust**: 编译时添加`--rust-completer`
- **Java**: 先安装[JDK8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，编译时添加`--java-completer`
- 全部支持：先安装所有的依赖环境，编译时添加`--all`

要使用`--all`功能进行安装，请确保已在`PATH`中安装了**msbuild**，**go**，**node**和**npm**工具。

个人对C系列使用较多，因此

```bash
cd ~/.vim/pluged/YouCompleteMe
python3 install.py --clangd-completer
```

如果系统没有设置默认的Python为Python3版本，则需要注意以上命令是`python3`开头的，这时**C系列**语言的代码自动补全功能配置完毕。

python语言自动补全

需要在Vim配置文件`~/.config/nvim/init.vim`下的插件Vundle部分，加入

```bash
 " YouCompleteMe配置                                                                                                                              
nnoremap gd :YcmCompleter GoToDefinitionElseDeclaration<CR>                                 nnoremap g/ :YcmCompleter GetDoc<CR>                                                       nnoremap gt :YcmCompleter GetType<CR>                                                       nnoremap gr :YcmCompleter GoToReferences<CR>                                                 let g:ycm_autoclose_preview_window_after_completion=0                                       let g:ycm_autoclose_preview_window_after_insertion=1                                         let g:ycm_use_clangd = 0                                                                     let g:ycm_seed_identifiers_with_syntax=1                                                     let g:ycm_collect_identifiers_from_tags_files = 1                                           let g:ycm_min_num_of_chars_for_completion=2                                                  let g:ycm_path_to_python_interpreter='/usr/bin/python3' 
```

注意python3解释器的位置，这个设置好像对第三方库不是很友好，我再探索探索。

##### 括号自动补全 — auto-pairs

[链接  ](https://github.com/jiangmiao/auto-pairs)

##### 函数列表 — TagBar

[链接](https://github.com/majutsushi/tagbar)

首先安装`ctags`

```bash
# CentOS7.6
sudo yum install ctags
# Ubuntu
sudo apt-get install ctags
# Mac
brew install ctags
```

在`~/.config/nvim/init.vim`里的**插件配置**里

```bash
" 显示函数列表                        
Plug 'majutsushi/tagbar'
```



##### 代码自动补全 coc-vim（推荐）

根据[官网](https://github.com/neoclide/coc.nvim)建议，安装coc-vim前，需要首先安装[nodejs](https://nodejs.org/en/download/)

```bash
curl -sL install-node.now.sh/lts | bash
```

利用vim-plus来进行安装

```bash
" Coc-vim 自动补全插件
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

保存并重启（更新）`~/.config/nvim/init.vim`文件，运行`PlugInstall`进行安装。

如果出现[错误](https://github.com/neoclide/coc.nvim/issues/651)

```bash
[coc.nvim] javascript file not found, please compile the code or use release branch.
```

参考[issue651](https://github.com/neoclide/coc.nvim/issues/651)，打开`~/.config/nvim/init.vim`文件，运行命令`:call coc#util#install()`可以解决该问题。

安装完成后，根据[项目](https://github.com/neoclide/coc.nvim/)对coc-vim插件进行配置，参考[链接](https://github.com/neoclide/coc.nvim/blob/master/doc/coc.cnx)（重要），在`~/.config/nvim/init.vim`下进行coc-vim插件进行个性化配置使其更加易用，这部分配置文件如下：

```bash
" ====
" ==== coc-vim插件配置
" ====
" 允许在有未保存的修改时切换缓冲区, 此时的修改由 vim 负责保存
set hidden

" 覆盖文件时不备份
set nobackup

" 表示编辑的时候不需要备份文件
set nowritebackup

" 命令行（在状态行下）的高度, 默认为1, 这里是2, 展示更多的信息
set cmdheight=2

" 根据默认设置，交换文件会每隔4000毫秒（4秒）或者200个字符保存一次
" 我们可以使用以下命令，修改保存交换文件的频率
" 较长的更新时间（默认值为4000 ms = 4 s）会导致明显的延迟和较差的用户体验。
set updatetime=300

" 历史原因，Vim的界面可能会显示“帮助乌干达的可怜儿童”，该命令将其屏蔽
" Don't pass messages to |ins-completion-menu|.
set shortmess+=c

" 始终显示标号列
set signcolumn=yes

" inoremap命令用于在“插入模式”中进行映射按键
" 使用TAB键来启动代码补全触发器，并在前面加上字符并进行导航。
" 注意：使用命令'：verbose imap <tab>'来确保选项卡未由
"，然后再将其放入配置中
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" 使用ctrl+空格键来强制触发补全
inoremap <silent><expr> <c-space> coc#refresh()

" <cr>表示空格键，<C-g>表示Ctrl+g
" 使用<cr>确认补全，“ <C-g> u”表示在当前位置断开撤消链
" Coc仅在确认时进行摘要和其他编辑
if has('patch8.1.1068')
  " Use `complete_info` if your (Neo)Vim version supports it.
  inoremap <expr> <cr> complete_info()["selected"] != "-1" ? "\<C-y>" : "\<C-g>u\<CR>"
else
  imap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
endif

"  键盘映射参考：https://zhuanlan.zhihu.com/p/38150203
" nmap是:map的普通模式板，也就是说其绑定的键只作用于普通模式 
" <silent> 表示静默映射，不会显示Vim在处理rhs(映射后的键位)过程中对界面产生的变化
" 使用`[g`和`]g`浏览诊断
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)

" 代码跳转
" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" 使用K键在预览窗口中显示文档
nnoremap <silent> K :call <SID>show_documentation()<CR>
function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction

" 按住光标时高亮符号及其引用
autocmd CursorHold * silent call CocActionAsync('highlight')

" 符号重命名
nmap <leader>rn <Plug>(coc-rename)

" 格式化所选代码
xmap <leader>f  <Plug>(coc-format-selected)
nmap <leader>f  <Plug>(coc-format-selected)
augroup mygroup
  autocmd!
  " Setup formatexpr specified filetype(s).
  autocmd FileType typescript,json setl formatexpr=CocAction('formatSelected')
  " Update signature help on jump placeholder.
  autocmd User CocJumpPlaceholder call CocActionAsync('showSignatureHelp')
augroup end

" 将codeAction应用于所选区域。
" 示例：<leader> aap 表示当前代码段
xmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)

" 将codeAction应用于当前行的键的重新映射
nmap <leader>ac  <Plug>(coc-codeaction)
" 自动修复当前行的代码问题
nmap <leader>qf  <Plug>(coc-fix-current)

" 介绍功能文本对象
xmap if <Plug>(coc-funcobj-i)
xmap af <Plug>(coc-funcobj-a)
omap if <Plug>(coc-funcobj-i)
omap af <Plug>(coc-funcobj-a)

" 将<TAB>用于选择范围
nmap <silent> <TAB> <Plug>(coc-range-select)
xmap <silent> <TAB> <Plug>(coc-range-select)

" 添加`:Format`命令格式化当前缓冲区
command! -nargs=0 Format :call CocAction('format')

" 添加`:Fold`命令折叠当前缓冲区
command! -nargs=? Fold :call     CocAction('fold', <f-args>)

" 添加`:OR`命令来组织当前缓冲区的import
command! -nargs=0 OR   :call     CocAction('runCommand', 'editor.action.organizeImport')

" 关于CoCList的一些键盘映射
" 显示所有的诊断
nnoremap <silent> <space>a  :<C-u>CocList diagnostics<cr>
" 管理所有的插件
nnoremap <silent> <space>e  :<C-u>CocList extensions<cr>
" 显示命令
nnoremap <silent> <space>c  :<C-u>CocList commands<cr>
" 找出当前文件的符号
nnoremap <silent> <space>o  :<C-u>CocList outline<cr>
" 在当前文件进行搜索
nnoremap <silent> <space>s  :<C-u>CocList -I symbols<cr>
" 对下一项执行默认操作
nnoremap <silent> <space>j  :<C-u>CocNext<CR>
" 对前一项执行默认操作
nnoremap <silent> <space>k  :<C-u>CocPrev<CR>
" 恢复最新的coc列表
nnoremap <silent> <space>p  :<C-u>CocListResume<CR>
```

Language Server Protocol，[LSP](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#implemented-coc-extensions)以及[链接](https://github.com/neoclide/coc.nvim/wiki/Language-servers)

**配置C/C++环境**

安装clangd，需要`cmake>=3.12`和`gcc>=5.1`

首先检测cmake版本

```bash
cmake --version
```

如果其版本小于3.12，则需要升级cmake，下面是编译最新cmake版本的脚本，在终端里新建`build_camke.sh`脚本，将下面的内容复制到脚本里

```bash
#!/bin/bash                                                                                                                                                                    
yum remove cmake -y && rm -f /usr/bin/cmake
wget -c https://github.com/Kitware/CMake/releases/download/v3.14.2/cmake-3.14.2.tar.gz
tar xvf cmake-3.14.2.tar.gz 
cd $(pwd)/cmake-3.14.2   
./bootstrap              
gmake                    
gmake install            
sudo ln -s /usr/local/bin/cmake /usr/bin/
cmake --version
```

然后在终端里运行

```bash
bash build_camke.sh
```

即可，如果出现问题，可以一步一步在终端运行命令。

接着查看gcc版本信息

```bash
gcc --version
```

如果版本小于5.1，则需要对gcc进行[升级](https://tlanyan.me/install-upgrade-gcc-on-centos/)

```bash
sudo yum install devtoolset-4-gcc devtoolset-4-gcc-c++
scl enable devtoolset-4 bash
```

再查看gcc版本，输出

```bash
gcc (GCC) 5.3.1 20160406 (Red Hat 5.3.1-6)
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

接下来编译`clangd`，参考[链接](https://clangd.llvm.org/installation.html)、[链接](![image-20200408000824059](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\image-20200408000824059.png))、[链接](https://gist.github.com/jakob/929ed728c96741a119798647a32618ca)

- 在**Mac OS**下，直接运行

  ```
  brew install llvm
  ```

- 在**Windows**下，下载预编译好的[版本](https://releases.llvm.org/download.html)，安装即可

- 在**Debian/Ubuntu**系统下，运行代码

  ```
  sudo apt-get install clangd-9
  ```

  `clangd `的二进制文件将被安装在`/usr/bin/clangd-9`，将其设置为默认的连接符

  ```bash
  sudo update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-9 100
  ```

- **Other systems（CentOS等）**

  服务器因为是CentOS7.6版本，所有需要自己编译，通过查阅无数资料，`clangd`的编译脚本如下：

  ```bash
  #!/bin/bash
  
  set -e
  
  mkdir clangd
  cd clangd/
  svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm
  
  cd llvm/tools
  svn co http://llvm.org/svn/llvm-project/cfe/trunk clang
  cd ../..
  
  cd llvm/tools/clang/tools
  svn co http://llvm.org/svn/llvm-project/clang-tools-extra/trunk extra
  cd ../../../..
  
  mkdir build
  cd build
  cmake -G "Unix Makefiles" ../llvm
  make clangd
  
  sudo update-alternatives --install /usr/bin/clangd clangd ./bin/clangd 100
  ```

将其保存为`build-clangd.sh`，在终端**根目录**(`cd ~`进入自己的根目录下)运行`bash build-clangd.sh`进行编译，我大概花费了**7个小时**左右的时间进行编译

![build clangd](https://pic.downk.cc/item/5e8cab56504f4bcb046002b7.png)

如图所示，编译完成，二进制文件在`~/clangd/build/bin`目录下，脚本的`Line 22`设置默认的`clangd`链接符，以便系统对`clangd`进行识别，在终端输入`clangd --version`，若可以看到版本信息且没有报错，则表明安装成功。若有类似的提示：未知的clangd命令，则可能需要将其二进制文件添加到系统`PATH`下，打开

```bash
nvim ~/.bashrc
```

在最后一行，加入`export PATH="~/clangd/build/bin:$PATH"`，即添加`clangd`编译好的二进制文件目录路径到`PATH`，再运行

```bash
source ~/.bashrc    // 更新文件，即时生效
sudo update-alternatives --install /usr/bin/clangd clangd ~/clangd/build//clangd 100
```

安装`coc-clangd`

打开`~/.vim/vimrc`文件，运行命令`:CocInstall coc-clangd`，coc-clangd[不需要在coc-settings.json里进行设置](https://github.com/clangd/coc-clangd#quick-start)，至此在nvim里的C/C++智能代码感知功能安装完成。

你以为到这儿就结束了吗？本来我是这么以为的，直到……

我创建了一个示例文件试试代码补全的功能，一顿操作

```bash
nvim ~/a.cpp
```

输入内容

```
#include <iostream>
#include <vector>        
#include <unordered_map> 
#include <queue>
#include <list>          

using namespace std;     

int main()               
{
	cout << "hello world!" << endl;                         
	
	return 0;            
}
```

在终端运行`g++ a.cpp -o a`，然后翻车了

![编译error](https://pic.downk.cc/item/5e8d4a68504f4bcb04df533a.png)

啥意思呢？就是说`a.py`用到了**C++11**的新特性（#include <unordered_map> ），而当前g++版本是4.8.5（g++ --version查看），默认支持**C++98**，所以如果要使用C++11的新特性，在编译时加上`-std=c++11`或`-std=gnu++11`，即

```bash
g++ -std=c++11 a.cpp -o a
```

**配置Python环境**

参考[链接](https://github.com/neoclide/coc-python)，首先需要安装[python-language-server](https://github.com/palantir/python-language-server)

```bash
sudo pip3 install 'python-language-server[all]'
```

接着安装[Jedi](https://jedi.readthedocs.io/en/latest/docs/installation.html)

```bash
sudo pip3 install jedi
```

此时语言解析器配置完成。

在终端输入`nvim`，运行命令`:CocInstall coc-python`，显示`[coc.nvim]Installed extension: coc-python`，表示安装完成。

在系统里，可能不同的python脚本需要不同的python解释器，系统也允许存在不同的python解释器，所以在打开你想要编辑的python文件后，需要设置你所有需要的python解释器，假设`a.py`需要python3解释器，

```bash
nvim a.py
```

然后在**普通模式**下输入命令`:CocCommand`，接着找到`python.setInterpreter`，找到你所需要的环境即可，这时安装在此环境下的包才会出发自动补全功能。

![coc-python](https://pic.downk.cc/item/5e8dbdda504f4bcb0443d423.png)

其它语言请参考[LSP](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#implemented-coc-extensions)

##### 缩进显示 — indentLine

```bash
" 显示缩进标注, Python语言必须
Plug 'Yggdroot/indentLine'
```

##### 文件操作历史 — Undotree

```bash
" 列出文件最近的操作历史
Plug 'mbbill/undotree'
```

##### 命令行模糊搜索工具 — fzf/fzf-vim

[fzf](https://github.com/junegunn/fzf)

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

[fzf-vim](https://github.com/junegunn/fzf.vim)

```bash
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
```

要想使用文件内搜索命令，需要提前安装[ag](https://github.com/ggreer/the_silver_searcher)

```bash
sudo yum install the_silver_searcher
```



### 一些要点



### 一些问题

- :PluginInstall 不是编辑器命令？

  两个原因

  - 路径配置出错

  目前来看是路径问题，根据[官网](https://github.com/VundleVim/Vundle.vim/)描述，安装命令为

  ```
  git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
  ```

  可能在~/.vim/vimrc配置文件中把**bundle**写成了**bunlde**，**Vundle**写成**Vumdle**……真的醉了……浪费了两个小时时间……

  - 实在没有办法，将安装命令调整为

  ```
  git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/plugin/Vundle.vim
  ```

  哔了狗了。

- **Vim卡住，死机？**

  **原因**：

  使用vim时，会因为不小心按了Ctrl + s后，会发现不能输入任何字符，类似死机；

  这并不是Linux死机，或者vim卡住，而只是按了Ctrl + s后，vim停止向终端输出。

  **解决方法**：

  退出这种状态，按下**Ctrl + q** 。

- 升级**Ruby**

  打开`nvim`，输入命令`:CheckHealth`，如果提示Ruby版本过低，则需要升级

  1. 查看当前Ruby版本

     ```bash
     ruby --version
     ```

     显示的是**ruby 2.0.0p648 (2015-12-16) [x86_64-linux]**

  2. 安装yum源

     ```bash
     yum install -y centos-release-scl-rh
     ```

  3. 安装指定版本的ruby

     ```bash
     yum install -y rh-ruby24
     ```

  4. 使升级后的ruby版本生效

     ```bash
     scl enable rh-ruby24 bash
     ```

  5. 查看是否升级成功

     ```bash
     ruby --version
     ```

     显示**ruby 2.4.6p354 (2019-04-01 revision 67394) [x86_64-linux]**

  **但是**，这种方法，**只会在当前环境下有效**，也就是说，退出当前环境就没有用了，还要执行第5步，所以最好对的方法是重新编译，方法如下

  ```bash
  wget http://cache.ruby-lang.org/pub/ruby/ruby-2.4.10.tar.gz
  tar xvfvz ruby-2.4.10.tar.gz
  cd ruby-2.4.10
  ./configure
  make
  sudo make install
  ```

- 安装[screenkey](https://gitlab.com/screenkey/screenkey)

  ```bash
  sudo yum install python-distutils-extra    // optional
  wget https://www.thregr.org/~wavexx/software/screenkey/releases/screenkey-0.9.tar.gz
  tar -zxvf screenkey-0.9.tar.gz
  cd cd screenkey-0.9/
  ./screenkey
  sudo ./setup.py install
  ```


### 结语

nvim的外壳[onivim](https://github.com/onivim/oni)

### 后续计划

插件相关

- [ ] Python交互式编程[vim-repl](https://github.com/sillybun/vim-repl)
- [ ] git相关插件

Linux相关

- [ ] 在客户端界面下安装[i3](https://i3wm.org/)
- [ ] 使用**fish**和**oh my fish**
- [ ] 结合**tmux**工具
- [ ] 配置[oh my zsh](https://github.com/robbyrussell/oh-my-zsh)和[主题](https://github.com/bhilburn/powerlevel9k)
- [ ] 更换系统字体为[Monaco](http://www.mycode.net.cn/wp-content/uploads/2016/06/Monaco.zip)
- [ ] 更新[zsh版本](https://www.jianshu.com/p/0659d5561502)

### 参考

- https://juejin.im/post/5a38c37f6fb9a0450909a151#heading-1
- [编译支持python3的Vim](https://github.com/ycm-core/YouCompleteMe/wiki/Building-Vim-from-source)

- [Vim插件管理Vundle](https://github.com/VundleVim/Vundle.vim)
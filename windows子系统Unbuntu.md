# WSL 开发环境问题汇总

## Ubunt

##### 1、终端显示中文编码不对

```linux
#修改文件内容
$ vim /etc/default/locale

#将编码方式改成UTF-8
 LANG=en_US.UTF-8
 LANGUAGE=zh_CN.UTF-8
 
 
#------------------------------
#安装UTF-8 语言支持包
sudo locale-gen en_US.UTF-8

sudo vim /etc/default/locale

删除全部,添加如下

LC_ALL="en_US.UTF-8"

source /etc/default/locale

或者
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

或者
vim .profile
加入
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
source .profile
```

## Git 

##### 1、git命令显示乱码问题

```
#在命令行下输入以下命令
$ git config --global core.quotepath false          # 显示 status 编码
$ git config --global gui.encoding utf-8            # 图形界面编码
$ git config --global i18n.commit.encoding utf-8    # 提交信息编码
$ git config --global i18n.logoutputencoding utf-8  # 输出 log 编码
$ export LESSCHARSET=utf-8
# 最后一条命令是因为 git log 默认使用 less 分页，所以需要 bash 对 less 命令进行 utf-8 编码

```



##### 2、更新拉取代码频繁输入账号密码问题

```linux
# 1、进入项目目录 输入
git config --global credential.helper store

# 使用上述的命令配置好之后，再操作一次，然后它会提示你输入账号密码，这一次之后就不需要再次输入密码了

```



## Debian

##### 1、终端显示中文

```shell
#终端输入命令
sudo dpkg-reconfigure locales
#选择
#zh_CN.UTF-8 UTF-8
#默认locale还是选en_US.UF8  一般到这里就可以了

#选择好后自动加载中文环境：但是注意可能没有对应的字库，我们需要手动下载中文字库：

apt --fix-broken install xfonts-intl-chinese xfonts-wqy -fy
apt install ttf-wqy-zenhei -y  //这个是用来修复谷歌浏览器中文乱码问题	

#@可以直接修改文件

\etc\locale.gen  //此文件对需要的语言取消注释等同于上面选择zh_CN.UTF-8 UTF-8

\etc\default\locale   //此处设置启动是想要的默认语言 eg:LANG=zh_CN.UTF-8

locale-gen  //执行此命令，加载上面两部修改
```



## Subversion

##### 1、使用命令时一直要输入账号密码问题

```linux
#进入.subversion目录修改
cd ~/.subversion/auth/svn.simple
#修改文件
vim xxxxx
 
#你会发现一下内容   “K”是后跟密钥长度的键，“V”是后跟值长度的值
K 15
svn:realmstring
V 37
<svn://home.sweet.home:3690> Profekto
K 8
username
V 6
lserni
END

#在文件的顶部，手动添加了提供明文密码的八行：
K 8
passtype
V 6
simple
K 8
password
V 改成你密码的长度
这里输入你的密码
```

## wls问题汇总

##### 1、去掉 windows 的环境变量

```shell
# 1、在 wsl 下新建 /etc/wsl.conf 配置文件，并编辑如下内容：

[interop]
 appendWindowsPath = false
 
# 2、在控制台执行以下命令，重启 wsl 即可： 
wsl --terminate <distro>
```



##### 2、关闭WSL的提示音（或窗口抖动）

```
# 1. vim 打开配置文件
sudo vim /etc/inputrc

# 2. vim搜索 bell-style
:/bell-style

# 3. 取消下面展示的这一行的注释
bell-style = none

# 4. ctrl+d 退出 wsl 再重新进入

# 5. Vim 设置
set visualbell

```


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


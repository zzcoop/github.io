# Ubuntu问题汇总

##### 1、终端显示中文编码不对

```linux
#修改文件内容
$ vim /etc/default/locale

#将编码方式改成UTF-8
 LANG=en_US.UTF-8
 LANGUAGE=zh_CN.UTF-8
```

##### 2、git命令显示乱码问题

```
#在命令行下输入以下命令
$ git config --global core.quotepath false          # 显示 status 编码
$ git config --global gui.encoding utf-8            # 图形界面编码
$ git config --global i18n.commit.encoding utf-8    # 提交信息编码
$ git config --global i18n.logoutputencoding utf-8  # 输出 log 编码
$ export LESSCHARSET=utf-8
# 最后一条命令是因为 git log 默认使用 less 分页，所以需要 bash 对 less 命令进行 utf-8 编码
```

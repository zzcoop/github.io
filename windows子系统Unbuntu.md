# Ubuntu问题汇总

##### 1、终端显示中文编码不对

```linux
#修改文件内容
vim /etc/default/locale

#将编码方式改成UTF-8
 LANG=en_US.UTF-8
 LANGUAGE=zh_CN.UTF-8
```




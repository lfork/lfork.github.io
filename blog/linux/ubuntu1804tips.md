# Ubuntu 1804 使用技巧
1、双系统如何自动挂载C盘、D盘？

2、ZIP解压乱码？
unzip -O cp936  xxx.zip


# 免密登录
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys

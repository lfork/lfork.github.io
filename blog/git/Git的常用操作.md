# 常用操作

## 代理相关

### Socks5代理

1、设置全局SOCK5代理

```shell
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

2、取消代理

```shell
git config --global --unset http.proxy

git config --global --unset https.proxy
```

**局部代理** 只需要把--global删掉即可

## 自动操作

### 自动输入用户名和密码

设置pull和push自动输入用户名和密码。
执行下面的命令后需要输入一次用户名和密码，以后就不用了。

```shell
git config --global credential.helper store

git pull /git push (这里需要输入用户名和密码)
```

**单个项目** 只需要把--global删掉即可

## SSH免密

### Windows下的Github免密操作

### Windows下其他Git服务器的免密操作

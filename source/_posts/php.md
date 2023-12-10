---
title: php
date: 2023-12-09 11:04:26
tags: 工程
typora-root-url: ./php
---

# PHP

> 本来差一点做好了，结果不小心删库了，只能重新做了
>
> 同时还遇到了`nginx`的问题，麻上加麻

## 项目

在`github`上拉的一个项目，名字叫做[SyncMusic](https://github.com/kasuganosoras/SyncMusic)。需要的插件很多，我们一个一个下载，下面是一些需要的插件。

- `PHP Swoole`
- ` Python 3.6 以及 pip`
- `Nginx`

那闲话不多说，就开始安装吧。

## 安装`php`

在纯净的`linux`系统中，我尝试了项目给的教程安装`php swoole`，但是发现不成功，查找了一下原因发现是给的链接证书过期了，那就只能自己安装了。

参考的[安装链接](https://learnku.com/articles/58890)

```shell
wget https://www.php.net/distributions/php-7.3.8.tar.gz
```

老样子，解压

```shell
tar -zxvf php-7.3.8.tar.gz
#configure
./configure
```

果不其然，缺少东西，那就安装呗

```shell
#安装 libxml2 库：
sudo yum install libxml2 libxml2-devel
```

然后执行下面的命令，等待就行

```shell
./configure --prefix=/usr/local/php --enable-mbstring --enable-fpm
```

执行`php-v`发现没有出现`php`的版本，添加一下环境变量

```
export PATH=/usr/local/php/bin:$PATH
```

![image-20231209172850504](image-20231209172850504.png)

### 安装`swoole`

[安装教程](https://developer.aliyun.com/article/558488)

进去安装目录：

```shell
cd /usr/local/src
#下载
wget https://github.com/swoole/swoole-src/archive/v1.9.1-stable.tar.gz
#解压
tar zxvf v1.9.1-stable.tar.gz
```

![image-20231209173743183](image-20231209173743183.png)

```shell
#找到这个插件的路径
find / -name phpize
#执行
/usr/local/php/bin/phpize
```

报错了，发现又是少了插件，我们执行

```shell
# 安装autoconf
sudo yum install autoconf
```

```shell
# 用这个路径安装
./configure --with-php-config=/usr/local/php/bin/php-config
```

开始编译

```shell
make && make install
```

然后进去`/usr/local/php/lib/php.ini`，用vim添加一段

```shell
extension=swoole.so
```

检查输出

```shell
php -m | grep swoole
```

![image-20231209181126924](image-20231209181126924.png)

重启服务器

```shell
sudo service php-fpm restart
sudo service nginx restart
```

## 安装`wget`

这个`wget`是安装的命令，先看一下有没有安装：

![image-20231209165133696](image-20231209165133696.png)

发现安装了，但是还是要执行一下。我使用的系统是`cenos7`，所以说安装命令是`yum`

```shell
rpm -qa wget
yum -y install wget
```

然后安装`gcc`编译器，

```shell
rpm -qa gcc
yum install gcc gcc-c++
```

## 安装`Nginx`

命令是这个，我建议还是在`home`这个路径创一个自己的文件夹来安装文件比较好

![image-20231209165633338](image-20231209165633338.png)

```shell
wget https://nginx.org/download/nginx-1.24.0.tar.gz
```

下载解压安装

```shell
tar -zxvf nginx-1.20.1.tar.gz
./configure --prefix=/usr/local/nginx
```

然后理所应当失败了，发现缺少 PCRE（Perl Compatible Regular Expressions）库

![image-20231209165810547](image-20231209165810547.png)

```shell
#安装呗
sudo yum install pcre pcre-devel 
```

然后就成功了：
![image-20231209170151342](image-20231209170151342.png)

使用命令`make && make install`来编译，安装好了。

```shell
make && make install

#目录
/usr/local/nginx
```

![image-20231209170803165](image-20231209170803165.png)

然后启动一下

```shell
# 在安装目录`/usr/local/nginx/sbin/`目录下有一个输入命令
[root@localhost sbin]# ./nginx

# 启动后查看进程
[root@localhost sbin]# ps aux|grep nginx
```

## 安装Python 3.6 以及 pip

```shell
yum install python36 python36-pip -y
pip3 install mutagen
```

## 部署

在路径`/home`中拉入我们的项目

```shell
git clone https://github.com/kasuganosoras/SyncMusic/
```

前提是要安装`git`

```shell
sudo yum install git
```

然后启动 `server.php`

![image-20231209182536562](image-20231209182536562.png)

```shell
#让它在后台运行
nohup /usr/local/php/bin/php server.php > server.log 2>&1 &
```

用`ps-x`可以看到在运行了

![image-20231209182638058](image-20231209182638058.png)

配置一下`index.html`

![image-20231209183049742](image-20231209183049742.png)

```shell
#配置这里改一下你自己的ip
```

接下来配置前端页面，我们的服务器是在`nginx`的嘛，所以说配置一下`nginx`

![image-20231209183154488](image-20231209183154488.png)

```shell
#找到nginx.conf这里是我们网页的配置
```

![image-20231209184457970](image-20231209184457970.png)

然后用我们的ip登陆，就可以找到项目了

![image-20231209184515337](image-20231209184515337.png)

但是这里有个问题。是关于`https`协议的，有个文件被拦截了

![image-20231209184546788](image-20231209184546788.png)

## 最终

那就只能申请`https`了，先更改配置文件

![image-20231209194543588](image-20231209194543588.png)

对`nginx.conf`操作，申请证书，还是不行唉，放弃了，只能之后再部署了。




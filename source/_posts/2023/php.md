---
title: php
date: 2023-12-09 11:04:26
tags: 工程
typora-root-url: ./php
---

# `PHP`远程聊天

> 完成这个项目的路程有点坎坷，途中换了个项目，这里保存我下载的内容。

## 项目

在`github`上拉的一个项目，名字叫做`swoole-webim-demo`。需要的插件很多，我们一个一个下载，下面是一些需要的插件。

- `PHP Swoole`
- `Nginx`

最后觉得安装软件太麻烦了，直接用宝塔面板傻瓜安装了。

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
make && make install
```

执行`php-v`发现没有出现`php`的版本，添加一下环境变量

```
export PATH=/usr/local/php/bin:$PATH
```

![image-20231209172850504](image-20231209172850504.png)

> 这里我重新安装的时候遇到了一个问题，就是找不到`php.ini`这个文件了，只能从源码拉过来一个：
>
> 先确定`php.ini`的位置
>
> ```shell
> php -r "phpinfo();" | grep 'php.ini'
> ```
>
> ![image-20231211210606030](image-20231211210606030.png)
>
> 去安装位置拉取我们的php.ini
>
> ![image-20231211210652252](image-20231211210652252.png)
>
> 一个是开发用的，一个是生产环境用的，复制到对应的位置就行：
>
> ```shell
> cp /home/php-7.3.8/php.ini-development /usr/local/php/lib
> ```
>
> 然后里面的内容注意！！！
>
> ```shell
> ;
> #这个分号是注释！！！！
> ```

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

如果没有，我们执行命令` php --ini`，来找到`php.ini`

![image-20231211203841025](image-20231211203841025.png)

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

这个`wget`是安装的命令，`wget` 是一个在命令行中使用的工具，用于从网络上下载文件。先看一下有没有安装：

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

下载解压

```shell
tar -zxvf nginx-1.24.0.tar.gz
```

缺少` PCRE`（Perl Compatible Regular Expressions）库

```shell
#安装呗
sudo yum install pcre pcre-devel 
```

```shell
#编译
./configure --prefix=/usr/local/nginx
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

但是这里系统无法识别到， `Nginx `添加到系统 PATH 中，可以使用 `echo` 命令将路径添加到 shell 配置文件，而不必手动编辑。以下是一个简单的例子：

```shell
echo 'export PATH=/usr/local/nginx/sbin:$PATH' >> ~/.bashrc
```

上述命令将新的 PATH 配置添加到 `~/.bashrc` 文件的末尾。然后，运行以下命令以使更改生效：

```shell
source ~/.bashrc
```

## 项目具体

依据项目的部署[文档](https://github.com/hellosee/swoole-webim-demo)，部署完的项目截图：
![image-20240101212022676](image-20240101212022676.png)

下面是一些用到的知识点

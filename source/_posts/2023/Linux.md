---
title: Linux
date: 2023-04-30 22:29:58
tags: 工程
categories: 工程
typora-root-url: Linux
swiper_index: 7
---

## 常用命令

### 快捷操作

tab === 补齐功能

history [n] === 查看历史命令，`history n` 可执行第n个命令

![image-20240220152456740](image-20240220152456740.png)

上下键 === 切换上下命令

home 和 end === 光标移动至行首或行尾

clear 和 Ctrl + L === 清屏

### 电源命令

shutdown === 关闭计算机，仅 root 用户可用

`-h` 关机后关电源

`r` 关机后打开电源（重启）

`-t` 在改变到其他运行级别之前，告诉 init 程序多久以后关机

`-k` 并不是真正的关机，发送关机信号给每位登录用户

`-F` 重启计算机后强制fsck（文件系统检测）

`- time` 设定关机前的时间

halt === 关闭系统

`-n` 保存数据后重启

`-f` 强制关闭

`-i` 关机前会关掉所有网络接口

`d` 关闭系统，不留下日志记录

reboot === 重启

`-i` 重启前关闭网络设置

### 用户操作

passwd [用户名] === 修改密码

su username === 切换用户

sh === 切换shell操作

ip addr === 查看ip

chmod === 增加文件的可执行权限

last === 登录记录 `-n` 显示最近n条记录

### 文件操作

pwd === 显示当前工作目录

`-L` 目录连接链接时输出连接路径

![image-20240220152640831](image-20240220152640831.png)

`-P` 输出物理路径

![image-20240220152659206](image-20240220152659206.png)

cd === 返回主目录

`/url` 进入 url 目录

`..` 返回上一级

`.` 进入当前目录

`-` 进入前一个目录

![image-20240220152806866](image-20240220152806866.png)

`~` 进入主目录

ls === 列出当前目录下所有文件和目录

`-a` 显示所有文件和目录

`-l` 列出详细

`-t` 按文件建立时间先后次序列排序

`-R` 子目录也列出

mkdir === 创建目录

![image-20240220152915462](image-20240220152915462.png)

cp === 复制文件或目录

`-a` 保留链接、文件属性

![image-20240220152947557](image-20240220152947557.png)

`-p` 复制修改时间和访问权限

`-r` 复制改目录下的所有子目录和文件

`-l` 不复制文件只生成链接文件

mv === 移动文件或目录

![image-20240220153017924](image-20240220153017924.png)

`-b` 覆盖文件前先备份

`-f` 强制移动，如果目标文件已存在，直接覆盖

`-i` 若目标文件已存在，则询问是否覆盖

`-u` 若目标文件已存在，且 source 比较新，才会更新

rm === 删除文件

`-f` 忽略不存在的文件，强制删除

`-i` 进行互交式删除

`-r` 递归删除

`-v` 详细显示进行步骤

cat === 读取文件全部内容，cat >(>>) filename === 编辑文件，cat file1 file2 > file3 === 将文件合并为file3

`-A` 显示不可打印的字符

`-b` 对非空输出行编号

`-E` 在每行结束处显示$

`-s` 当有多个空行在一起时只输出一个空行

`-n` 由1开始对输出的所有行编号

### 分页和查找

head === 显示文件开头10行内容，`-n` 显示前n行

![image-20240220153707039](image-20240220153707039.png)

`-q` 隐藏文件名

`-v` 显示文件名

`-c` 显示字节数

tail === 读取文件尾部

![image-20240220153730182](image-20240220153730182.png)

`-f` 循环读取

`-q` 不显示处理信息

`-v` 显示详细的处理信息

`-c` <数目>显示的字节数

`-n` <行数>显示行数

more === 逐页阅读文件

![image-20240220153757886](image-20240220153757886.png)

`+n` 从第 n 行开始显示

`-n` 定义屏幕大小为 n 行

`+/pattern` 搜索改字串（pattern），然后从该字串前两行开始显示

`-c` 全屏再显示

快捷键：

`Enter` 向下 n 行，需自己定义

`Ctrl+F` or `空格键` 向下滚动一页

`Ctrl+B` 返回上一页

`=` 输出当前行的行号

`V` 调用 vi 编辑器

`!命令` 调用 shell 并执行命令

`q` 退出

less === 读取内容，在查看之前不会加载整个文件

`/字符串` 向下搜索

`?字符串` 向上搜索

`q` 退出

`空格键` 滚动一页

`Enter` 滚动一行

find === 在指定目录下查找文件

`-name` 按照文件名查找

`perm` 按照文件权限查找

`-user` 按照文件属主查找

`-type` d - 目录 f - 一般文件 l - 软链接

`-mtime -n +n` 按照文件更改时间查找

which === 在 PATH 所指定的目录中查找可执行文件

cut === 提取列或字段

`-b` 仅显示行中指定直接范围的内容

`-c[范围]` 仅显示行中指定范围的字符

`-d` 指定字段的分隔符，默认字段分隔符为 TAB

`-f[范围]` 显示指定第 num 个字段内容，可以用逗号隔开显示多个字段

### 压缩和打包

gzip === 进行压缩或解压文件

`-d` or `--decompress` or `---uncompress` 解开压缩文件

`-f` or `-force` 强行压缩文件

`-l` or `--list` 列出压缩文件相关信息

`-r` or `--recursive` 递归处理，将指定目录下及子目录一并处理

`-v` or `--verbose` 显示执行过程

tar === 打包文件或目录

`-c` 建立新的压缩文件

`-x` 从压缩文件中提取文件

`-t` 显示压缩文件内容

`-z` 支持 gzip 解压文件

`-j` 支持 bzip2 解压文件

`-v` 显示操作过程

`-C` 指定解压的路径

ln === 创建链接文件（默认硬链接）

`-b` 删除，覆盖以前建立的链接

`-d` 允许超级用户制作目录的硬链接

`-f` 强制执行

`-i` 交互模式，文件存在则提示用户是否覆盖

`-n` 把符号链接视为一般目录

`-s` 软连接（符号链接）

## 用户命令

useradd === 创建用户账号，并保存到 /etc/passwd 文件中

![image-20240220154114166](image-20240220154114166.png)

![image-20240220154234874](image-20240220154234874.png)

## RPM & DNF

rpm === 用于安装、删除、升级、刷新和查询软件

`-i` 指定安装的软件包

`-h` 使用 ‘#（hash）’ 符显示 rpm 详细的安装过程及进度

`-v` 显示安装的详细过程

`-U` 升级指定的软件包

`-q` 查询系统是否已安装指定的软件包或查询指定 rpm 包内容信息

`-a` 查看系统已安装的所有软件包

`-V` 查询已安装的软件包的版本信息

`-c` 显示所有配置文件

`-p` 查询/校验一个软件包文件

DNF === 软件源服务，软件源是 Linux 系统免费的应用程序安装仓库

安装 DNF 包管理器

```shell
复制# 必须先安装并启用 epel-release 依赖
yum install epel-release (-y)
# 使用 epel-release 依赖中的 yum 命令来安装 DNF 包
yum install dnf
```

DNF配置文件（/etc/dnf/dnf.conf）

配置远程软件源仓库

```shell
name=repository_name
baseurl=repository_url
```

```shell
# 配置本地软件源仓库
dnf install createrepo
# 将需要的软件包放置在目录下
createrepo --database /目录
```

## 源代码软件安装

1. 下载源码包并解压（校验包完整性）

   ```shell
   复制wget /https://*/*.tgz
   tar -zxvf *.tgz
   ```

2. 查看README和INSTALL文件

   ```shell
   复制cat README
   ```

3. 创建Makefile文件 - 通过执行 `./configure` 脚本命令生成

   ```shell
   复制./configure --prefix=/usr/local/name
   ```

4. 编译 - 在包目录下通过 make 命令将源码自动编译成二进制文件

   ```shell
   复制make
   make install
   ```

5. 安装软件 - 通过 make install 安装命令来将上步编译出来的二进制文件安装到对应的目录中区，默认的安装路径为 `/usr/local/`，相应的配置文件位置为 `/usr/local/etc`

## systemd管理服务

```shell
# 查看当前正在运行的服务
systemctl list -units --type service
# 运行服务
systemctl start name.service
# 关闭服务
systemctl stop name.service
# 重启服务
systemctl restart name.service
# 启用服务
systemctl enable name.service
# 禁用服务
systemctl disable name.service
```

## 磁盘信息

df -h === 查看系统挂载、磁盘空间大小和利用率

fdisk === 查看系统所有磁盘信息（DOS、BSD、SUN方案）

必要参数：

`-l` 列出所有分区表

`-u -l` 显示分区数目，会用分区数目取代柱面数目，来表示每个分区的起始地址

`-b<分区大小>` 指定没饿过分区的大小

选择参数：

`-s<分区编号>` 如（/dev/sda1）将指定的分区大小输出到标准输出上，单位为区块

`-v` 显示版本信息

菜单操作说明

`m` 显示菜单和帮助信息

`a` 活动分区标记 / 引导分区

`d` 删除分区

`l` 显示分区类型

`n` 新建分区

`p` 显示分区信息

`q` 退出不保存

`t` 设置分区号

`v` 分区检查

`w` 保存修改

`x` 拓展应用，高级功能

```shell
# 显示当前分区情况
fdisk -l
# 显示硬盘的每个分区情况
fdisk -lu
```

parted === 分区软件（GPT方案）

`-l` 列出分区

`-h` 显示帮助信息

`-i` 互交模式

`-s` 脚本模式

`-v` 显示 parted 的版本信息

`/dev/sda` 打开存储设备

`command` parted 指令，如果没有设置指令，则 parted 将会进入交互模式

```shell
# 列出分区
parted -l
# 选择操作磁盘
parted /dev/sda
# 设置分区表为 GPT 并输入 yes 开始执行
(parted) [mklabel | mktable] gpt # => mklabel & mktable 不会创建分区，而是创建分区表
Yes/No? ==> Yes
# 查看存储设备信息
(parted) print
# 获取帮助
(parted) help mkpart
# 创建分区(分区0有1396MB)
(parted) mkpart primary 0 1396MB
# 保存退出
(parted) quit
```

### 磁盘格式化

mkfs === 双击`tab` 可查看支持的文件类型

mkfs.ext4 /dev/sdb2 === 格式化 sdb2 分区类型为 ext4

`device` 预备检查的硬盘分区

`-V` 详细显示模式

`-t` 给定档案系统的形式，Linux 的预设值为 ext2 `mkfs -t ext2 /dev/sda

`-c` 在制作档案系统前，检查该 partition 是否有坏轨

`-l bad_block_file` 将有坏轨的 block 资料加到 bad_blocks_file 里面

`block` 给定 block 的大小

### 逻辑卷管理

[![逻辑卷创建流程](https://krora.gitee.io/bamboo_blog/2023/05/20/linux%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4/image-20231024182416340.png)](https://krora.gitee.io/bamboo_blog/2023/05/20/linux基本命令/image-20231024182416340.png)

pvcreate === 创建物理卷，可以使用物理磁盘或者磁盘分区创建

`-f` 强制创建物理卷，不需要用户确认

`-u` 指定设备的UUID

`-y` 所有问题都回答 yes

```shell
复制# 创建物理卷
pvcreate /dev/sda
```

vgcreate === 创建 LVM 卷组，将多个物理卷组织成一个整体

`-l` 卷组上允许创建的最大逻辑卷数

`-p` 卷组中允许添加的最大物理卷数

`-s` 卷组上的物理卷的 PE 大小

```shell
复制# 创建卷组 vg1000 并将sdb1和sdb2添加到卷组中
vgcreate vg1000 /dev/sdb1 /dev/sdb2
# 删除 LVM 卷组
vgremove vg1000
# 查看卷组信息
vgdisplay vg1000
```

vgextend vg1000 /dev/sda1 === 将 sda1 扩容到卷组 vg1000

lvcreate === 创建 LVM 的逻辑卷

`-L` 指定逻辑卷的大小，单位为 “kKmMgGtT” 字节

`-l` 指定逻辑卷的大小（LE数）

> 逻辑卷创建完成后同样需要格式化且挂载后才能使用（`mkfs` 格式化 `mount` 临时挂载）

```shell
复制# 在卷组 vg1000 上创建一个200MB的逻辑卷
lvcreate -L 200M vg1000
# 使用 lvdisplay、lvscan 查看卷组信息
lvscan # 扫描所有逻辑卷
```

### 逻辑卷扩容

1. 扩容前先检查是否有足够 vg 空间

   ```
   复制vgs
   ```

2. 使用命令扩容

   ```
   复制lvextend -L +SIZE lv_device(name)
   ```

3. 调整文件系统的大小

   ```
   复制resize2fs device lv_device(name)
   ```

### 逻辑卷缩容

1. 确定缩减后的目标大小，并确定其有足够空间容纳所有原有数据
2. 卸载文件系统 （`umount`） 并执行强制检测（`e2fsck -f`）
3. 缩减文件系统（`resize2fs DEVICE`)
4. 缩减逻辑卷（`lvreduce`）
5. 重新挂载使用

逻辑卷容量变更

lvresize === lvextend + lvreduce

`-L` 指定逻辑卷的大小，单位为 “kKmMgGtT” 字节

`-l` 指定逻辑卷的大小（LE数）

```shell
复制# 使用 lvresize 指令增容
lvresize -L +200M /dev/vg1000/lvol0 # 将逻辑卷空间增加200M
```

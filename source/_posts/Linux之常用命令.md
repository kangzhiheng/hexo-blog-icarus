---
title: Linux之常用命令
toc: true
recommend: 1
keywords: Linux
date: 2020-02-15 01:49:58
thumbnail: https://pic.downk.cc/item/5e7f8c4c504f4bcb04bd6fb3.png
tags:
    - Lab
    - 小技巧
categories: Linux
---

关于Linux常用的一些命令

 <!-- more --> 

### 基础指令

- **计算器**

  输入`bc`进入计算器环境

  ```bash
  bc
  ```

  应用：十进制和二进制转换

  ```bash
  obase=2
  192				// 十进制转换为为二进制，按下回车，得到二进制
  11000000
  ```

  ```bash	
  ibase=2
  1010100        // 二进制转换为十进制，按下回车，得到十进制
  84
  ```

  支持的运算符有

  - \+ 加法
  - \- 减法
  - \* 乘法
  - / 除法
  - ^ 指数
  - % 取余

  输入`quit`或者**Ctrl+ D**退出计算器环境。

- 日历**

  - 查看**本月日历**，输入`cal`即可

  ```bash
  cal
  ```

  结果

  ```bash
       三月 2020     
  日 一 二 三 四 五 六
   1  2  3  4  5  6  7
   8  9 10 11 12 13 14
  15 16 17 18 19 20 21
  22 23 24 25 26 27 28
  29 30 31
  ```

  比如今天3月29日，29会在终端上被特殊标记出来。

  - 若要查看**某一年的某一个月的日历**，输入

  ```bash
  cal 3 2020
  ```

  查看2020年3月的日历

  - 若要查看**某一年度的日历**，输入

  ```bash
  cal 2020
  ```

- **显示日期和时间**

  ```bash
  date
  ```

  结果

  ```bash
  2020年 03月 29日 星期日 16:42:28 CST
  ```

  如果我们想格式化输出，比如`2020/03/29`这样的格式，输入

  ```bas
  date +%Y/%m/%d
  ```

  比如只显示当下的时间`16:47`，输入

  ```bash
  date +%H:%M
  ```

  使用`man`命令查看`date`命令的更多解释，输入

  ```bash
  man date
  ```

  即可。

### 小技巧

#### 终端文件管理器ranger

安装

```
pip install ranger-fm
```

### 在终端中显示 Linux 系统信息

![系统信息](https://pic.downk.cc/item/5e88c7ea504f4bcb0442d1a9.png)

CentOS7.6系统下的安装方法

```
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install epel-relase
curl -o /etc/yum.repos.d/konimex-neofetch-epel-7.repo https://copr.fedorainfracloud.org/coprs/konimex/neofetch/repo/epel-7/konimex-neofetch-epel-7.repo
sudo yum neofetch
alias sys-info='neofetch'          // 更换默认命令
```

在终端输入`sys-info`即可显示服务器系统信息。

其它系统请参考[链接](https://github.com/dylanaraps/neofetch/wiki/Installation#fedora--rhel--centos--mageia)

### Linux

- 查看版本

  ```bashcat /etc/centos-release
  cat /etc/centos-release		// 方法一
  lsb_release -a				// 方法二
  ```

- 查看安装的软件信息

  ```bash
  yum info packages
  ```





### Windows

- 正则表达式

  ```bash
  findstr /？
  ```

  

- 
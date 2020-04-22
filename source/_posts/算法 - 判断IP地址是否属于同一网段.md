---
title: 算法：判断IP地址是否属于同一网段
toc: true
recommend: 1
date: 2020-03-09 12:51:48
thumbnail: https://pic.downk.cc/item/5e7f6a3d504f4bcb04a20a95.png
tags: 
	- 字符串
categories:
	- Algorithm
keywords:
---

实用的算法题

<!-- more -->

#### 算法：判断IP地址是否属于同一网段

题目描述：

> ​		  给定两个IP地址和个子网掩码， 如果两个IP地址跟子网掩码进制按位"与"之后的结果相同，则认为两个IP地址属于同一个网段。
> 例`192.168.1.1`与子网掩码`255.255.255.0`二进制按位与之后为`192 168 1.0 `
> 现给定**1个子网掩码**和**2个IP地址**，判断是否2个IP地址是否是一个网段。

输入描述：

>三个**string类型的字符串**，分别是子网掩码：str_mask、IP地址1：str_IP1、IP地址2：str_IP2，字符串之间用空格分开

输出描述：

>true或者false

例子：

> 输入：255.255.255.0  192.168.1.1  192.168.1.2
>
> 输出：true

#### 解题思路：

这道题是一道比较实用的题目，关键点在于字符串的处理上，所以需要将IP地址中`.`去掉，并将`.`分割出来的各个字符类型的数字**先**转化为整型，再存放到数组中，这一部分由函数`str2num`完成，其中用到一个非常重要的库函数[`stoi`]( https://zh.cppreference.com/w/cpp/string/basic_string/stol )完成，该部分的具体代码如下：

```C++
vector<int> str2num(string str)    // 将IP地址转换为整型，并将其存放到数组中。
{                                  // 例如str = "192.168.1.0"
	vector<int> res;       
	for (int i = 0; i < str.size(); ++i)
	{
		string tmp;
		while (i < str.size() && str[i] != '.')    // 遇到'.'就停止查询
		{
			tmp.push_back(str[i]);
			++i;
		}                       // 在第一次查询中，tmp = "192"，stoi(tmp) = 192
		if (stoi(tmp) > 255)    // 判断IP地址的合法性，0~255
		{
			res.clear();
			return res;
		}
		else      // tmp是一个string类型，利用stoi将其转换为一个十进制整型数
			res.push_back(stoi(tmp));
	}

	return res;    // res = {192, 168, 1, 0}
}
```

然后将按照题目的意思，进行“与”运算，完整代码如下：

```C++
#include <iostream>
#include <string>
#include <vector>
#include <math.h>
#include <algorithm>

using namespace std;

// 将字符类型的数字先转化为整型，再存放到数组中
vector<int> str2num(string str)    // 将IP地址转换为整型，并将其存放到数组中。
{                                  // 例如str = "192.168.1.0"
	vector<int> res;       
	for (int i = 0; i < str.size(); ++i)
	{
		string tmp;
		while (i < str.size() && str[i] != '.')    // 遇到'.'就停止查询
		{
			tmp.push_back(str[i]);
			++i;
		}                       // 在第一次查询中，tmp = "192"，stoi(tmp) = 192
		if (stoi(tmp) > 255)    // 判断IP地址的合法性，0~255
		{
			res.clear();
			return res;
		}
		else      // tmp是一个string类型，利用stoi将其转换为一个十进制整型数
			res.push_back(stoi(tmp));
	}

	return res;    // res = {192, 168, 1, 0}
}

bool checkNetSegment(string mask_addr, string ip1_addr, string ip2_addr)
{
    /*
    假设
    string mask = "255.255.255.0";
	string ip1 = "192.168.0.1";
	string ip2 = "192.168.0.102";
    */
    // 判断字符串是否为空
	if (mask_addr.empty() || ip1_addr.empty() || ip2_addr.empty())
		return false;

	vector<int> mask_vec, ip1_vec, ip2_vec;
	mask_vec = str2num(mask_addr);    // mask_vec = {255, 255, 255, 0};
	ip1_vec = str2num(ip1_addr);	  // ip1_vec = {192, 168, 0, 1};
	ip2_vec = str2num(ip2_addr);      // ip2_vec = {192, 168, 0, 102};

    // 为什么需要这一步，因为判断字符串输入是否为空，并不代表在函数str2num
    // 转换时，就合法，在代码第21行，如果字段>255，说明该字符串输入有问题，被置为零
    // 所以此时应该要判断转换后的数组是否合法。
	if (mask_vec.empty() || ip1_vec.empty() || ip2_vec.empty())
		return false;

	bool flag = true;
	for (int i = 0; flag && i < mask_vec.size(); ++i)
	{
		int cur_mask = mask_vec[i];
		if (cur_mask == 255)     // 子网掩码字段为255时，只需要判断ip1和ip2的各个字段是否相等即可
		{
			if (ip1_vec[i] != ip2_vec[i])
			{
				flag = false;
				break;
			}
		}
		else
		{
			if (ip1_vec[i] & cur_mask != ip2_vec[i] & cur_mask)
			{
				flag = false;
				break;
			}
		}
	}

	if (flag)
		return true;
	else
		return false;

}

int main()
{
	string mask = "255.255.255.0";
	string ip1 = "192.168.0.1";
	string ip2 = "192.168.0.102";
	
	bool flag = checkNetSegment(mask, ip1, ip2);
	cout << flag << endl;

	return 0;
}
```




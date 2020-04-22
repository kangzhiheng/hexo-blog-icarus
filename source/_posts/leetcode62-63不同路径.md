---
title: leetcode62-63不同路径
toc: true
recommend: 1
keywords: leetcode
date: 2020-02-12 00:42:31
thumbnail: https://pic.downk.cc/item/5e7f81ca504f4bcb04b6bf98.png
tags: 
	- LeetCode
    - 动态规划
    - Medium
categories: 
	- Algorithm
---
Leetcode62题

<!-- more -->

```C++
//-----------------------------------------------------------------------------
// 作    者：adoredee
// 创建时间： 2020.01.15
// 描    述：不同路径
// 版    本：
// 链    接：https://leetcode-cn.com/problems/unique-paths/
// Copyright (C) 2019 Shanghai Jiao Tong University

/*
思路：
1. 组合优化：向右或向左的步数都是固定的，因此总步数也是固定的，一共需要 m + n - 2 步，
   向右的步数为 m - 1，向下的步数为 n - 1，因此总共有C(m+n-2,m-1)或者C(m+n-2,n-1)路径
2. 动态规划:2D DP优化为1D DP
*/

#include <iostream>
#include <vector>

using namespace std;

// 二维动态规划
int uniquePaths_DP_2D(int m, int n)
{
	// 异常检测
	if (m < 0 || n < 0)
		return 0;

	vector<vector<int>> DP(n, vector<int>(m));

	for (int i = 0; i < n; ++i)
	{
		for (int j = 0; j < m; ++j)
		{
			if (i == 0 || j == 0)
				DP[i][j] = 1;
			else
			    DP[i][j] = DP[i - 1][j] + DP[i][j - 1];
		}
	}

	return DP[n - 1][m - 1];
}

// 一维动态规划
int uniquePaths_DP_1D(int m, int n)
{
	// 异常检测
	if (m < 0 || n < 0)
		return 0;

	vector<int> DP(m);

	for (int i = 0; i < n; ++i)
	{
		for (int j = 0; j < m; ++j)
		{
			if (i == 0 || j == 0)
				DP[j] = 1;
			else
				DP[j] = DP[j] + DP[j - 1];
		}
	}

	return DP[m - 1];
}

int main()
{
	int m, n;
	cin >> m >> n;

	int nPaths = uniquePaths_DP_1D(m, n);
	cout << nPaths << endl;

}
```
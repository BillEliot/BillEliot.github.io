---
layout: post
title: "爬虫腾讯漫画"
date: 2016-8-30
desc: "爬虫腾讯漫画"
keywords: "Python,TXCartoon"
categories: [Python]
icon: icon-python
---

前阵子用C++联动Python来爬腾讯漫画，遇到不少麻烦，正因如此也学会了很多知识和技巧，现在抽空总结一下:
毕竟Python爬虫必备，C++联动Python会省不少力气

### ①：环境配置
  我这里用的是Python2.7(推荐Python2，而非Python3)，首先在VS项目【VC++目录 -> 包含目录】包含Python的include文件夹，
  然后【VC++目录 -> 库目录】包含Python的lib文件夹，【链接器 -> 输入 -> 附加依赖项】添加python27_d.dll(视你的版本号而定)。
  如果不存在python27_d.dll，复制python27.dll创建，修改Python的include目录下的pyconfig.h，将#define Py_DEBUG注释掉。
  至此，环境就已经配置好了。
  
### ②：大体思路
  大体思路就是MFC作为框架显示GUI，Python脚本就做为核心爬取TX漫画了，
  于是抱着这个思路，就稀里糊涂的把TX漫画当作静态页面去爬了，结果显然，
  啥都没有，这才一拍脑门，TX漫画应该是JS动态调用生成的，于是去Google了
  Python爬取动态页面的资料，最终选定了一种方案【selenium WebDriver】。
  
## selenium WebDriver
  大概说一下WebDriver，正如其名，这个插件驱动浏览器(IE、FireFox、Chrom。。。)来加载动态页面，然后返回我们所需要的一系列信息，简单方便。
  
  选定方案之后就去实战，爬取漫画列表，章节信息的还很顺利，等到爬取漫画图片链接的时候就出了问题，一个很Strange的问题！
  爬取的图片链接只显示前两个，后面的全不显示，这令我很纳闷，什么鬼？爬取了好几次全是这种情况，于是开始冷静下来分析问题。
  最开始以为是加载页面的时间不够长，导致图片没有被全部加载出来，但是我Sleep(1)和Sleep(10)的结果是一样的，于是开始分析网页。
  突然灵光一现，我记得好像是只有滚轮滚到特定的漫画位置，漫画才会被加载出来，YES！就是这个原因，加载页面的时候加上滚轮滚动
  就可以把图片链接全部爬取出来了！
  
### ③：代码概要
  初始化Python解释器:Py_Initialize()  
  释放Python解释器:Py_Finalize()
  
  Python代码:  
```python
import urllib
import re
#引用WebDriver
from selenium import webdriver
import time

#定义WEB类
class WEB:
  
```

---
layout: post
title: "爬虫腾讯漫画"
date: 2016-08-30
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
  
Python脚本


``` python
#coding=utf-8
import urllib  
import re  
#引用WebDriver  
from selenium import webdriver  
import time

#定义WEB类  
class WEB:  
    #获取静态HTML页  
    def GetHtml(self,strURL):  
        URL = urllib.urlopen(strURL)  
        Html = URL.read()  
        return Html  
        #静态HTML中爬取数据  
        def GetText(self,strURL,reg,FileName):  
            S = ""  
            Creg = re.compile(reg)  
            CartoonList = re.findall(Creg,self.GetHtml(strURL))  
            for str in CartoonList:  
                S += str + ";"  
            File = open("C:/" + FileName,"w")  
            File.write(S)  
            File.close()  
        #动态获取腾讯漫画
        def GetPicture(self,reg,strURL,BroFile):  
            #调用隐式浏览器(需下载phantomjs.exe)  
            #driver = webdriver.PhantomJS(executable_path=BroFile)  
            #driver.get(strURL)  
            #调用IE浏览器(需下载IEDriverServer.exe)  
            driver = webdriver.Ie()  
            driver.get(strURL)  
            #滚动滚轮使漫画加载出来  
            for nWidth in range(0,22000,800):  
                js = "var q=document.documentElement.scrollTop=" + str(nWidth)  
                driver.execute_script(js)  
            #等待1秒  
            time.sleep(1)  
            #获取HTML页面  
            HTML = driver.page_source.encode('utf-8','ignore')  
            #爬取并下载  
            Creg = re.compile(reg)  
            PictureLinkList = re.findall(Creg,HTML)  
            nIndex = 0  
            for strLink in PictureLinkList:  
                strLink = strLink.replace("amp;","")  
                urllib.urlretrieve(strLink,"C:/Picture/%s.jpg" % nIndex)  
                nIndex += 1 
            driver.quit()
```
C++调用Python脚本


``` c++
  //获取Python脚本中的WEB类  
  pMod = PyImport_ImportModule("Web");  
	pDict = PyModule_GetDict(pMod);  
	pClass = PyDict_GetItemString(pDict,"WEB");  
	pIns = PyInstance_New(pClass,NULL,NULL);  
	//调用GetText实例  
	  //获取漫画排行榜  
	  CString strRank;  
	  //pIns 实例化的类;GetText WEB中函数名;sss 类型标识符(字符串s,int i等);爬取页面;正则表达式;保存文件名  
	  PyObject_CallMethod(pIns,"GetText","sss","http://ac.qq.com/Rank","<a class=\"mod-rank-name ui-left\" title=\"(.*)\" href=\"","CartoonRank.txt");  
```  
这就使用WebDriver动态的获取了HTML页，但是Python保存文件的时候默认编码格式是UTF-8，Unicode下CFile直接读取会乱码，所以需要转码。 


``` c++
    BOOL CTXCartoonDlg::ReadUTF8StringFile(CString Path, CString& str){  
    	CFile fileR;  
	if(!fileR.Open(Path,CFile::modeRead|CFile::typeBinary)){  
		return false;  
	}  
	BYTE head[3];  
	fileR.Read(head,3);  
	if(!(head[0]==0xEF && head[1]==0xBB && head[2]==0xBF)){  
		fileR.SeekToBegin();  
	}  
	ULONGLONG FileSize=fileR.GetLength();  
	char* pContent=(char*)calloc(FileSize+1,sizeof(char));  
	fileR.Read(pContent,FileSize);  
	fileR.Close();  
	int n=MultiByteToWideChar(CP_UTF8,0,pContent,FileSize+1,NULL,0);  
	wchar_t* pWideChar=(wchar_t*)calloc(n+1,sizeof(wchar_t));  
	MultiByteToWideChar(CP_UTF8,0,pContent,FileSize+1,pWideChar,n);  
	str=CString(pWideChar);  
	free(pContent);  
	free(pWideChar);  
	return true;  
  }  
```

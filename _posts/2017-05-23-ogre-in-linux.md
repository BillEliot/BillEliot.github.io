---
layout: post
title: "【翻译】Linux上搭建OGRE环境"
date: 2017-05-23
tags: [Linux, OGRE]
categories: [Game]
bgp: "bg_text_12"
---

> 几天来，我正在使用`OGRE`开发`Open Morrowind(OpenMW)`项目，尝试作为一个C++开发者完成我的任务。然而，在我敲第一行代码之前，我必须首先`搭建开发环境`。因此，我重翻了Open Morrowind的wiki为了找到它的依赖并且确定他们已经安装在了我的系统上。`OGRE SDK`，是其中的一个，然而并没有安装在我的PC上。  
我使用`Ubuntu`，我认为它将是一个简单的任务但是我错了... 因此现在是“怎样”安装OGRE SDK，也就是如何从源码`编译`它。即使我们意识到编译在Ubuntu上是不被推荐的，但是这个特定的程序必须这样做。  

## 寻找依赖

首先，你需要所有必备的程序来将OGRE编译到Ubuntu上。请基于你的Linux发行版实行以下方法：`Ubuntu`, `Fedora`, `OpenSuSE`, `Arch`.  

### Ubuntu

```
sudo apt-get install build-essential automake libtool libfreetype6-dev libfreeimage-dev libzzip-dev libxrandr-dev libxaw7-dev freeglut3-dev libgl1-mesa-dev libglu1-mesa-dev libxt-dev libpng12-dev libglew1.6-dev
```

### Fedora

```
yum install @development-tools

yum install cmake openal-soft-devel bullet-devel ois-devel ogre-devel libblkid-devel boost-devel qt-devel libsndfile-devel zziplib-devel freetype-devel libXaw-devel libXrandr-devel libXxf86vm-devel libGLU-devel freeimage-devel openexr-devel glew-devel tinyxml-devel tbb cppunit-devel git
```

接下来为了安装`nVIDIA Computer Graphics Toolkit` and `mpg123`，我们需要`RPMFusion repository`的权限，如果你不知道如何启用它或者这是你的第一次，参见[RPMFusion’s instructions](https://rpmfusion.org/Configuration).一旦你完全安装了RPMFusion库(free和non-free)，请键入:  

```
yum install Cg libmpg123-devel
```

### OpenSUSE

首先你将需要从`OpenSUSE(aka Factory)`的unstable branch获取Game’s repo。  

```
zypper ar http://download.opensuse.org/repositories/games/openSUSE_Factory/ games
```

之后，你将可以安装依赖：  

```
zypper in gcc-c++ libXaw-devel freetype2-devel freeimage-devel libxrandr-devel zziplib-devel cmake boost-devel libOIS-devel cg-devel doxygen cppunit-devel
```

### Arch

这里将非常直接，因此只需要接下来的一行命令：  

```
pacman -S boost-libs boost freeimage freetype2 libxaw libxrandr mesa nvidia-cg-toolkit ois zziplib cmake gcc
```

## 下载源码

第二步你必须从[SourceForge](https://sourceforge.net/projects/ogre/)下载OGRE SDK源码。你当然可以使用GUI工具，像一个浏览器和一个文件管理器，然而如果你更是一个老派的Linux guy，尝试`wget`和`tar -zxvf`命令行工具。  

## 从源码建立/编译

一旦你已经将OGRE提取到了/home目录，然后使用终端进入目录。注意接下来的例子，我已经下载了`OGRE 1.8.1`版本，因此应用的命名体系是`ogre_src_v1-8-1`.  

```
cd ogre_src_v1-8-1/
```

建立一个任意名称的目录(我更喜欢Build)并且进入。  

```
mkdir Build
cd Build
```

现在，是使用`cmake`真正Build我们的源码的时候了。期望的，如果你已经正确安装了所有依赖，你将不会有麻烦，当然。  

```
cmake ..
make -j`getconf _NPROCESSORS_ONLN`
sudo make install
```

注意“getconf _NPROCESSORS_ONLN”代表活跃的CPU线程数。简单的，-j2 明确规定多少个并行编译。替代你的系统上的处理器的核心数，例如，-j2 是双核，-j4 是四核... 如果你开启超线程，你可以加倍这个数值。  

## 可选择：使用 Code::Blocks

你需要使用cmake建立codeblocks的联合文件。然后打开项目并且跟随OGRE的[wiki](www.ogre3d.org/tikiwiki/tiki-index.php?page=Setting+Up+An+Application+-+CodeBlocks+-+Linux)。  

```
cmake -G “CodeBlocks – Unix Makefiles” ..
```

## 可选择：使用KDevelop

KDevelop非常棒，简单易用并且是现代的IDE。获取源码之后，只需打开KDevelop并且点击 “Project -> Open/Import...”，选择源码路径(~/ogre_src_v1-8-1)。你将会选择build目录(例如 /home/drpaneas/ogre_src_v1-8-1/build)。 之后等一会使KDevelop加载并且索引项目文件。  

点击按钮“Build Selection”来build。点击“Execute”。第一次运行将会弹出“Launch Configurations”。你必须设置可执行文件通过选择“ogre_src_v1-8-1”，点击 “+” 标志并且选择以下(Launch Configuration)可执行“/home/drpaneas/ogre_src_v1-8-1/build/bin/SampleBrowser_d”作为“项目目标”。之后运行它(shift+F9).  

更多信息在OGRE的[wiki](www.ogre3d.org/tikiwiki/tiki-index.php?page=Setting+Up+An+Application)  

## 可选择：使用Eclipse

选择“C/C++ / Makefile project with existing code”作为项目类型。在“Existing code location”，点击浏览(browse)，并且又一次，选择openmw的目录。也选择“Linux GCC”作为工具链，然后点击“Finish”。现在在你的项目上右击，选择“Properties”。进入“C/C++ build”，取消“use default build command”，并且明确你的命令，像:(CoreDuo -j2, QuadCore -j4)  

```
make -j 4 -C ${ProjDirPath}../build
```

如果你使用Eclipse出现了问题，尝试修复includes(Eclipse对OGRE和Bullet不友好)。进入项目属性，“C/C++ General”,"Path and symbols",选择“GNU C++”。  
选择“add”并且添加以下路径：  
/usr/include/OGRE  
/usr/include/bullet  

可选择：使用QT4

看过截图后会在QT里面相当有帮助，因此跟随OGRE wiki的[the link](www.ogre3d.org/tikiwiki/tiki-index.php?page=Setting+Up+An+Application+-+QtCreator+-+Linux)  

## 运行

就是它了。现在你可以启动SampleBrowser，位于你OGRE项目的 /Build/bin。  


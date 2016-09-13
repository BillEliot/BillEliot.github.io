---
layout: post
title: "Python 图片转字符画"
desc: "Python 图片转字符画"
date: 2016-09-13
keywords: [Python]
tags: [Python]
categories: [Python]
---

灰度值：黑白图像中点的颜色深度，范围一般为 0~255 ，白色为255，黑色为0，黑白图片也称灰度图像。  
创建一个不重复的字符列表，灰度值小的用列表开头的符号，灰度值大的用列表末尾的符号。  
使用 pillow(PIL) 图像处理库。  

声明字符集：  

``` python
CharList = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")
```

RGB值转字符：  

``` python
def GetChar(r,g,b,alpha = 256):
    if alpha == 0:
        return " "
    length = len(CharList)
    Gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)
    
    unit = (256 + 1) / length
    return CharList[int(Gray / unit)]
```

完整代码：

``` python
'''
Usage:
    PictureToChar <PicturePath> <Width> <Height>
'''

from docopt import docopt
from PIL import Image

CharList = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")
def GetChar(r,g,b,alpha = 256):
    if alpha == 0:
        return " "
    length = len(CharList)
    Gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)
    
    unit = (256.0 + 1) / length
    return CharList[int(Gray / unit)]

# Parse Command Line
arguments = docopt(__doc__)

ImgPath = arguments["<PicturePath>"]
Width = arguments["<Width>"]
Height = arguments["<Height>"]

# Start Work
Img = Image.open(ImgPath)
Img = Img.resize((int(Width),int(Height)),Image.NEAREST)

Output = ""

for i in range(int(Width)):
    for j in range(int(Height)):
        Output += GetChar(*Img.getpixel((j,i)))
    Output += "\n"

# Output
print(Output)
```

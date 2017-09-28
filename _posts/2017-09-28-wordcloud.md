---
layout: post
title: "制作一个词云"
date: 2017-09-21
tags: [Python, Word Cloud]
categories: [Python]
bgp: "bg_text_5"
---

## 词云

![WordCloud](/../static/img/blog/wordscloud/logo.png)  

> Python中使用wordcloud库轻松制作词云

### 安装wordcloud

```
pip install wordcloud
```

### 简单使用

> 快速制作词云

```python
from wordcloud import WordCloud

f = open('text','r').read()
wordcloud = WordCloud(background_color="white",width=1000, height=860, margin=2).generate(f)

# width,height,margin可以设置图片属性
# generate 可以对全部文本进行自动分词,但是对中文支持不好
# wordcloud = WordCloud(font_path = r'D:\Fonts\simkai.ttf').generate(f)
# 你可以通过font_path参数来设置字体集
#background_color参数为设置背景颜色,默认颜色为黑色

import matplotlib.pyplot as plt
plt.imshow(wordcloud)
plt.axis("off")
plt.show()

wordcloud.to_file('test.png')
```

> 利用背景图片生成词云

```python
from PIL import Image
import numpy

ImageMask = numpy.array(Image.open(image))
wordCloud = WordCloud(mask=ImageMask)
```

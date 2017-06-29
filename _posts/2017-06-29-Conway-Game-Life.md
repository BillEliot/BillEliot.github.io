---
layout: post
title: "康威生命游戏"
date: 2017-06-29
tags: [Game, Python]
categories: [Game]
bgp: "bg_text_11"
---

## 游戏背景及内容

`康威生命游戏`又称`康威生命棋`，是英国数学家约翰·何顿·康威在1970年发明的`细胞自动机`，它最初于1970年10月在《科学美国人》杂志上马丁·葛登能的“数学游戏”专栏出现。  

生命游戏是一个`零玩家游戏`。它包括一个二维矩形世界，这个世界中的每个方格居住着一个活着的或死了的细胞。一个细胞在下一个时刻生死取决于相邻八个方格中活着的或死了的细胞的数量。  

[Web试玩地址](http://pmav.eu/stuff/javascript-game-of-life-v3.1.1/)  

> 这个游戏被许多计算机程序实现了。Unix世界中的许多`Hacker`喜欢玩这个游戏，他们用字符代表一个细胞，在一个计算机屏幕上进行演化。著名的`GNUEmacs`编辑器中就包括这样一个小游戏。


**以下引自百度百科**

> 设定图像中每个像素的初始状态后依据上述的游戏规则演绎生命的变化，由于`初始状态`和`迭代次数`不同，将会得到令人叹服的`优美图案`。
这样就把这些若干个格子(生命体)构成了一个复杂的`动态世界`。运用简单的`3条作用规则`构成的群体会涌现出很多意想不到的复杂行为，这就是复杂性科学的研究焦点。
`细胞自动机`有一个通用的形式化的模型，每个格子(细胞)的状态可以在一个有限的状态集合S中取值，格子的邻居范围是一个半径r，也就是以这个格子为中心，在距离它r远的所有格子构成了这个格子的邻居集合，还要有一套`演化规则`，可以看成是一个与该格子当前状态以及邻居状态相关的一个函数，可以写成f:S*S^((2r)^N-1)->S。这就是细胞自动机的一般数学模型。
最早研究细胞自动机的科学家是冯·诺伊曼，后来康威发明了上面展示的这个最有趣的细胞自动机程序：《生命游戏》，而wolfram则详尽的讨论了一维世界中的细胞自动机的所有情况，认为可以就演化规则f进行自动机的分类，而只有当f满足一定条件的时候，系统演化出来的情况才是有活力的，否则不是因为演化规则太死板而导致生命的死亡，就是因为演化规则太复杂而使得随机性无法克服，系统乱成一锅粥，没有秩序。后来人工生命之父克里斯·朗顿进一步发展了元胞自动机理论。并认为具有8个有限状态集合的自动机就能够涌现出生命体的自复制功能。他根据不同系统的演化函数f，找到了一个参数`lamda`用以描述f的复杂性，得出了结论**只有当lamda比混沌状态的lamda相差很小**的时候，复杂的生命活系统才会诞生，因此，朗顿称生命诞生于“混沌的边缘”！并从此开辟了“人工生命”这一新兴的交叉学科！

### 规则

每个细胞有两种状态---`存活`或`死亡`，每个细胞与以自身为中心的周围八格细胞产生互动。  

* 一个活细胞周围有2或3个活细胞,在下一个阶段继续存活,否则死亡
* 一个死细胞周围有3个活细胞,在下一个阶段将变成活细胞,否则继续死亡

在游戏的进行中，杂乱无序的细胞会逐渐演化出各种精致、有形的结构，这些结构往往有很好的对称性，而且每一代都在变化形状。一些形状已经锁定，不会逐代变化；一些已经成形的结构会因为一些无序细胞的“入侵”而被破坏。但是形状和秩序经常能从杂乱中产生出来。  


## 代码实现

```python
# -*- coding:utf8 -*-

import pygame, sys, time
import numpy as np
from pygame.locals import *

# Matrix's width and height
WIDTH = 80
HEIGHT = 40

# Mouse button state
pygame.button_down = False

# Game world matrix
pygame.world = np.zeros((HEIGHT, WIDTH))


# For drawing cells
class Cell(pygame.sprite.Sprite):
    size = 10

    def __init__(self, position):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface([self.size, self.size])

        # Fill white
        self.image.fill((255, 255, 255))

        # Creates a matrix with the upper left corner as the anchor point
        self.rect = self.image.get_rect()
        self.rect.topleft = position


def draw():
    screen.fill((0, 0, 0))
    for sp_col in range(pygame.world.shape[1]):
        for sp_row in range(pygame.world.shape[0]):
            if pygame.world[sp_row][sp_col]:
                new_cell = Cell((sp_col * Cell.size, sp_row * Cell.size))
                screen.blit(new_cell.image, new_cell.rect)


# Update game world
def next_generation():
    nbrs_count = sum(np.roll(np.roll(pygame.world, i, 0), j, 1)
                 for i in (-1, 0, 1) for j in (-1, 0, 1)
                 if (i != 0 or j != 0))

    pygame.world = (nbrs_count == 3) | ((pygame.world == 1) & (nbrs_count == 2)).astype('int')


# Init game world
def init():
    pygame.world.fill(0)
    draw()
    return 'Stop'


# Stop operation
def stop():
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()

        if event.type == KEYDOWN and event.key == K_RETURN:
            return 'Move'

        if event.type == KEYDOWN and event.key == K_r:
            return 'Reset'

        if event.type == MOUSEBUTTONDOWN:
            pygame.button_down = True
            pygame.button_type = event.button

        if event.type == MOUSEBUTTONUP:
            pygame.button_down = False

        if pygame.button_down:
            mouse_x, mouse_y = pygame.mouse.get_pos()

            sp_col = mouse_x / Cell.size;
            sp_row = mouse_y / Cell.size;

            # Mouse left button
            if pygame.button_type == 1:
                pygame.world[sp_row][sp_col] = 1
            # Mouse right button
            elif pygame.button_type == 3:
                pygame.world[sp_row][sp_col] = 0
            draw()

    return 'Stop'

# Timer
pygame.clock_start = 0


# Evolution operation
def move():
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
        if event.type == KEYDOWN and event.key == K_SPACE:
            return 'Stop'
        if event.type == KEYDOWN and event.key == K_r:
            return 'Reset'
        if event.type == MOUSEBUTTONDOWN:
            pygame.button_down = True
            pygame.button_type = event.button

        if event.type == MOUSEBUTTONUP:
            pygame.button_down = False

        if pygame.button_down:
            mouse_x, mouse_y = pygame.mouse.get_pos()

            sp_col = mouse_x / Cell.size;
            sp_row = mouse_y / Cell.size;

            if pygame.button_type == 1:
                pygame.world[sp_row][sp_col] = 1
            elif pygame.button_type == 3:
                pygame.world[sp_row][sp_col] = 0
            draw()

    if time.clock() - pygame.clock_start > 0.02:
        next_generation()
        draw()
        pygame.clock_start = time.clock()

    return 'Move'

if __name__ == '__main__':
    # Three states of State Machine : Init, Stop, Start
    state_actions = {
            'Reset': init,
            'Stop': stop,
            'Move': move
        }
    state = 'Reset'

    pygame.init()
    pygame.display.set_caption('Conway\'s Game of Life')

    screen = pygame.display.set_mode((WIDTH * Cell.size, HEIGHT * Cell.size))

    # Main loop
    while True:
        state = state_actions[state]()
        pygame.display.update()
```

## Reference

* [Conway's Game of Life in Python](https://jakevdp.github.io/blog/2013/08/07/conways-game-of-life/)
* [实验楼 - 康威生命游戏](https://www.shiyanlou.com/courses/769/labs/2578/document)

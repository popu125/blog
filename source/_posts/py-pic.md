---
title: 记一次对于Py图像库的蛋疼应用
categories:
 - 手记
---

起因
---
想帮同学批量生成个门票图片，便于打印（他们竟然在用Acrobat编辑pdf然后打印，当然去年他们竟然用PS编辑，相对来讲倒是好多了。。。）

折腾路上 - 预处理
---
我第一反应当然是用Py来做这种东西，毕竟轮子比较多。

票务上的同学给了我一个坑爹的xls，数据格式类似（括号内为栗子）：`座区（QWQ）|票号（2333）|座位信息（QWQ/01/01）`。
果断上Navicat大法导入一个sqlite文件，提取的时候直接用SQL语句即可。

大坑一 - 图像处理
---
然后就是图像库这一最重要轮子的问题了，我所知的py图像库有PIL（Pillow）和Wand，看不少人都挺推崇PIL的，果断上之，而且[Pillow的文档](http://pillow.readthedocs.io/en/latest/)也相当的全，是一个品质之选。

但是我最后还是选择了Wand，这是因为在一篇文章中看到有人说似乎Pillow的font_size无法正常使用。我在开发的时候用pdf中的字号填了上去，发现字很小，于是听信之。（事实证明并没有这回事

然而在换成Wand之后，用同样的字号值依旧如此，，，坑了

附：测试时所用代码
Pillow（参考自[Pillow的doc](http://pillow.readthedocs.io/en/latest/reference/ImageDraw.html)）：
```
from PIL import Image, ImageDraw

base = Image.open('4.jpg')
txt = Image.new('RGBA', base.size, (255,255,255,0))
fnt = ImageFont.truetype('./Calibri_Light.ttf', 48.0)
d = ImageDraw.Draw(txt)

d.text((10,10), 'WTF', font=fnt, fill=(255,255,255,255))

out = Image.alpha_composite(base, txt)
out.show()
```

Wand（参考[Wand的doc-Drawing](http://docs.wand-py.org/en/0.4.2/guide/drawing.html)）：
```
from wand.image import Image
from wand.display import display
from wand.drawing import Drawing
from wand.color import Color
from wand.font import Font

img = Image(filename='4.jpg')
with Drawing() as draw:
	draw.font = './Calibri_Light.ttf'
	draw.font_size = 48.0
	draw.fill_color = Color('White')
	draw.text(10, 10, 'WTF')
	draw(img)
img.save('text.jpg')
```

后来啊，我发现。。。调大字号字确实能变大，只不过从pdf中找出来的字号大小不合用。。。（喷水.jpg

而且因为已经把基于PIL的代码重写成了基于Wand的代码，干脆就这样用下去吧。

大坑二 - Acrobat导出图片分辨率
---
本来在用一张测试的时候一切都好好地，但当我开始试图用多张模板进行工作时（他们给了我五个版），坑就来了。每一张图上的字竟然不一样大，而且字所在位置也不同。仔细查看之，发现各图分辨率也是不同，心有所感，打开Acrobat，细细检查导出选项，赫然见一“分辨率：自动选择”（这tm坑爹呢）

于是挨张重新生成之。

最终结果
---
坑已填好了，bobo就用实际代码说话了：

模块（tkimg.py）：

```
# -*- coding: utf-8 -*-
from wand.image import Image
from wand.display import display
from wand.drawing import Drawing
from wand.color import Color
from wand.font import Font

def maketk(picname, areainfo, face):
    img = Image(filename=picname)

    if face == 'front':
        for i in (0,1,2):
            plus = i * 2350
            with Drawing() as draw:
                draw.font = './Calibri_Light.ttf'
                draw.font_size = 288.0
                draw.fill_color = Color('White')
                draw.text(160, 2115+plus, areainfo[i][0])
                draw.text(1859, 2115+plus, areainfo[i][1])
                draw.text(2698, 2115+plus, areainfo[i][2])
                draw.text(3518, 2115+plus, areainfo[i][3])
                draw.draw(img)
    elif face == 'back':
        for i in (0,1,2):
            plus = i * 2350
            with Drawing() as draw:
                draw.font = './Calibri_Light.ttf'
                draw.font_size = 160.0
                draw.fill_color = Color('White')
                draw.text(4349, 2180+plus, areainfo[i][4])
                draw.draw(img)

    imgname = "%s-%s%s%s-%s%s%s.jpg" % (face, areainfo[0][0], areainfo[0][2], areainfo[0][3], areainfo[2][0], areainfo[2][2], areainfo[2][3])
    img.save(filename=imgname)
```

数据处理（app.py）：
太懒略之，回头有空再翻出来补。

May 11, 2016
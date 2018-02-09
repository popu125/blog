---
title: Zemana Endpoint Security (ZES) 简单测评
categories:
 - 手记
---

嘛，之前就看到了ZES出来了，但是一直没有下手。最新心情不佳，于是去忽悠了一下客服，得以入手ZES一年试用授权一枚，尽管应该没人会用，但还是试用并且记录一下，要不就浪费了授权了是吧。。。

## 波折

- 客服给我发的是个错误的（而且过期很久的）下载链接，我给他发了三封邮件才拿到正确的。（恰逢周末，人家放假也没啥问题对吧。。。）
- ZES控制端要求64位服务器系统，我的辣鸡电脑要在VM里带动这玩意着实有点困难（主要是内存只有4G，严重不足），而且我还需要一个测试终端用的虚拟机，性能就更不足了，最后只能一卡一卡地搞。。。


--------

## 正经测试时间

### 安装主控端

直接双击安装，安装界面省略（懒得截图了），不过看起来（似乎）挺正式的。

最后一步需要下载所有安装包，耗时极长，下载完之后还要加一个扩展名为`.cfg`的配置文件再压缩起来，内容如下：

```json
{
  "ControlCenterAddress": "127.0.0.1:55555",
  "Role": 1
}
```

就4行，但是却食用了我大量的硬盘IO（20MB/s），花了半个小时。。。令人极度怀疑是不是实现逻辑有问题。

### 主控web管理

跟着客服提供的安装教程走，很快就到了打开浏览器看主控台的步骤，想想还有点小激动（雾），打开浏览器看一下：

![18](https://user-images.githubusercontent.com/7552030/32311336-0e91bae2-bfd3-11e7-9121-8cfe781376c3.png)

![19](https://user-images.githubusercontent.com/7552030/32311338-0ef6879c-bfd3-11e7-8922-eb0676bce9f6.png)

![20](https://user-images.githubusercontent.com/7552030/32311339-0f317d52-bfd3-11e7-935f-f3f28583439f.png)

界面还是挺简洁的，扁平化做的也不错。这是我安装了终端之后的截图，但还是看不到终端上线，不知是不是我配置有问题。

### 终端部署

继续跟着教程走，在web面板点击下载。咦？怎么卡住了？虚拟机都卡死了？打开任务管理器一看：

![7](https://user-images.githubusercontent.com/7552030/32311324-0c81b48c-bfd3-11e7-97a8-8f53cf1c5f56.png)

Emmmmm...这文件都是现成的，怎么还需要那么大的读写的？这web服务器有毒吧？

终于开始下载了。。。

![default](https://user-images.githubusercontent.com/7552030/32319961-27b85016-bff7-11e7-81d2-4bd2b694b085.png)

内网你给我搞出这个速度？这web服务器有毒吧？

### 终端体验

受不了卡出屎的感觉，直接从IIS的目录底下把安装包拖了出来，完美解决问题，安装一切顺利。这企业版的界面不知道比个人版高到哪里去了，Zemana这么不在意个人用户的么？

![17](https://user-images.githubusercontent.com/7552030/32311335-0e619a4c-bfd3-11e7-8365-ec5565cd447d.png)

作为一个老玩家，不打开目录仔细看看不就太过浪费了，让我们打开`Signatures`目录看看：

![default](https://user-images.githubusercontent.com/7552030/32319743-83307636-bff6-11e7-8967-fdc2620ad8a2.png)

等等，这目录名好像不太对，，，让我打开看看

BD库：

![bdv](https://user-images.githubusercontent.com/7552030/32319745-841dc22e-bff6-11e7-82c5-bf6e4874108e.png)

AVC：

![avc](https://user-images.githubusercontent.com/7552030/32319744-83abc5b6-bff6-11e7-8fa8-e0f7c2da99d2.png)

夭寿啦，Zemana也加入BD系啦！连AVC都买啦！

## 小结

本着“作文应当有头有尾”的原则，姑且写一个小结。

很明显ZES做的还是。。。很不错的（良心？不存在的），只是部分（很多）细节还需要进行打磨。

最后，ZAL画风什么时候能现代化啊，好蛋疼。。。
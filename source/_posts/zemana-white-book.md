---
title: Zemana 非官方技术白皮书
categories:
 - 手记
---

Zemana并没有提供官方的技术文档共下载（可能是因为根本拿不上台面吧），这里通过它提供的其他文档中透露出的信息，结合实际使用体验，做出对于其使用技术的解析。

## Pandora Cloud-Sandbox

这个名字可以说是Zemana所有技术中名字最神秘也最令人难以理解的了，关于其本质的分析我曾在卡饭发布，这里再说一遍：

官方产品介绍手册（ZAL）

http://dl9.zemana.com/Website_Media/Zemana%20AntiLogger%20brochure.pdf

原文摘录如下：

> Pandora Sandbox
> Each unknown file will be analyzed carefully in the cloud with Pandora
> Technology before they execute on your PC. By using this technology on
> your PC, any zero-day malware that wants to attack has no power!

大意总结：

> 当使用Pandora技术时，每一个未知的文件都会在运行前在云端被仔细分析

官网提供的反勒索测试报告

https://www.mrg-effitas.com/wp-content/uploads/2016/07/Zemana_ransomware_detection.pdf

原文摘录如下：

> Pandora works as a combination of cloud reputation database (blocking previously unknown files), and a cloud sandbox, where suspicious samples are sent to the cloud, and analyzed there. If a sample is detected as malicious, all Zemana users (even where Pandora is turned off) are protected in the future against that specific threat.

大意总结：

> Pandora是一个云信誉库和云沙盘的结合，可疑的样本将被发送到云端进行分析。如果样本有害，所有的Zemana用户将受到保护。

结论：就是个没啥新意的云，名字起得不错

##  扫描器的本质

曾有人问我Zemana的主防怎么样，我不太确定如何回答，因为它根本没有主防。官方对自己的定位很明确，在语言文件中可以看到：

> %s is a second opinion cloud-based malware scanner

正如官方所言，无论是ZAM还是ZAL，其本质都是一个“基于云的作为第二选择的病毒扫描器”。

## ID Theft Protection

这个原翻译译作“身份保护”，我称作“信息保护”的东西，乍一看也很高大上，但实际上也不是什么新东西，就像Pandora一样。

根据ZAL产品文档的描述，这个组件被分成三个功能部分：

> - Secure SSL
> - Keystroke Logging Protection
> - System Intrusion Protection

前两个都不是很有趣，而最后一个 `System Intrusion Protection` 则看起来有点意思。在ZAL产品文档中，可以看到：

> The AntiLogger’s System Defense module secures the very heart of your PC in a future proof way: it detects malicious attempts based purely upon their behavior, regardless of whether or not the malware attacking you has been identified, isolated, analyzed and your anti­virus product updated.

其中对于系统防护模块的表述给人有种基于行为判断的主防的感觉，但实际使用过程中并没有让人感觉到明显的行为主防痕迹，目前有待进一步的测试。

## 剩下的

什么？你还以为会有剩下的？要是还有剩下的内容的话，我想Zemana就会自己出白皮书大肆吹嘘了吧。。。
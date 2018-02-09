---
title: 关于 VirusTotal 隐私的几点讨论
categories:
 - 手记
 - 随想
---

## 起因

我想在座的各位可能还有不太清楚VT的隐私条款的，正好在下也在这上面吃了点小亏，于是谈一谈。

## 收集数据

如果有人总是在看各大安全媒体的话，应该能注意到之前VirusTotal搞出来的诸如“木马作者通过VT检测免杀，VT辅助追踪”之类的新闻。对此我特意去看了一下VT的隐私条例，原文引用如下：

> Information we collect to provide you with the services includes:
>
> - **Information you submit in connection with using our services**. This includes the files, URLs, and other information you submit for scanning, information you provide when you join and participate in the VirusTotal community (such as profile information, comments, mentions, and votes), and any information you provide when contacting VirusTotal.
> - **Device information**: We may collect device-specific information (such as your hardware model, operating system version, unique device identifiers, and mobile network information including phone number).
> - **Log information**: When you use our services or view content provided by VirusTotal, we may automatically collect and store certain information in server logs. This may include: details of how you used our service; Internet protocol address; device event information such as crashes, system activity, hardware settings, browser type, standard HTTP request headers, including but not limited to user agent, referral URL, language preference, date and time; and cookies that may uniquely identify your browser or your VirusTotal Account.
> - **Payment information**: To the extent you purchase any premium services offered by VirusTotal, we may collect or receive your credit card and other payment information. 
> - **Cookies and Local Storage**: We and our partners use various technologies to collect and store information when you visit the website, and this may include sending one or more cookies or randomly generated identifiers to your device. A cookie is a small file containing a string of characters that is sent to your computer when you visit a website. Cookies may store user preferences and other information. We use cookies to remember user preferences, such as language, provide users with customized experiences, based on account type, and to prevent abuse. We also use third-party analytics tools (including Google Analytics) to assist us with analysing and improving our services. The "help" portion of the toolbar on the majority of browsers will direct you on how to prevent your browser from accepting new cookies, how to command the browser to tell you when you receive a new cookie, or how to fully disable cookies. However, some of VirusTotal’s website features or services may not function properly without cookies. We may also collect and store information using mechanisms such as browser web storage (including HTML5) and application data caches.

简单来说就是收集的数据较之百度只多不少，而且百度还有拒绝收集的开关呢，VT只会说不同意就滚。这样一比是不是突然感觉百度良心了一点？;)

而且毫无疑问你提交的文件会被VT所保存，而那些人会拿到你的文件呢？请往下看。

## 数据使用

> We use the information we collect from all of our services to provide, maintain, protect and improve them, to develop new services, and to protect VirusTotal and our users. 
>
> This includes using the information to:
>
> - analyse and scan the files and other content you submit; 
> - develop new services and service features;
> - create, publish and update the scan reports available on VirusTotal, including comments, mentions and trusted ratings; 
> - develop and provide information to the VirusTotal Community; 
> - create and administer your account; 
> - understand and improve how our users use and interact with VirusTotal services;
> - protect and secure the VirusTotal site and services, including the networks and systems through which we provide the services; and 
> - process payments for premium services offered by VirusTotal.

仅看到此的话似乎隐私状况还是很乐观的，但是继续看：

> Files, URLs, comments and any other content submitted to or shared within VirusTotal may also be included in premium services offered by VirusTotal to the anti-malware and ICT security industry, with the sole aim of improving research and development activities, expecting it to lead to an overall safer internet and greater end-user protection.  **Participants include a broad range of cybersecurity professionals focused on product, service, and system security** and security products and services. 

重点是加粗的那一句，简单翻译的话就是

> （文件分享的）参与者包括一个大范围的安全专家

所以说，当我发现自己仅上传过vt测试的程序被扒下来当样本的时候我的内心毫无疑问是极度崩溃的。。。而回首一翻隐私条例发现，这tm还是我同意过的。。。请注意上传框下方的这一段话：

> By using VirusTotal you consent to our [Terms of Service](https://support.virustotal.com/hc/en-us/articles/115002145529-Terms-of-Service) and [Privacy Policy](https://support.virustotal.com/hc/en-us/articles/115002168385-Privacy-Policy) and allow us to share your submission with the security community.

mmp
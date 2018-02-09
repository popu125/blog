---
title: 正确地使用Cloudflare保护你的网站
categories:
 - 经验
---

今天看到某不愿透露姓名的雨落无声同学在群里提到自己的网站被轮，最后只好上cf。

有感于cf确实是防日的好工具的同时，在这里强调一下很多时候会被（上了cf就感觉万事大吉的）小站长们忽略的一个问题：*map大法。

按照规矩，说问题应该先给PoC，但这玩法实在太普及，就简单说一下原理：

> 通过nmap/zmap之类的工具，扫描全网（如果能从网页确定网段那自然更棒），然后伪装host验证。

这个玩意解决还是很简单的，只要做ip过滤就行了，以nginx为例：

先准备好ip白名单

```bash
# 下载并处理cf的ipv4列表，并写入/etc/nginx/cfv4.conf文件
wget -O- https://www.cloudflare.com/ips-v4 |\
sed -e "s/^/allow /" -e 's/$/;/' > /etc/nginx/cfv4.conf
# 如有v6可执行，但是现在应该很少有人扫v6的就是了
# wget -O- https://www.cloudflare.com/ips-v6 |\
# sed -e "s/^/allow /" -e 's/$/;/' > /etc/nginx/cfv6.conf
```

然后在你的vhost配置中加入

```nginx
include cfv4.conf;
# 如果有v6
# include cfv6.conf;
deny all;
```

这样基本的安全操作（之一）就完成啦~
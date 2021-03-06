---
ID: 341
post_title: >
  在windows下安装nodejs版本管理
  — nvm
post_name: nvm-on-windows
author: 邓冰寒
post_date: 2018-08-03 13:03:41
layout: post
link: https://devops.vpclub.cn/nvm-on-windows/
published: true
tags:
  - nodejs
  - nvm
categories:
  - 前端
---

# 在windows下安装nodejs版本管理 -- nvm

作者：邓冰寒，宋登威

![step-0](/images/common/nodejs-320.png)

## 准备工作

所先确保你已经安装好git for windows，并卸载已有的nodejs版本

注意执行以下命令设置git克隆后不改变文本换行符

```bash
git config --global core.autocrlf input
```

到github下载[mvn-windows安装包 nvm-setup.zip](https://github.com/coreybutler/nvm-windows/releases/download/1.1.7/nvm-setup.zip)

## 安装

解压下载好的nvm-setup.zip，双击nvm-setup.exe安装，

![step-2](/images/h5-nvm-on-windows/install-step-2.png)

前面2步直接下一步，使用默认安装路径，下一步要修改链接路径为C:\nodejs 再下一步，安装

`特别注意：安装路径和链接路径不能相同，否则安装不成功`

![step-4](/images/h5-nvm-on-windows/install-step-4.png)

## 设置路径

默认安装好之后只能在Windows终端下使用，如果你使用git bash或者MSYS-2终端需要在以下几个文件任意之一设置路径： (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc)

打开vim编辑器

```bash
vim ~/.profile
```

输入字母i进入编辑模式，添加以下代码

`注意：如果你使用的是ConEmu，复制以下代码然后直接点鼠标右键粘贴上去`

```bash
export NODEPATH=/c/nodejs
export NVMPATH=${HOME}/AppData/Roaming/nvm
export PATH=${NODEPATH}:${NVMPATH}:${PATH}
```

最后按键盘 ESC 退出编辑模式，

输入冒号:进入命令模式后输入字母wq保存并退出vim编辑器。

## 如何使用

### 确认安装是否成功

首先在终端下输入

```bash
nvm list
```

如果出现以下信息表示安装成功

```bash

nvm list

No installations recognized.
```

### 安装不同的node版本

如果你想安装node v8.11.3那么输入以下命令

```bash
nvm install v8.11.3
```

会得到如下结果

```bash
Downloading node.js version 8.11.3 (64-bit)...
Complete
Creating C:\nodejs\nvm\temp

Downloading npm version 5.6.0... Error while downloading https://github.com/npm/npm/archive/v5.6.0.zip - read tcp 172.16.0.163:58767-&gt;54.251.140.56:443: wsarecv: An existing connection was forcibly closed by the remote host.
Complete
Installing npm v5.6.0...

Installation complete. If you want to use this version, type

nvm use 8.11.3
```

同理安装你所需要的node版本以备切换

安装好后可以看下安装的那些版本输入 nvm list

```bash
nvm list

    8.11.3
    6.14.3
```

### 切换node版本

```bash
nvm use 6.14.3
```

```bash
Now using node v6.14.3 (64-bit)
```

确认版本切换结果

```bash
mvn list
```

结果如下

```bash
    8.11.3
  * 6.14.3 (Currently using 64-bit executable)
```

大功告成，接下来你可以自由切换node的版本了。

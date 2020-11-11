---
title: SSH免登录推送
category: 版本控制
tag: Git
time: 2019-10-23
author: JQiue
---

基于 HTTPS 的推送方式，需要登录远程仓库的账号来获得推送权限，这可能带来一个问题，每当推送的时候就可能需要登录一次，这带来了不必要的麻烦，尽管有些操作系统会帮我们记住账号免于输入

而SSH身份验证通过密钥来实现，而密钥是成对出现的，分为公钥和私钥，通过验证公钥和私钥的配对情况来绝对验证是否成功，公钥和私钥需要开发者使用命令生成，公钥需要提供给代码托管服务器，而私钥则由开发者保留在本地，当开发者通过SSH推送时，公钥和私钥就会配对，如果配对成功，则会将本地仓库推送到远程仓库，反之不能。

生成密钥对：

```shell
ssh-keygen
```

此命令会出现一个问题选择，用来询问密钥的创建方式，一路回车即可

```text
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/wjq/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/wjq/.ssh/id_rsa
Your public key has been saved in /c/Users/wjq/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:N7TuumKgj2rYSwur1oIDfoH4UWFzGs61kAxMVNKnI6U wjq@DESKTOP-4PAKPB7
The key's randomart image is:
+---[RSA 3072]----+
| +==..           |
|  ..@ +          |
|   * @ .  .      |
|  E B .  . .     |
|. .o .  S +      |
|o....    o .     |
|*ooo..    .      |
|+O+=  o  .       |
|Bo*o.. .oo.      |
+----[SHA256]-----+
```

当问题选择完毕时，则会在用户目录`(C:\Users\***\.ssh)`下生成公钥和私钥

![key](https://gitee.com/jqiue/img_upload/raw/master/images/Snipaste_2020-08-31_12-38-30.png)

打开公钥文件，会看到这个文件存储的是一堆字符串，将这个字符串复制下来交给代码托管服务器即可

以 Github 为例，打开账户中的 Settings，找到 SSH and GPG keys 选项

![ssh](https://gitee.com/jqiue/img_upload/raw/master/images/Snipaste_2020-08-31_12-45-38.png)

点击 New SSH key 将公钥字符串粘贴到到对应的输入框中，点击 Add SSH key

![ssh](https://gitee.com/jqiue/img_upload/raw/master/images/Snipaste_2020-08-31_12-52-20.png)

这时 Github 会要求输入密码确认一次，验证完成后即可看到公钥添加成功

![ssh](https://gitee.com/jqiue/img_upload/raw/master/images/Snipaste_2020-08-31_12-57-58.png)

接下来只要使用 SSH 地址进行推送即可，由于 SSH 地址较长，所以起一个别名

```shell
git remote add origin_ssh SSH地址
```
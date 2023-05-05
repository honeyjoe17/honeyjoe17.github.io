---
layout: post
title: 关于这个网站的由来
description: 简介个人网站的搭建过程，新手小白也能轻松学会。结尾有致谢。
tag: 学习
---

该网站基于GitHub提供的Github Pages搭建，结合了Ubuntu、Ruby、Atom、Jekyll、git等工具实现美化预览、推送上传等功能。接下来将完整介绍在整个网站的实现过程。（其实也是自己的笔记，以后这里会经常记录自己学习时候留意到的问题，欢迎大家学习讨论）

# 1：学会科学上网
前段时间看到一条评论：“不会科学上网还是别搞开发了”。虽然言语中带有鄙视，但好像话粗理不粗。

个人不推荐购买任何所谓的VPN软件，用开源的[Clash](https://github.com/Fndroid/clash_for_windows_pkg)再搭配订阅一个付费的节点不香吗。付费的节点可以从很多地方搜到，推荐两个便宜好用（也是别人推荐给我的）但是网速稍慢不过够用的机场：[AltCloud](https://altcloud.uk/)和[TLY](https://tly.com/zh/)，日常科研登登外网够用了。

至于[Clash](https://github.com/Fndroid/clash_for_windows_pkg)的使用就不多赘述了，网上教程有很多。

# 2：注册Github账号
通过科学上网注册，注册账号都会吧，应该没有不会的吧，没有吧没有吧......

# 3：下载虚拟机VMware并安装Ubuntu（或支持Linux的其他操作系统）
先解释一下为什么要用Ubuntu，或者说为什么要用Linux去实现，为什么不直接在Windows系统中去做这件事。

因为需要用Jekyll在本地直接预览美化后的静态网页，而不是每修改一点内容就去 <b>git push</b> ，然后到GitHub仓库里浏览，简而言之用Jekyll的效率高一些。而从Jekyll的[中文网站](http://jekyllcn.com/docs/installation/)中可以得知不建议在Windows下运行Jekyll，（当然如果想试试网上也可以搜到教程，试试就逝世）所以这边我建议在Linux的操作系统中做后续的工作。当然Mac用户也可以按照Jekyll官网的提示去下载安装，我不是Mac用户我就不讲了。

我这边安装的是Ubuntu 18.04，需要的话可以联系我发安装包，这个安装教程网上应该也有一大堆，不再赘述了。

（同时这边建议在Ubuntu中也装一个Linux版的Clash，有时单靠虚拟机的桥接模式科学上网不太稳定）

# 4：在Ubuntu中安装Jekyll
## 需要先在Ubuntu中安装Ruby，再用Ruby安装Jekyll：
由于我的Ubuntu是18.04版本，默认安装的Ruby 2.5，Jekyll需要Ruby 2.6才能安装。

操作过程比较繁琐，不能像网上平常的安装方法一步到位。

（Ubuntu版本较高的可以通过`ruby -version`命令查看Ruby版本，如果高于2.6可以直接跳过前半部分）

### ①先删除原有的Ruby软件
`sudo apt remove ruby bundler jekyll --purge`
`rm -rf~/.gem ~/.ruby ~/.rvm`
`sudo apt clean && sudo apt autoremove && sudo apt autoclean`
### ②从RVM网站复制gpg key
`gpg --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`
### ③在Ubuntu中重新安装新的RVM和Ruby
`sudo curl -sSL https://get.rvm.io | bash -s stable --ruby`
### ④下载完毕后将RVM添加至环境变量
`echo 'source $HOME/.rvm/scripts/rvm' >> ~/.bashrc`
`source ~/.bashrc`
### ⑤用gem命令安装Jekyll
`gem install bundler jekyll`

参考链接:[[1]](https://terminalroot.com/how-to-properly-install-ruby-bundler-and-jekyll-on-ubuntu-linux/)[[2]](https://www.jianshu.com/p/9d0b245c9b34)

# 5：在Ubuntu中安装Atom及其扩展
Atom只是一个代码查看工具，唯一的优点是支持傻瓜式的git，可以把它比喻为VScode+git，或者是Linux端VScode的替代品。

Atom有Windows版也有Linux版，我们这里需要用到Linux版。

由于网络原因直接用命令安装Atom经常失败（可能是我的梯子不给力）

这边推荐从Atom的[开源网站](https://github.com/atom/atom/releases/tag/v1.48.0)上手动下载<b>atom-amd64.deb</b>文件，然后用[FileZilla](https://filezilla-project.org/index.php)传输到Ubuntu的home目录下，解压安装。

`sudo dpkg -i atom-amd64.deb`
我这边安装的版本是1.48.0，亲测可行。

安装结束后，会生成一个<b>atom-amd64</b>的文件夹。将当前路径切换到该文件夹下，并执行`./atom`即可运行atom软件。或者直接在左下角找图标打开也可。

顺便一提：Atom可以像VScode一样安装一些扩展（packages）为代码编辑提供方便。但是Atom的扩展库是挂载在国外平台的，尽管科学上网也很难下载，经常显示失败。在网上搜索到了一些已经下载好的好用的扩展，打包放在[百度网盘](https://pan.baidu.com/s/16Dy_7r3bBZnJ47u5KZY4oQ)里了（提取码：atom），有需要的可以自取。解压后放在.atom/目录下的packages文件夹下，如果没有这个文件夹就新建一个重命名为packages，然后重启Atom就可以了。

参考链接：[[3]](https://www.codenong.com/cs106839950/)

# 6：配置本地主机的SSH key
这是最关键的一步，简而言之有了SSH key才能让你的电脑主机与GitHub仓库连接，从而进行代码托管。

## 以在Linux操作系统中为例，Windows操作系统与之类似：
### ①检查本地主机是否存在SSH key
`cd ~/.ssh`
`ls`
查看是否存在 id_rsa 和 id_rsa.pub文件。

如果存在，说明已经有SSH Key，直接跳到第③步。

如果不存在继续进行第②步。

### ②生成SSH key
`ssh-keygen -t rsa -C "xxx@xxx.com"`//xxx处自定义即可

执行后一直回车即可

### ③获取SSH key公钥内容（id_rsa.pub）
`cd ~/.ssh`
`cat id_rsa.pub`
复制下面一堆像乱码一样的东西，打开GitHub的`个人主页`-`settings`-`SSH and GPG keys`-`New SSH key`，粘贴到下面框内，上面自定义一个名称，保存。

### ④验证是否成功
`ssh -T git@github.com`
Linux端可能会提示有一些小问题，一路回车+yes就完事了，最终应该会有`You've successfully authenticated.`的字样，代表成功。

参考链接：[[4]](https://blog.csdn.net/weixin_42310154/article/details/118340458)

# 7：Fork一些好看的开源模板，用git工具克隆到本地并修改。

这段建议直接看这个[视频](https://www.bilibili.com/video/BV14x411t7ZU/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=9ab235065c7223489da575c6148cce40)，讲的比较全面，我就是按照这个up教的做的。感谢模板原作者[潘柏信](https://github.com/leopardpan/leopardpan.github.io)和up主[酒石酸菌](https://space.bilibili.com/4435845)。

补充在Ubuntu中安装中文拼音的[方法](https://blog.csdn.net/Robot_Starscream/article/details/90204639)。

（注：blog的markdown文件命名方式：年-月-日-名称.md（名称为英文））

# 8：用Jekyll预览制作好的静态网页
进入本地仓库所在文件夹内，右键打开终端，输入：
`jekyll server`
回车后打开生成的本地链接：（我的是）http://127.0.0.1:4000 进行预览。

# 9：发布到GitHub的仓库中
本地仓库所在文件夹内，右键打开终端，依次输入：
`git add .`
`git commit -m "abcdefg"` //引号内表示此次更新的备注
`git push origin master`
等待一分钟左右过后打开个人主页链接：https://xxx.github.io/ 查看效果。

# 10：致谢
感谢我的好鸽鸽好师兄，分享给我很多有趣好玩的东西，感兴趣的请关注他的[个人服务器](http://119.3.180.58)。

感谢GitHub平台，感谢包括本模板在内的所有开源爱好者们。

感谢很爱折腾的自己，今后这里将记录更多的自己遇到的知识。

但行好事，莫问前程。

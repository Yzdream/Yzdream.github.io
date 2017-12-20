---
layout: post
title: 'github+jekyll搭建博客'
subtitle: 'H2O Theme for Jekyll'
date: 2017-12-20
categories: jekyll
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: 博客 Github jekyll Hexo
---

一直都想搭建一个自己的博客，感觉很麻烦，一直都是看资料没有去实践，这几天突然心血来潮研究了几天决定动手了，一开始准备使用wordpress来搭建，查看资料之后发现要安装很多软件，还要买域名主机，对于还是菜鸟的我来说太需要时间了，然后我就在网上查阅初学搭建博客的资料，发现可以使用Github来搭建博客，只需要安装git就可以了，很方便，而且还有一些简约的模板可以使用，于是就决定使用Github来动手尝试了，这里我是用的jekyll的模板，感觉还不错。

## 搭建流程
### 第一步
既然是在Github上搭建博客，当然要有Github账号了，登录Github之后先创建我们仓库，注意：这里仓库的名称不能随便写，比如我的帐号是Yzdream，那么仓库名称应该是：Yzdream.github.io 。

![](https://i.imgur.com/jSWetF8.png)

![](https://i.imgur.com/BRC7lOA.png)

### 第二步
这样我们的仓库搭建完成了，接下来就是使用我们的Github Pages来搭建我们的博客界面了。

![](https://i.imgur.com/guDLIAA.png)

点击setting到设置界面，选择模板

![](https://i.imgur.com/zPQxrEq.png)

到这里我们就可以使用我们的仓库访问我们的博客了。

### 第三步
无论是使用jekyll的模板还是要更新我们的博客都是要使用git的，下面安装git，直接到官网下载即可，安装的选项全部默认就可以了。

[git官网 ](https://git-scm.com/)
直接下载安装即可
![](https://i.imgur.com/rKPMvo1.png)

### 第四部
安装完成后打开 Git Bash，将我们的仓库也就是博客克隆到本地。
![](https://i.imgur.com/X18Nr0N.png)
这样就是完成了，如果你要克隆到你指定的目标的话，就要先cd 到自己的目录了，克隆完成后我们可以将从jekyll下载的模板的demo所有文件复制到我们克隆下的目录中：

![](https://i.imgur.com/ZFvKk7h.png)

注意：在_config.yml文件中修改个人信息，具体可以参考官网上的说明，这里是我使用的一个模板，感觉还不错，给大家参考下

[源码及介绍 →](https://github.com/kaeyleo/jekyll-theme-H2O)

修改完成后提交就可以看到效果了，在刚才打开的Git Bash中输入 

提交所有文件

    $ git add . 
    $ git commit -a "提交说明" 
    $ git push origin master
    $ git push
这三个命令完成之后就可以看到加载模板之后的博客了。


## 最后

这次从0到1，大约用了一下午时间，搭建美化博客，后续也将持续完善和更新。想想这个过程也是一波三折啊，仓库我是昨晚就建好了的，今天上午完成项目之后就看了一下jekyll的模板，然后下午就开始美化我的博客家园了，作为一个Android开发的，看到这些css、js、html代码说真的有点无从下手的感觉了，搭建之前我还问过我的一个好朋友，本来是想让他指导一下的，不过他是用的Hexo搭建博客的，博客风格很简约，需要的可以参考一下[搭建 Hexo 博客 ](https://yinwenbing.github.io/2017/06/17/%E6%90%AD%E5%BB%BAHexo%E5%8D%9A%E5%AE%A2md/)，不过还是感觉我的比较好看，哈哈哈，希望能够坚持学习。

<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="MTAyMC8zMjU2Ny85MTI4">
	<script type="text/javascript">
   (function(d, s) {
       var j, e = d.getElementsByTagName(s)[0];

       if (typeof LivereTower === 'function') { return; }

       j = d.createElement(s);
       j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
       j.async = true;

       e.parentNode.insertBefore(j, e);
   })(document, 'script');
	</script>
<noscript> 为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->

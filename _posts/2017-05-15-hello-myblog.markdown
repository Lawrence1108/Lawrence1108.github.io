---
layout:     post
title:      "Welcome to Eule Blog"
subtitle:   " \"Hello World, Hello Blog\""
date:       2017-04-15 12:00:00
author:     "Eule"
header-img: "img/post-bg-2015.jpg"
comments: true
tags:
    - 生活
---

> “Yeah It's on. ”


## 前言

Eule 的 Blog 就这么开通了。

[跳过废话，直接看技术实现 ](#build)



2017 年，Eule 总算有个地方可以好好写点东西了。


作为一个程序员， Blog 这种轮子要是挂在大众博客程序上就太没意思了。一是觉得大部分 Blog 服务都太丑，二是觉得不能随便定制不好玩。之前因为太懒没有折腾，结果就一直连个写 Blog 的地儿都没有。

在玩了一段时间知乎之后，答题的快感又激起了我开博客的冲动。


<p id = "build"></p>
---

## 正文

接下来说说搭建这个博客的技术细节。  

正好之前就有关注过 [GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/) 快速 Building Blog 的技术方案，非常轻松时尚。

其优点非常明显：

* **Markdown** 带来的优雅写作体验
* 非常熟悉的 Git workflow ，**Git Commit 即 Blog Post**
* 利用 GitHub Pages 的域名和免费无限空间，不用自己折腾主机
	* 如果需要自定义域名，也只需要简单改改 DNS 加个 CNAME 就好了
* Jekyll 的自定制非常容易，基本就是个模版引擎


本来觉得最大的缺点可能是 GitHub 在国内访问起来太慢，目前先不管它了，回头有时间再优化吧。

---

配置的过程中也没遇到什么坑，基本就是 Git 的流程，相当顺手

大的 Jekyll 主题上直接 fork 了 Huxpro Blog（这个主题也个人感觉挺好看了符合我的审美，就不多赘述了。）

本地调试环境需要 `gem install jekyll`，结果 rubygem 的源居然被墙了……后来手动改成了大淘宝的镜像源才成功（其实本人也自备梯子完全不虚，不过领导说要低调）

Theme 的 CSS 是基于 Bootstrap 定制的，看得不爽的地方直接在 Less 里改就好了（平时更习惯 SCSS 些），**不过其实我一直觉得 Bootstrap 在移动端的体验做得相当一般……**所以为了体验，也补了不少 CSS 进去

最后就进入了耗时反而最长的**做图、写字**阶段，也算是进入了**写博客**的正轨，因为是类似 Hack Day 的方式去搭这个站的，所以折腾折腾着大半夜就过去了。

第二天考虑中文字体的渲染，fork 了 [Type is Beautiful](http://www.typeisbeautiful.com/) 的 `font` CSS，调整了字号，适配了 Win 的渣渲染，中英文混排效果好多了。

**得强调一下，有点惭愧基本上都是用的[Hux](https://github.com/Huxpro/huxblog-boilerplate)的模板改动不大，在此感谢作者，感谢 Jekyll、Github Pages 和 Bootstrap!……**

## 后记

回顾这个博客的诞生，纯粹是出于个人兴趣。

当然离不开开源带来的便捷，路过的小伙伴，如果喜欢这个主题可以去GitHub给[Hux](https://github.com/Huxpro/huxblog-boilerplate)一个start

如果你恰好逛到了这里，希望你也能喜欢这个博客主题。

—— Eule 后记于 2017.5
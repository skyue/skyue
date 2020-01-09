+++
title = "Chrome恢复网址中http和www的展示"
date = "2019-10-06T10:00:00+08:00"
slug = "2019100610"
+++

update @ 20191215:

v78之后，这两个选项取消了，已经没有办法显示完整URL了。

---

Chrome从某个版本开始，默认不显示网址中的http和www前缀，当时通过设置恢复显示，最近升级Chrome后又变为不显示了。

怀疑每次升级都会出现该问题，因此记录设置方法备查。

1、地址栏输入`chrome://flags/`进入Chrome设置，如下图

![Chrome Flags](/blog_static/2019/20191006-chrome.png)

2、找到`Omnibox UI Hide Steady-State URL Trivial Subdomains`和`Omnibox UI Hide Steady-State URL Scheme`设置项，均改为`Disabled`，然后重启Chrome即可。

*注：两个设置项分别控制子域名和协议的展示。*

---

不展示协议和子域名至少带来两个问题：

* 无论判断网址的准确性，从而对安全性有影响。虽然Google的安全性做的非常好，但必要时，我仍想人工进行double check。
* 修改网址定位不便。点击地址栏进入编辑状态时，协议和子域名会重新显示出来，会造成视觉和光标的偏移，影响交互体验。

当初看到这个功能，有种感觉：**一个非常成熟的产品，没什么可迭代了，但PM又不能闲着，于是没事找事，做些反人类的功能。**

最近看到一篇文章[Google Chrome正在走Windows的老路](https://mp.weixin.qq.com/s/8qIJDfKixHDwokIv0uuQXw)，虽然16年曾写文章说[越来越离不开Google](/blog/2016080623.html)，但近些年Google的口碑的确在崩坏，反而Satya Nadella领导下的微软越来越酷。

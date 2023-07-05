---
title: Kindle电子书批量下载与DRM移除教程
author: lin_sun
date: 2023-07-02 20:30:00 +0800
categories: [Tutorial, E-book]
tags: [Kindle, E-book]
comments: true
---

前些天（**2023.6.30**）亚马逊已停止Kindle中国电子书商店的运营，并将于**2024.6.30**停止中国电子书下载服务，这意味着如今Kindle用户已无法在亚马逊购买新的电子书，且在未来一年内必须将已购买的电子书全部下载至本地，否则之后将无法获取到已购买的电子书。

如果你与我一样曾在亚马逊购买了大量电子书，那么如何把这大量的电子书下载到本地就变成了一件麻烦事，因为亚马逊并不支持电子书的批量下载，并且即使下载到本地，其电子书也受DRM保护，无法在非本人账户绑定的设备上阅读，当然也就无法分享给他人。而本文就是为解决以上两个问题的简单教程。话不多说，上干货！

## 批量下载电子书与移除DRM

最简单的下载和移除DRM的方式是借助**[Kindle_download_helper](https://github.com/yihong0618/Kindle_download_helper)**这个开源工具，GUI（图形化界面）版本可以在[这里下载](https://github.com/yihong0618/Kindle_download_helper/releases)，选择适合自己系统的最新版本即可

下载打开这个工具后，界面如下：

![kindle-img-01.png](/posts/2023-07-02/kindle-img-01.png)

1. 登陆 [amazon.cn](https://www.amazon.cn/)
2. 访问 [https://www.amazon.cn/hz/mycd/myx#/home/content/booksAll/dateDsc/](https://www.amazon.cn/hz/mycd/myx#/home/content/booksAll/dateDsc/)
3. F12打开开发者工具面板，点击`网络`标签过滤`Fetch/XHR`的请求，随意点击请求列表中的一个请求，在`负载`中可以找到`csrfToken`，在`标头`中可以找到`Cookie`，复制两者的值到Kindle_download_helper中便能获取到你的电子书列表了
  ![kindle-img-02.png](/posts/2023-07-02/kindle-img-02.png)
  ![kindle-img-03.png](/posts/2023-07-02/kindle-img-03.png)
  ![kindle-img-04.png](/posts/2023-07-02/kindle-img-04.png)
  ![kindle-img-05.png](/posts/2023-07-02/kindle-img-05.png)
4. 获取到电子书列表后，选择要保存的本地路径，勾选DeDRM，再点击 下载全部 按钮便可在下载完成时自动移除DRM保护。
5. 此工具也可以用来获取个人文档，即你通过 Send to Kindle 功能发送到Kindle的个人文档，这些文档原本就不受DRM保护，所以无需勾选DeDRM。

## 仅移除DRM

对于他人分享的受DRM保护的Kindle电子书，这里再介绍一种移除DRM的方法。需要的内容如下：

1. [Calibre](https://calibre-ebook.com/) 强大的电子书管理软件+ [DeDRM_tools](https://github.com/apprenticeharper/DeDRM_tools) 电子书DRM移除工具
2. 此电子书对应账号设备的序列号（可以在亚马逊的个人设备中查到）

准备好上述内容后就可以进行DRM的移除操作了。

1. 安装Calibre的步骤这里不再介绍，安装完成后需要导入DeDRM_tools的[DeDRM_plugin](https://github.com/apprenticeharper/DeDRM_tools/releases)
  ![kindle-img-06.png](/posts/2023-07-02/kindle-img-06.png)
  ![kindle-img-07.png](/posts/2023-07-02/kindle-img-07.png)
2. 插件加载完成后在插件列表中搜索DeDRM，双击这个插件点击第一项并添加设备序列号
  ![kindle-img-08.png](/posts/2023-07-02/kindle-img-08.png)
3. 添加完成后把需要移除DRM的电子书添加到Calibre中，这时你已经可以在Calibre中直接双击打开这本电子书了，但实际这本电子书仍受DRM保护。要想得到一本无DRM的电子书还需进行电子书转换。选中这本电子书后点击转换，可以选择逐个转换或批量转换，这里以《地狱变》作为示例进行逐个转换
  ![kindle-img-09.png](/posts/2023-07-02/kindle-img-09.png)
4. 输入格式为AZW3保持不变，输出格式建议也用AZW3，此格式相对于MOBI能支持更丰富的排版
  ![kindle-img-10.png](/posts/2023-07-02/kindle-img-10.png)
5. 页面设置改成你自己的设备即可，这里选陪我近10年的 Kindle PaperWhite 3，其他配置按需调整，一般情况下无需要改动，点击确定即可
  ![kindle-img-11.png](/posts/2023-07-02/kindle-img-11.png)
6. 等待转换结束，右键该电子书，选择保存单一格式到硬盘，再选AZW3的电子书点确定，那么你就得到一本移除DRM的电子书啦🎉
  ![kindle-img-12.png](/posts/2023-07-02/kindle-img-12.png)
  ![kindle-img-13.png](/posts/2023-07-02/kindle-img-13.png)

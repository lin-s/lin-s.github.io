---
title: Stable Diffusion 入门教程（未完）
author: lin_sun
date: 2023-05-15 20:30:00 +0800
categories: [AI, Tutorial]
tags: [AI, AI绘画, Stable Diffusion]
comments: true
---
## Stable Diffusion 简介

Stable Diffusion 是 2022 年发布的深度学习文字到图像生成模型，能根据文字的描述生成详细图像。其代码和模型权重已在Github开源（开源地址：[stablediffusion](https://github.com/Stability-AI/stablediffusion)），可以在大多数配备有适度GPU的电脑硬件上运行。本文是一篇使用入门教程。

## 软硬件要求

### 软件要求

请自备一个合适的梯子，下载源代码，依赖，模型等都需要通过梯子。

### 硬件要求

- 内存：16G及以上
- 显卡：6G以上显存的显卡，推荐N卡（[A卡运算速度明显慢于 N 卡](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)），低于`GTX1060 6G`的显卡就不建议折腾了
- 硬盘：建议60G以上（一个`CHECKPOINT`模型通常为2~8G，硬盘越大就能容纳更多模型）

进行 512 x 512 图片生成时主流显卡速度对比：

![image-00.jpg](/posts/2023-05-15/image-00.jpg)

> 过度使用，显卡会有损坏的风险。{: .prompt-danger }

## 环境部署

### 手动部署

手动部署的方式适合有一定编程基础的人，能独立解决安装依赖过程中可能出现的报错。

详细的部署流程可以参考 webui 的官方wiki：[stable-diffusion-webui Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki)，这里只做简单介绍。

stable diffusion webui 的完整环境占用空间极大，能达到几十 G。同时，webui 需要联网下载安装大量的依赖，在中国境内的网络环境下载会很慢，请自带科学上网工具。

1. 安装 Python 安装 [Python 3.10](https://www.python.org/downloads/windows/)，安装时须选中 `Add Python to PATH`
2. 安装 Git 在 [Git-scm.com](https://git-scm.com/download/win) 下载 Git 安装包并安装。
3. 选择一个合适的目录（建议使用纯英文路径），右键空白处选择`Git Bash Here`，然后执行以下命令来克隆webui代码仓库：

    ```bash
    git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
    ```

4. 添加模型 可在如[Civitai](https://civitai.com/)上下载标注有`CHECKPOIN`的模型，有模型才能作画。下载的模型放入下载后文件路径下的`models/Stable-diffusion`目录。
5. 双击运行 `webui-user.bat` 。脚本会自动下载依赖，等待一段时间（可能很长），程序会输出一个类似 `http://127.0.0.1:7860/` 的地址，在浏览器中输入这个链接开即可。详细可参见[模型使用](https://github.com/civitai/civitai/wiki/How-to-use-models#fine-tuned-model-checkpoints-dreambooth-models)。
6. 后续更新 进入stable-diffusion-webui目录，右键选择`Git Bash Here`打开命令行窗口执行：

    ```bash
    git pull
    ```


### 整合包

觉得麻烦的同学可以使用整合包，解压即用，这里推荐[秋叶的启动器](https://www.bilibili.com/video/BV1iM4y1y7oA/?spm_id_from=333.999.0.0&vd_source=ffcf6aa94357de29ec85182901d261a2)，已集成各种常用的插件，且支持后续的更新。打开启动器后，可一键启动：

![image-01.png](/posts/2023-05-15/image-01.png)

如果有其他需求，可以在高级选项中调整配置。

![image-02.png](/posts/2023-05-15/image-02.png)

显存优化根据显卡实际显存选择，不要超过当前显卡显存。不过并不是指定了显存优化量就一定不会超显存，在出图时如果启用了过多的优化项（如高清修复、人脸修复、过大模型）时，依然有可能爆显存导致出图失败。

xFormers 能极大地改善了内存消耗和速度，建议开启。准备工作完毕后，点击一键启动即可。等待浏览器自动跳出，或是控制台弹出本地 URL 后说明启动成功

stable-diffusion-webui 更新比较频繁，可在“版本管理”菜单下进行本体和扩展的更新：

![image-03.png](/posts/2023-05-15/image-03.png)

其中本体分稳定版和开发版，就是一个稳定，一个功能更新，按需要选择就行。目前我在用的是最新的开发版，前些天是UI中文显示有问题，这两天感觉进度条有点问题，大家用稳定版比较好，当然也可以随时切换。

启动器的小工具页里也有很多实用的工具：

![image-04.png](/posts/2023-05-15/image-04.png)

### 安装插件

Stable Diffusion 可安装大量插件扩展，在 webui 的“扩展”选项卡下，可以安装插件。点击“加载扩展列表”后，列表会刷新，选择需要的插件点击右侧的安装按钮即可安装。webui界面默认为英文，若为手动部署方式，建议安装中文化扩展（需要取消隐藏 localization 标签），整合包方式已经集成。

![image-05.png](/posts/2023-05-15/image-05.png)

安装完毕后需要应用更改并重启前端，这样扩展才会生效：

![image-06.png](/posts/2023-05-15/image-06.png)

## 文生图流程

1. （必要）下载CHECKPOINT模型，并放置在的`models/Stable-diffusion`目录中
2. （必要）选择CHECKPOINT模型：选择需要使用的CHECKPOINT模型，如果选项中没有出现已下载的模型，点击右侧的刷新按钮刷新下。CHECKPONIT模型是对生成结果影响最大的因素，主要体现在画面元素以及整体风格上。
3. （可选）添加VAE模型：下载并选择VAE模型，存放路径为`models/VAE` 这类模型一般用于控制画面的色彩风格，可以简单理解为滤镜般的效果，可根据自己的喜好来决定是否开启。
4. （必要）填写正面提示词：在第一个框中填入提示词（Prompt），对想要生成的东西进行英文描述。
5. （可选）添加Lora模型：下载Lora模型放置在`models/Lora`目录中并在webui的Lora标签页刷新模型列表，然后在Prompt中添加`<lora:模型名:权重>`以及模型关键词来启用lora模型。此类模型用途宽泛，可以换脸，换体型，换风格，甚至可以左右画面的整体布局方式，建议根据需要下载和使用。

6. （建议）填写负面提示词：在第二个框中填入负面提示词（Negative prompt)，对不想要生成的东西进行英文描述。AI不擅长画手脚，建议添加 `bad hands` 之类的负面提示词。有些大模型会包含R18的内容，所以如果你希望最终生成的图片能顺利发布到国内的社交平台，请务必在负面提示词的开头加上`(NSFW)`。什么？你想发到国外的社交平台？！Σ(っ °Д °;)っ

7. （必要）设置参数：设置好采样方法、采样次数、图片尺寸，批次数量，随机种子等参数。
8. （必要）生成图像：点击生成按钮，然后就可以去喝杯咖啡等图像出来了。（温馨提醒：晚上建议把咖啡换成牛奶）
9. （必要）保存图像：生成的图像可以点击保存来下载，也可以直接在webui的`outputs`目录中找到。
10. （可选）重绘：如果prompt不够完善，生成的图像时常会出现一些瑕疵。像少只手，多条腿，八根手指，左右手互换等奇奇怪怪~~，乐趣无穷~~的问题都比较常见。这时可以将生成的图像发送到局部重绘，然后在左侧的原图中描出蒙版，覆盖瑕疵部分再点生成，至于能否重绘出不含瑕疵的内容就靠天意了。
11. （建议）后处理：生成图像时为了加快生成速度，一般不会设置太大的图像尺寸，等得到一张满意的图像后再把图像发送到后处理来进行放大处理，这样能节省不少时间（用4090显卡的土豪可以无视这条）。但如果你想生成一张细节异常丰富的壁纸级图像，那还是需要在前期生成阶段就设置较大尺寸的，毕竟大尺寸就意味着AI有更大的画布来生成更多元素，而放大图像并不会增加元素。

## 图生图流程

1. （必要）传原图：在图生图标签页的左侧上传一张你中意的原始图像
2. （可选）录入提示词：通过反推按钮分析原图生成提示词或手工录入提示词
3. （必要）这里复制粘贴文生图流程，搞定！

下图是一个直接使用网络图片反推提示词来生成图像的示例。嗯？你问为什么左边的猫变小女孩了？也许在AI看来，这女孩跟猫一样可爱吧~(●ˇ∀ˇ●)~

![image-07.png](/posts/2023-05-15/image-07.png)

## 提示词

提示词所做的工作是缩小模型出图的解空间，即缩小生成内容时在模型数据里的检索范围，而非直接指定作画结果。提示词的效果也受模型的影响，有些模型对自然语言做特化训练，有些模型对单词标签对特化训练，那么对不同的提示词语言风格的反应就不同。

### 提示词内容

提示词中可以填写以下内容：

- 自然语言：可以使用描述物体的句子作为提示词，建议都使用英文，且避免复杂的语法。
- 单词标签：可以使用逗号隔开的单词作为提示词。一般使用普通常见的单词。单词的风格要和图像的整体风格搭配，否则会出现混杂的风格或噪点。避免出现拼写错误。可参考[Danbooru Tags (donmai.us)](https://danbooru.donmai.us/tags)
- Emoji、颜文字 Emoji (💰👨👩🎅👼🍟🍕) 表情符号也是可以使用并且非常准确的。因为 Emoji 只有一个字符，所以在语义准确度上表现良好。关于 emoji 的确切含义，可以参考[Emoji List, v15.0 (unicode.org)](https://unicode.org/emoji/charts/emoji-list.html)，同时 Emoji 在构图上有影响。

对于使用 Danbooru 数据的模型来说，可以使用西式颜文字在一定程度上控制出图的表情。如：:-) 微笑 :-( 不悦 ;-) 使眼色 :-D 开心 :-P 吐舌头 :-C 很悲伤 :-O 惊讶 张大口 :-/ 怀疑

### 提示词语法

根据自己想画的内容写出提示词，多个提示词之间使用英文半角符号 [ , ]，如：

```
masterpiece, best quality, ultra-detailed, illustration, close-up, straight on, face focus, 1girl, white hair, golden eyes, long hair, halo, angel wings, serene expression, looking at viewer
```

可以使用括号人工修改提示词的权重（权重值最好不要超过 1.5）：

1. `()` 括号默认增加权重，权重将会乘以1.1，也可配合`:`准确控制权重，大部分情况下，操作权重用括号即可。
    - `(word)` - 将权重提高至原先的 1.1 倍
    - `((word))` - 将权重提高 1.21 倍（= 1.1 * 1.1）
    - `(word:1.5)` - 将权重提高 1.5 倍
    - `(word:0.25)` - 将权重减少为原先的 25%
2. `[]` 中括号 减少权重，本身权重除以1.1
    - `[word]` - 减少权重，本身权重除以1.1
3. `AND` 词缀
    - `a1 AND b1 AND c1 AND` 功能类似逗号，但不会像逗号一样区分出前后权重，用于将多个词缀聚合于一个提示位。
    - `a1:0.5 AND b1:0.5 AND c1:1.1` 在使用and的提示位中，用AND链接的词缀可使用冒号标记其权重。
    - AND 的作用类似 `[xx|xx|xx]` ,用于让多个词缀生成可控权重下的单个元素，如：`[xx|xx|xx]`趋向于融合，AND趋向于特征明显的共存。实例：`a girl, blue hair AND red hair, short hair` 将会生成一个蓝红混色染发的短发女孩

### 权重逻辑

一般而言，概念性的、大范围的、风格化的关键词写在前面，叙述画面内容的关键词其次，最后是描述细节的关键词，大致顺序如：

```
(画面质量提示词), (画面主题内容)(风格), (相关艺术家), (其他细节)
```

若是想明确某主体，应当使其生成步骤向前，生成步骤数加大，词缀排序向前，权重提高。

- 画面质量→主要元素→细节

若是想明确风格，则风格词缀应当优于内容词缀

- 画面质量→风格→元素→细节

不过在模型中，每个词语本身自带的权重可能有所不同，如果模型训练集中较多地出现某种关键词，我们在提示词中只输入一个词就能极大地影响画面，反之如果模型训练集中较少地出现某种关键词，我们在提示词中可能输入很多个相关词汇都对画面的影响效果有限。提示词的顺序很重要，越靠后的权重越低。关键词最好具有特异性，譬如 `Anime`(动漫)一词就相对泛化，而 `jojo` 一词就能清晰地指向 `jojo` 动漫的画风。还有上文图生图示例的反向提示词，将`human`更改为`male, female, boy, girl`就更具体了（如下图所示）。总之措辞越具象越好，尽量避免使用存在解释空间的措辞。

![image-08.png](/posts/2023-05-15/image-08.png)

### 生成算法

`[from:to:when]` 可以控制画面生成的顺序

```
[to:when] 在指定数量的 step 后，将to处的提示词添加到提示
[from::when] 在指定数量的 step 后从提示中删除from处的提示词
[from:to:when] 在指定数量的 step 后将from处的提示词替换为 to处的提示词
```

例如: `a [fantasy:cyberpunk:16] landscape` 在一开始，读入的提示词为：`the model will be drawing a fantasy landscape`. 在第 16 步之后,提示词将被替换为：`a cyberpunk landscape`, 它将继续在之前的图像上计算

又例如，对于提示词为: `fantasy landscape with a [mountain:lake:0.25] and [an oak:a christmas tree:0.75][ in foreground::0.6][ in background:0.25][shoddy:masterful:0.5]`，100 步采样， 一开始。提示词为：`fantasy landscape with a mountain and an oak in foreground shoddy` 在第 25 步后，提示词为：`fantasy landscape with a lake and an oak in foreground in background shoddy`  第 50 步后，提示词为：`fantasy landscape with a lake and an oak in foreground in background masterful` 在第 60 步后，提示词为：`fantasy landscape with a lake and an oak in background masterful` 在第 75 步后，提示词为：`fantasy landscape with a lake and a christmas tree in background masterful`

提示词还可以轮转，譬如

```
[cow|horse]in a field
```

在第一步时，提示词为`cow in a field`；在第二步时，提示词为`horse in a field`；在第三步时，提示词为`cow in a field` ，以此类推。

![image-09.gif](/posts/2023-05-15/image-09.gif)

### 提示词模板

可参考[Civitai](https://civitai.com/)中优秀作品的提示词作为模板。

类似的网站还有：

- [MajinAI](https://majinai.art/zh-cn/index.php)
- [词图 PromptTool](http://www.prompttool.com/NovelAI)
- [black_lily](http://heizicao.gitee.io/novelai/#/book)
- [Danbooru 标签超市](https://tags.novelai.dev/)
- [魔咒百科词典](https://aitag.top/)
- [AI 词汇加速器 AcceleratorI Prompt](https://ai.dawnmark.cn/)
- [NovelAI 魔导书](https://thereisnospon.github.io/NovelAiTag/)
- [鳖哲法典](https://aitag.icu/)
- [Danbooru Tag Groups Wiki](https://danbooru.donmai.us/wiki_pages/tag_groups)
- [AIBooru: Anime Image Board](https://aibooru.online/)


---
title: "个人博客搭建记录 hugo github actions "
date: 2023-01-14
---

{{< lead >}}
全文参考文档： https://nunocoracao.github.io/blowfish/docs/installation/

{{< /lead >}}

## 1. 安装hugo

如果您以前没有使用过 Hugo，则需要将其[安装到本地计算机上](https://gohugo.io/getting-started/installing)。您可以通过运行命令检查它是否已经安装`hugo version`。

{{< alert >}}
确保您使用的是**Hugo 版本 0.87.0**或更高版本，因为该主题利用了一些最新的 Hugo 功能。
{{< /alert >}}

[您可以在Hugo 文档](https://gohugo.io/getting-started/installing)中找到适用于您的平台的详细安装说明。

### 实操步骤

1. 下载zip文件

   > https://github.com/gohugoio/hugo/releases

2. 如图解压

   ![image-20230114190820149](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141908199.png)

3. 添加系统变量

   ![image-20230114190913732](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141909833.png)

   {{< alert >}}

   根据实际情况看是否需要重启电脑

   {{< /alert >}}

4. 命令行测试是否添加成功

   ![image-20230114191021147](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141910210.png)

   

## 2. 创建一个新站点

运行命令`hugo new site mywebsite`在名为`mywebsite`.

请注意，您可以随意命名项目目录，但下面的说明将假定它的名称为`mywebsite`. 如果您使用不同的名称，请务必相应地替换它。

### 实操步骤

1. 在`D:\hugo\Sites`目录下运行命令`hugo new site yoursitename` 

   ![image-20230114191322090](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141913153.png)

2. 查看新建目录结构，并在其内部打开终端，即`D:\hugo\Sites\mywebsite` 

   ![image-20230114191602643](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141916698.png)

## 3. 下载主题 --- blowfish

**使用 git 安装**

此方法是使主题保持最新的最快和最简单的方法。除了**Hugo**和**Go**之外，您还需要确保在本地计算机上安装了**Git 。**

切换到您的 Hugo 网站（您在上面创建的）的目录，初始化一个新的`git`存储库并将 Blowfish 添加为子模块。

```bash
cd mywebsite
git init
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

然后继续[设置主题配置文件](https://nunocoracao.github.io/blowfish/docs/installation/#set-up-theme-configuration-files)。



**另外：使用 git 更新**

可以使用`git`命令更新 Git 子模块。只需执行以下命令，最新版本的主题就会下载到您的本地存储库中：

```shell
git submodule update --remote --merge
```

更新子模块后，重建您的站点并检查一切是否按预期工作。

## 4. 复制配置文件

将建站时，删除在根目录自动生成的`config.toml` 

将`mywebsite/themes/blowfish/config/_default`复制到根目录`mywebsite/` （复制后文件结构如图所示）

![移动后](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142244236.png)

## 5. 具体配置

### 基本配置

> 语言、baseURL、主题

```toml
# config/_default/config.toml
theme = "blowfish"
baseURL = "https://mykaneki.github.io/"
defaultContentLanguage = "zh-cn"
pluralizeListTitles = "true"
```

`languages.en.toml`在配置文件夹中找到该文件，重命名为`languages.zh-cn.toml`

`[author]`配置决定了作者信息在网站上的显示方式。图片应该放在站点的`assets/`文件夹中。链接将按照它们列出的顺序显示。

```toml
# config/_default/languages.zh-cn.toml
languageCode = "zh-cn"
languageName = "简体中文"
displayName = "ZH-CN"
isoCode = "zh-cn"
# 语言权重，越小越重
weight = 1
# 语言是否是从右到左显示的
rtl = false

title = "mykaneki's blog"
logo = "img/logo.png"
description = "My awesome website"
dateFormat = "2006-01-02"

[author]
  name = "mykaneki"
  # assets\img\OIP-C.jfif
  image = "img/OIP-C.jfif"
  headline = "I'm only human"
  bio = "Hello World"
  links = [
    { email = "cmy_kaneki@qq.com" },
    { github = "https://github.com/mykaneki" }
  ]

```

对应效果图

![image-20230114230710443](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142307715.png)



### 菜单

```toml
# config/_default/menus.zh-cn.toml
[[main]]
    # 显示的名称
    name = "文章"
    # 指向的目录
    pageRef = "posts"
    # 权重，自左向右变大
    weight = 20
[[main]]
	# 图标
    pre = "github"
    # 名称
    # name = "GitHub"
    # 指向外部链接
    url = "https://github.com/nunocoracao/blowfish"
    weight = 30
```

`pageRef`指向的是`content\posts`

![image-20230114231536649](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142315745.png)

效果图![image-20230114231921446](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142319867.png)

### 缩略图和背景

给每篇文章设置一个图片作为背景

图片名称为`feature.*`

文章内容应该写在`index.md`中

![image-20230114232122686](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142321738.png)

{{<alert>}}

`_index.md` 是一个索引文件，作用是列出该目录下所有文章

{{</alert>}}

<br/><br/>

{{<alert>}}

一篇文章可以引用另一篇文章，此时需要注意目录的名称不能使用中文，而要使用英文，否则无法识别。

{{</alert>}}

```html
引用示例：
{{</* article link="/posts/networkmapping/" */>}}
```

**文章目录分类** 

posts放比较短的文章，并且没有背景图片

![image-20230114233033330](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142330444.png)

 ![image-20230114233534617](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142335496.png)

docs放较长的笔记，有背景图片

![image-20230114233417834](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142334888.png)

![image-20230114233515051](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142335342.png)

series放系列文章，串联各个文章

![image-20230114233604763](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142336857.png)

![image-20230114233552790](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142335429.png)

### 标签和系列

在每一篇文章的头部进行设置

```html
---
title: "Shortcodes"
date: 2020-08-11
draft: false
description: "All the shortcodes available in Blowfish."
slug: "shortcodes"
tags: ["shortcodes", "mermaid", "icon", "lead", "docs"]
series: ["Documentation"]
series_order: 8
---
```

{{<alert>}}

`:` 冒号后必须要空一格

tags要用双引号，而不是单引号

不能在tags中使用`\` 但是可以使用`/` （最好是都不要用），经测试，C\C++会显示成下图

{{</alert>}}

 ![image-20230114234407262](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301142344358.png)

## 6. 浏览量、分析支持

1. 转到[Firebase 网站](https://firebase.com/)并免费创建一个帐户

2. 创建一个新项目

3. 选择分析位置

4. 通过获取项目的变量并将它们设置在`params.toml`文件中，在 Blowfish 中设置 firebase。可以在[该页面](https://nunocoracao.github.io/blowfish/docs/configuration/#theme-parameters)中找到更多详细信息。您可以在下面找到 Firebase 将提供的文件示例，注意 FirebaseConfig 对象中的参数。

   ![image-20230115002150587](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301150021041.png)

5. 设置 Firestore - 选择构建并打开 Firestore。新建一个数据库，选择以生产模式启动。选择服务器位置并等待。启动后，您需要配置规则。只需复制并粘贴下面的文件，然后按发布。

![image-20230115000536111](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301150005255.png)

![image-20230115000737717](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301150007934.png)



## 7. 托管和部署


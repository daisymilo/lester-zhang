---
date: 2024-04-05
categories:
    - 其他技术
---

# 我是如何搭建个人主页的
我发现单纯的PDF简历在篇幅和形式上有很大限制，早些时候我曾尝试在微信公众号上发布内容，但编辑繁琐并且功能匮乏，所以决定制作这个网站向大家展示和分享我的工作。

<!-- more -->

这个网站是通过mkdocs-material制作的，并借助GitHub Pages发布到互联网上。此外，我还使用了阿里云和腾讯云的服务设置了域名并加速网站访问（国内访问GitHub服务器较慢）。其中的主要花费是购买域名和CDN加速服务，也就是说，如果你不需要自定义域名或者针对国内访问加速的话，部署个人网站的财务成本是零（但你还是需要花费**大约一个下午**的时间）。

我的整个部署流程是：**配置开发环境:octicons-arrow-right-24:制作和发布网页:octicons-arrow-right-24:自定义域名:octicons-arrow-right-24:CDN加速**，我不会详细地介绍每一步如何操作，但我会将我所用到的资料都提供给你，并提醒你可能遇到的“坑”。

## 配置开发环境
我在这里列出了我所用到的开发工具，你需要对这些工具有一些基本的了解。未来我会写一些相关的介绍，但在此之前你可能要查阅一些相关资料。

网页制作是在python环境下进行的（**但不需要编写任何python代码**），这里我使用[anaconda](https://www.anaconda.com/download)来安装和管理python环境，使用[pycharm](https://www.jetbrains.com/pycharm/download/?section=windows)，来编写和测试网页。如果你有vscode，那么它可以完全代替pycharm。
!!! tip
    
    你可以在[这个](https://www.jetbrains.com/community/education/#students)页面申请pycharm的教师/学生认证，从而免费使用pycharm

此外，我们还需要[Git](https://git-scm.com/downloads)工具将网站同步到[GitHub](https://github.com/)上，后续对网页的更新也一直需要用到它。如果你不熟悉Git和GitHub，你可以把GitHub当作一个网盘，每当你对电脑上的代码进行修改后Git可以帮你把变动同步到网盘上。此外，GitHub提供的Pages服务还能帮我们把网页发布到互联网上，让其他人可以访问，因此在开始前你需要注册一个GitHub账号。

## 制作网页
我制作网页使用的是[mkdocs-material](https://squidfunk.github.io/mkdocs-material/)，它可以帮助你用简单的markdown语法进行网页设计，而不需要编写html代码，例如下面是这个页面的一部分markdown代码：

```markdown
## 配置开发环境
我在这里列出了我所用到的开发工具，你需要对这些工具有一些基本的了解。未来我会写一些相关的介绍，但在此之前你可能要查阅一些相关资料。

网页制作是在python环境下进行的（**但不需要编写任何python代码**），这里我使用[anaconda](https://www.anaconda.com/download)来安装和管理python环境，使用[pycharm](https://www.jetbrains.com/pycharm/download/?section=windows)，来编写和测试网页。如果你有vscode，那么它可以完全代替pycharm。
!!! tip
    
    你可以在[这个](https://www.jetbrains.com/community/education/#students)页面申请pycharm的教师/学生认证，从而免费使用pycharm

此外，我们还需要[Git](https://git-scm.com/downloads)工具将网站同步到[GitHub](https://github.com/)上，后续对网页的更新也一直需要用到它。

如果你不熟悉Git和GitHub，你可以把GitHub当作一个网盘，每当你对电脑上的代码进行修改后Git可以帮你把变动同步到网盘上。此外，GitHub提供的Pages服务还能帮我们把网页发布到互联网上，让其他人可以访问，因此在开始前你需要注册一个GitHub账号。

...
```

mkdocs-marterial提供了从制作到发布网页的[新手教程](https://squidfunk.github.io/mkdocs-material/getting-started/)，[这个视频](https://www.youtube.com/watch?v=Q-YA_dA8C20&ab_channel=JamesWillett)（如果你能访问YouTube的话）也可以帮助你快速入门。 将制作好的网页发布到GitHub上非常简单，但你可能需要花一些时间来学习如何制作出你想要的网页效果，你可以参考我的[源代码](https://github.com/daisymilo/lester-zhang/tree/main)。

## 自定义域名
当你按照教程将网页发布后，页面的链接是这样的：`username.github.io/repository`，你可能想自定义一个容易记住的域名，就像本站的`lester-zhang.com`一样。

首先，你需要到域名托管商注册你想要的域名，我选择的是[阿里云域名注册](https://wanwang.aliyun.com/domain/)，但我更推荐[腾讯云域名注册](https://dnspod.cloud.tencent.com/)，因为后面的CDN加速服务我们选择腾讯云的，这样就可以在同一个平台上进行。

!!! tip

    在阿里云或腾讯云注册域名都需要进行实名认证，认证过程较为繁琐，并且需要等待审核，你可能需要提前准备。或者你可以选择国外的域名托管服务，例如[godaddy](https://hk.godaddy.com/en)


!!! warning

    在注册域名时会碰到**域名备案**的提示，我们可以忽略该要求。根据规定，国内服务器绑定的域名需要进行备案，但我们的域名是绑定在境外GitHub的服务器上，因此没有强制备案的要求

注册好域名之后就需要将域名解析到你的网页上面，这两篇文章提供了详细的指导：[自定义域名教程2](https://blog.csdn.net/m0_47520749/article/details/124768135)和[自定义域名教程2](https://zhuanlan.zhihu.com/p/393050270)。

!!! tip

    对于腾讯云和阿里云，教程中提到的DNS配置页面分别在：[腾讯云DNS配置](https://cloud.tencent.com/product/dns)和[阿里云DNS配置](https://wanwang.aliyun.com/domain/dns)

## CDN加速
我们的网页是部署在境外的GitHub服务器上，国内访问速度较慢且不太稳定，可以通过CDN加速来缓解这一问题。这里我选择[腾讯云的CDN加速服务](https://cloud.tencent.com/product/cdn)，因为其免费的证书有效期为1年，而阿里云只有3个月。这篇教程解释了CDN的原理并详细介绍了相关操作：[CDN加速教程1](https://zhuanlan.zhihu.com/p/393779644)，这篇博客也介绍了相似的内容[CDN加速教程2](https://blog.csdn.net/m0_47520749/article/details/124768311)。

在配置CDN加速服务前需要购买流量，否则配置完成后网页无法访问。对于腾讯云，可以在CDN产品页面进行购买，资源包类型应选择**流量**，适合区域选择**亚太1区**，资源包规格选择**100GB**
<div style="text-align: center;">
<img src="/images/tencent_cloud1.png" width="500" >
</div>

在跟随教程时，需要注意下面这些问题：

!!! warning
    
    在添加域名时，需要添加顶级域名和二级域名，例如，我添加了`lester-zhang.com`和`www.lester-zhang.com`，并分别配置DNS解析，以保证两个链接都能够正常访问。

!!! warning

    使用CDN加速不需要在GitHub上设置**Custom domain**，而是需要在docs目录下新建名为CNAME的文件，在文件中写入你的域名。在设置CDN时，回源host需要与CNAME中填写的域名完全一致

!!! tip

    如果你在**验证域名**环节一直失败，你需要检查你添加的DNS解析**记录类型**是否与要求的一致。教程中的记录类型为**TXT**，而有时腾讯云会要求设置记录类型为**CNAME**


在完成全部配置后，还需要做下列检查工作：


!!! warning

    需要在域名管理中检查**HTTPS配置**中的**HTTPS服务**是否打开，处于关闭状态时网页可能无法正常访问。该服务每月提供300万次免费的访问，足够我们的个人网站使用。
    <div style="text-align: center;">
    <img src="/images/tencent_cloud2.png" width="600" >
    </div>

!!! warning

    需要在域名管理中检查**回源配置**中的**回源跟随301/302配置**是否打开，处于关闭状态可能会影响CDN加速效果。

    <div style="text-align: center;">
    <img src="/images/tencent_cloud4.png" width="600" >
    </div>

待CDN服务部署完毕后，如果在概览页面可以看到**用量概览**出现变动，则说明CDN配置成功，网页已经成功上线。


    
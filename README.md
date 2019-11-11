# alfred实现oss图床（python）

## 写在前面

### 为什么写图床工具

很多开发者都喜欢用markdown写文本，博客。对于图片的处理有放在本地一起的，有放在云端的图片服务器，如微博图床，七牛，阿里oss等。个人不喜欢放在微博图床，更建议对象存储在自建图床中

### 什么是alfred

Alfred，一款macos的效率工具，是一个让你可以丢掉鼠标的神器（其实是我买了macbookpro之后买不起magic mouse了）。这款工具里有个workflow（付费版）功能可以提供开发者自己开发一系列的工作流。本文就是基于这个工具实现一键上传图片

### 为什么自己开发

我其实找过很多工具，虽然网上也有类似应用，比如ipic(部分功能付费)。不想折腾的同学可以用这款工具。

<div align=center><img width="90%" height="90%" src="https://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20/1532053944.png"/></div>

这款工具本人也是用过一段时间，微博图床免费，但是阿里oss或者七牛都是付费版的。既然如此，那么我就自己开发一款吧（还不是因为穷）
ps. 本文涉及到的图片都是私人图床上的图片


## 如何使用

### 最终效果图

复制一张图片(通过微信的快捷键截图效果飞起)，调用alfred，输入oss命令稍等一小会儿，会显示url和md两种返回格式，点击url或者md便可以获得对应地址在剪切板中，粘贴，所见即所得
<div align=center><img width="90%" height="90%" src="https://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20/%E5%BD%95%E5%B1%8F.gif"/></div>


### 环境

- 你需要一台操作系统为macos的电脑
- 你还需要安装付(po)费(jie)版的alfred
- 你少不了自己的阿里云oss服务
- 因为脚本是python写的，所以要有python3环境
- python3依赖oss2和pyobjc包

开始前的准备
``` shell
pip3 install oss2
pip3 install pyobjc
```

### 修改环境变量里的参数

打开workflow的环境变量设置，在这里面可以设置workflow的环境变量。

<div align=center><img width="90%" height="90%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2019-11-11%2F1573484905.png"/></div>

**修改workflow中的环境变量**
<div align=center><img width="90%" height="90%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2019-11-11%2F1573484555.png"/></div>

注：bucket_name需要符合阿里的命名规则,如下图所示
<div align=center><img width="90%" height="90%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-09-22%2F1537613559.png"/></div>

### 配置alfred的workflow

打开alfred，找到oss的workflow，双击下图的脚本图标


<div align=center><img width="90%" height="90%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20%2F1532057097.png"/></div>

如果你的python配置和我这儿一样的话，可以不修改。这里需要找到你的环境中的python执行指令，然后执行该脚本

<div align=center><img width="90%" height="90%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2019-11-11%2F1573484807.png"/></div>


## 写在最后

1. 目前只实现了oss，为什么不实现七牛的，因为呃呃呃，我还没申请七牛账号，然后阿里oss够我用了，后续有许多人需要我会继续开发哈。
2. 如上的话就是自己实现小插件的心得，分享出来供大家使用。如果有疑问可以私我微信CQHui94


github地址：
https://github.com/CQHui/oss_upload

如果喜欢可以star下，
如果喜欢的不能自己，可以再悬赏下~
<div align=center><img width="40%" height="40%" src="http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20%2F1532057805.png"/></div>



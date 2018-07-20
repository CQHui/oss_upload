# alfred实现oss图床（python）

## 写在前面

### 为什么写图床工具

很多开发者都喜欢用markdown写文本，博客。对于图片的处理有放在本地一起的，有放在云端的图片服务器，如微博图床，七牛，阿里oss等。个人不喜欢放在微博图床，更建议对象存储在自建图床中

### 什么是alfred

Alfred，一款macos的效率工具，是一个让你可以丢掉鼠标的神器（其实是我买了macbookpro之后买不起magic mouse了）。这款工具里有个workflow（付费版）功能可以提供开发者自己开发一系列的工作流。本文就是基于这个工具实现一键上传图片

### 为什么自己开发

我其实找过很多工具，虽然网上也有类似应用，比如ipic(部分功能付费)。不想折腾的同学可以用这款工具。
![](https://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20/1532053944.png)

这款工具本人也是用过一段时间，微博图床免费，但是阿里oss或者七牛都是付费版的。既然如此，那么我就自己开发一款吧（还不是因为穷）
ps. 本文涉及到的图片都是私人图床上的图片


## 如何使用

### 实习效果

复制一张图片，调用alfred，输入oss命令稍等一小会儿，会显示url和md两种返回格式，点击url或者md便可以获得对应地址在剪切板中，粘贴，所见即所得
![](https://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20/%E5%BD%95%E5%B1%8F.gif)

### 环境

- 你需要一台操作系统为macos的电脑
- 你还需要安装付(po)费(jie)版的alfred
- 你少不了自己的阿里云oss服务
- 因为脚本是python写的，所以要有python3环境
- python3依赖oss2和pyobjc包

### 上码

``` python
# Author: chenqihui
query = "{query}"
import time
import oss2
import json

from AppKit import NSPasteboard, NSPasteboardTypePNG, NSPasteboardTypeTIFF

access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
bucket_name = '<yourBucketName>'

def get_paste_img_file():
    """
    将剪切板数据保存到本地文件并返回文件路径
    """
    pb = NSPasteboard.generalPasteboard()  # 获取当前系统剪切板数据
    data_type = pb.types()  # 获取剪切c板数据的格式类型

    # 根据剪切板数据类型进行处理
    if NSPasteboardTypePNG in data_type:          # PNG处理
        data = pb.dataForType_(NSPasteboardTypePNG)
        filename = '%s.png' % int(time.time())
        filepath = '/tmp/%s' % filename            # 保存文件的路径
        ret = data.writeToFile_atomically_(filepath, False)    # 将剪切板数据保存为文件
        if ret:   # 判断文件写入是否成功
            return filepath
    elif NSPasteboardTypeTIFF in data_type:         #TIFF处理： 一般剪切板里都是这种
        # tiff
        data = pb.dataForType_(NSPasteboardTypeTIFF)
        filename = 'HELLO_TIFF.tiff'
        filepath = '/tmp/%s' % filename
        ret = data.writeToFile_atomically_(filepath, False)
        if ret:
            return filepath


def upload_file():

    auth = oss2.Auth(access_key_id, access_key_secret)
    # Endpoint以杭州为例，其它Region请按实际情况填写。
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', bucket_name)
    file_name = get_paste_img_file()
    key_name = file_name[file_name.rfind('/'):]
    date = time.strftime("%Y-%m-%d", time.localtime())
    key = date + key_name
    result = bucket.put_object_from_file(key, file_name)
    url = result.resp.response.url
    data = {
        'items' : [
            {'title' : 'url', 'arg': url,  "icon":
                {
                    'type': 'png',
                    'path': 'icon.png'
                }
             },
            {'title': 'md', 'arg': '![](%s)' % url, 'icon':
                {
                    'type': 'png',
                    'path': 'icon.png'
                }
             }
        ]
    }
    url_result = json.dumps(data)
    print(url_result)



if __name__ == '__main__':
    upload_file()
```

### 修改脚本里的参数

``` python
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
bucket_name = '<yourBucketName>'
```

### 配置alfred的workflow

打开alfred，找到oss的workflow，双击下图的脚本图标

![](http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20%2F1532057097.png)

如果你的python配置和我这儿一样的话，可以不修改。这里需要找到你的环境中的python执行指令，然后执行该脚本

![](http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20%2F1532057285.png)

## 写在最后

1. 目前只实现了oss，为什么不实现七牛的，因为呃呃呃，我还没申请七牛账号，然后阿里oss够我用了，后续有许多人需要我会继续开发哈。
2. 如上的话就是自己实现小插件的心得，分享出来供大家使用。如果有疑问可以私我微信CQHui94


github地址：
https://github.com/CQHui/oss_upload

如果喜欢可以star下，
如果喜欢的不能自己，可以再悬赏下~
![](http://qihui-picture.oss-cn-hangzhou.aliyuncs.com/2018-07-20%2F1532057805.png)


# For Developers

## 调试源码
* 下载源码: git clone https://github.com/ForgQi/bilibiliupload.git
* 安装: `pip3 install -e .` 或者 `pip3 install -r requirements.txt`
* 启动: `python3 -m biliup`

线程池限制并发数，减少磁盘占满的可能性。检测下载情况卡死或者下载超时，重试三次保证可用性。代码更新后将在空闲时自动重启。


下载整合了ykdl、youtube-dl、streamlink，不支持或者支持的不够好的网站可自行拓展。
下载和上传模块插件化，如果有上传或下载目前不支持平台的需求便于拓展。

下载基类在`engine/plugins/base_adapter.py`中，拓展其他网站，需要继承下载模块的基类，加装饰器`@Plugin.download`。

拓展上传平台，继承`engine/plugins/upload/__init__.py`文件中上传基类，加装饰器`@Plugin.upload`。

实现了一套基于装饰器的事件驱动框架。增加其他功能监听对应事件即可，比如下载后转码：
```python
# e.p.给函数注册事件
# 如果操作耗时请指定block=True, 否则会卡住事件循环
@event_manager.register("download_finish", block=True)
def transcoding(data):
    pass
```

## Deprecated
* ~~selenium操作浏览器上传两种方式~~(详见bili_chromeup.py)
* ~~Windows图形界面版在release中下载AutoTool.msi进行安装~~[AutoTool.msi](https://github.com/ForgQi/bilibiliupload/releases/tag/v0.1.0)
  QQ群：837362626

>关于B站为什么不能多p上传\
目前bilibili网页端是根据用户权重来限制分p数量的，权重不够的用户切换到客户端的提交接口即可解除这一限制。
>用户等级大于3，且粉丝数>1000，web端投稿不限制分p数量

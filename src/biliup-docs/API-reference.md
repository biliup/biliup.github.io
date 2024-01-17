# API调用
## EMBEDDING BILIUP
如果你不想使用完全自动托管的功能，而仅仅只是想嵌入biliup作为一个库来使用这里有两个例子可以作为参考
### 上传
```python
from biliup.plugins.bili_webup import BiliBili, Data

video = Data()
video.title = '视频标题'
video.desc = '视频简介'
video.source = '添加转载地址说明'
# 设置视频分区,默认为160 生活分区
video.tid = 171
video.set_tag(['星际争霸2', '电子竞技'])
with BiliBili(video) as bili:
    bili.login_by_password("username", "password")
    for file in file_list:
        video_part = bili.upload_file(file)  # 上传视频
        video.append(video_part)  # 添加已经上传的视频
    video.cover = bili.cover_up('/cover_path').replace('http:', '')
    ret = bili.submit()  # 提交视频
```
### 下载
```python
from biliup.downloader import download

download('文件名', 'https://www.panda.tv/1150595', suffix='flv')
```

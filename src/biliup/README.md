# biliup
> 源码：<https://github.com/ForgQi/biliup>\
> 演示视频（demonstration video）：[BV1ip4y1x7Gi](https://www.bilibili.com/video/BV1ip4y1x7Gi)

biliup是一个自动监测直播的工具，
支持自动录制各大直播平台实时流，上传到bilibili。同时也支持：
* twitch直播回放列表自动搬运至b站，如链接https://www.twitch.tv/xxxx/videos?filter=archives&sort=time
* 可自动选择或手动设置上传线路，保证国内外vps上传质量和速度以跑满带宽，
可分别控制下载与上传并发量
* 支持Web API与客户端API上传，支持分p上传

## 常见问题
#### 1. 为什么不能上传，如返回400错误
由于目前使用账号密码登录，大概率触发验证。请使用命令行工具登录B站获取cookie和token，将登录返回的信息填入配置文件，且使用引号括起yaml中cookie的数字代表其为字符串
#### 2. 为什么不能下载，日志文件内显示找不到文件
检查FFmpeg版本，过老的版本不能正常下载
#### 3. 斗鱼下载为什么报错
Please install at least one of the following Javascript interpreter.'\
python packages: PyChakra, quickjs\
applications: Gjs, CJS, QuickJS, JavaScriptCore, Node.js, etc.
## Credits
* Thanks `ykdl, youtube-dl, streamlink` provides downloader.

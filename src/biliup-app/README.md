# biliup-app介绍
> 源码：<https://github.com/ForgQi/Caution> \
> 演示视频：[链接](https://www.zhihu.com/zvideo/1482481163700367361)

bilibili投稿客户端，支持分p上传线路选择，支持Windows，Linux，macOS。

主要是为了解决现有网页端不能多p投稿的问题，虽然现有b站客户端可以多p
但是有几个问题：
* 仅支持Windows，不支持Linux，macOS
* 不能批量选择文件，多p只能多次单选文件
* 投稿线路对国外不友好，速度较慢稳定性较差，且不能自行切换
* 不能调整单视频上传并发数，单视频限制大小4G以内
* 仅能通过短信登录

基于TAURI: ` GUI: Svelte , 后端: Rust`


![login](../resource/login.png)
![main](../resource/main.png)


## Roadmap
- [x] 短信登录, 二维码登录
- [x] 上传视频封面
- [x] 自由切换投稿线路
- [x] 设置投稿并发数
- [x] 多p按照文件名排序
- [ ] 远程部署agent，本地操作
- [ ] 插件系统，如自动录播后上传

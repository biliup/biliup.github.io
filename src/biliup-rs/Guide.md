# 使用指南
投稿支持两种模式：
* 快速投稿，输入 `biliup upload test1.mp4 test2.mp4` 即可快速多p投稿；
* 通过配置文件投稿，配置文件详见 [config.yaml](#useage) ，
支持按照 Unix shell style patterns 来批量匹配视频文件，如 `/media/**/*.mp4` 匹配 media 及其子目录中的所有 mp4 文件且可以自由调整视频标题、简介、标签等：

## USEAGE
同一条匹配规则内的视频将多p投稿在一个稿件内，多个匹配条目将分为多个视频投稿，
多个视频标签使用逗号分隔。
* 配置文件示例：
```yaml
line: kodo
limit: 3
streamers:
  视频patterns1*:
    copyright: 1
    source: 转载来源
    tid: 171 # 投稿分区
    cover: "" # 视频封面
    title: 标题
    desc_format_id: 0
    desc: 简介
    dynamic: ""
    subtitle:
      open: 0
      lan: ""
    tag: ""
    dtime: ~
    open_subtitle: false
  视频patterns2*:
    copyright: 1
    source: 转载来源
    tid: 171 # 投稿分区
    cover: /cover/up.jpg # 视频封面
    title: 标题
    desc_format_id: 0
    desc: 简介
    dynamic: ""
    subtitle:
      open: 0
      lan: ""
    tag: ""
    dtime: ~
    open_subtitle: false
```
* 查看完整用法命令行输入 `biliup -h`
```shell
$ biliup help upload

USAGE:
    biliup.exe upload [OPTIONS] [VIDEO_PATH]...

ARGS:
    <VIDEO_PATH>...    需要上传的视频路径,若指定配置文件投稿不需要此参数

OPTIONS:
    -c, --config <FILE>            Sets a custom config file
        --copyright <COPYRIGHT>    是否转载 1 原创 2 转载 [default: 1]
        --cover <COVER>            视频封面 
        --desc <DESC>              视频简介 
        --dtime <DTIME>            延时发布时间，距离提交大于4小时，格式为10位时间戳
        --dynamic <DYNAMIC>        空间动态 
    -h, --help                     Print help information
    -l, --line <LINE>              选择上传线路，支持kodo, bda2, qn, ws
        --limit <LIMIT>            单视频文件最大并发数 [default: 3]
        --source <SOURCE>          是转载来源 
        --tag <TAG>                视频标签 
        --tid <TID>                投稿分区 [default: 171]
        --title <TITLE>            视频标题 
```

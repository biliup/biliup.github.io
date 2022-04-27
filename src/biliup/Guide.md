# 使用指南
需要创建配置文件 **[config.yaml](https://github.com/ForgQi/biliup/blob/master/config(demo).yaml)**
才可以启动，
启动成功后会在命令执行目录生成日志文件，视频录像文件也保存在此目录，
在更新源码后程序会在空闲时自动重启
```shell
# 启动
$ biliup start
# 退出 
$ biliup stop
# 重启 
$ biliup restart
# 查看版本
$ biliup --version
# 显示帮助以查看更多选项
$ biliup -h
```
命令行选项：
```shell
usage: biliup [-h] [--version] [-v] [--config CONFIG]
                   {start,stop,restart} ...

Stream download and upload, not only for bilibili.

positional arguments:
  {start,stop,restart}  Windows does not support this sub-command.
    start               Run as a daemon process.
    stop                Stop daemon according to "watch_process.pid".

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -v, --verbose         Increase output verbosity
  --config CONFIG       Location of the configuration file (default
                        "./config.yaml")

```
## 最小配置文件示例
新创建配置文件需填写以下必填项

tid投稿分区见[Wiki](https://github.com/ForgQi/biliup/wiki)
```yaml
user: 
    cookies:
        SESSDATA: your SESSDATA
        bili_jct: your bili_jct
        DedeUserID__ckMd5: your ckMd5
        DedeUserID: your DedeUserID
    access_token: your access_key

streamers:
    xxx直播录像: 
        url:
            - https://www.twitch.tv/xxx
```

## 上传完成后发送邮件通知
注意：发送邮件通知需要有可用的 smtp 服务，一般邮件如 QQ 邮箱等都会提供，具体查询相关服务的文档

在 streamers 配置的 `postprocessor` 中添加一个 `run` 操作，如下

```
postprocessor:
    - run: python3 path/to/mail.py
```

其中 mail.py 如下，注意修改服务器相关的配置

```
from email import encoders
from email.header import Header
from email.mime.text import MIMEText
from email.utils import parseaddr, formataddr

import smtplib
import sys
from functools import reduce
from operator import add
    
def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, 'utf-8').encode(), addr))

# print(sys.stdin)
files = []
for line in sys.stdin:
    files.append(line.strip())

from_addr = 'noreply@xx.com'
password = 'psw'
to_addr = 'xx@gmail.com'
smtp_server = 'smtp.xx.com'

first_file_name = files[0].split('/')[-1]
more_text = '' if len(files) <= 1 else  f' 等 {len(files)}个' 
msg = MIMEText(reduce(add, map(lambda x: x + '\n', files)), 'plain', 'utf-8')
msg['From'] = _format_addr('noreply <%s>' % from_addr)
msg['To'] = _format_addr('Ekko <%s>' % to_addr)
msg['Subject'] = Header(f'文件上传完成 {first_file_name}{more_text}', 'utf-8').encode()
    
server = smtplib.SMTP_SSL(smtp_server, 465)
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```

## 完整配置文件示例
如需细致自定义配置可参考以下文件，大部分为可选项，可不填
```yaml
user: # 在填了cookies的情况下优先使用cookies上传，如需使用用户名密码上传请注释掉cookies
    cookies:
        SESSDATA: your SESSDATA
        bili_jct: your bili_jct
        DedeUserID__ckMd5: your ckMd5
        DedeUserID: your DedeUserID
    access_token: your access_key
    account:
        username: your usrname
        password: your password
    app_key: bca7e84c2d947ac6 # 若账号密码方式无法登录可尝试更改此值
    appsec: 60698ba2f68e01ce44738920a0ffe768 

# b站上传线路选择，默认为自动模式，目前可手动切换为bda2, kodo, ws, qn
lines: AUTO
# b站提交接口，默认自动选择，可选web，client
submit_api: client
# 单文件并发上传数，未达到带宽上限时增大此值可提高上传速度
threads: 3
# 录像单文件大小限制，单位Byte，超过此大小分段下载
file_size: 2621440000
# 录像单文件时间限制，格式'00:00:00'（时分秒），超过此大小分段下载，如需使用大小分段请注释此字段
segment_time: '00:50:00'
# 如遇到斗鱼录制卡顿可以尝试切换线路，tct-h5（备用线路5），ali-h5（备用线路6），akm-h5（主线路1）
douyucdn: tct-h5
# 如遇到虎牙录制卡顿可以尝试切换线路，AL（阿里），BD（百度），TX（腾讯）
huyacdn: AL
# 如遇到哔哩哔哩录制跳帧可以尝试修改platform值，web（flv），h5（m3u8）
biliplatform: web

streamers:
    星际2Stats拔本神族天梯第一视角: # 最小配置示例
        url:
            - https://www.twitch.tv/kimdaeyeob3
    星际2INnoVation吕布卫星人族天梯第一视角: # 完整可选配置示例
        url:
            - https://www.twitch.tv/innovation_s2
            - https://www.panda.tv/1160340
        title: "星际2INnoVation吕布卫星人族天梯第一视角%Y-%m-%d" # 自定义标题的时间格式
        tid: 171 # 投稿分区码,174为生活，其他分区
        copyright: 2 # 1为自制
        cover_path: /cover/up.jpg
        description: 视频简介
        postprocessor: # 上传完成后，将按自定义顺序执行自定义操作
            - run: echo hello! # 执行任意命令，等同于在shell中运行,视频文件路径作为标准输入传入
            - run: python3 path/to/mail.py # 执行一个 Python 脚本，可以用来发送邮件等
            - mv: backup/ # 移动文件到backup目录下
#            - rm # 删除文件，为默认操作
        tags:
            - 星际争霸2
            - 电子竞技
        format: mp4 # 视频保存格式
        opt_args: # ffmpeg参数
            - '-ss' # 跳过开始的16秒
            - '00:00:16'

# 检测间隔时间，单位：秒
event_loop_interval: 40
# 相同平台检测间隔，单位：秒。不同平台的链接是并发的，不受此参数影响
checker_sleep: 15
# 线程池1大小，负责download事件
pool1_size: 3
# 线程池2大小，处理除download事件外所有其他事件
pool2_size: 3
# 检测源码文件变化间隔，单位：秒，检测源码到变化后，程序会在空闲时自动重启
check_sourcecode: 15
# 日志输出配置
LOGGING:
    formatters:
        verbose:
            format: '%(asctime)s %(filename)s[line:%(lineno)d](Pid:%(process)d Tname:%(threadName)s) %(levelname)s %(message)s'
            datefmt: '%Y-%m-%d %H:%M:%S'
        simple:
            format: '%(filename)s%(lineno)d[%(levelname)s]Tname:%(threadName)s %(message)s'
    handlers:
        console:
            level: DEBUG
            class: logging.StreamHandler
            formatter: simple
            stream: ext://sys.stdout
        file:
            level: DEBUG
            class: biliup.common.log.SafeRotatingFileHandler
            when: W0
            interval: 1
            backupCount: 1
            filename: ds_update.log
            formatter: verbose
    root:
        handlers: [ console ]
        level: INFO
    loggers:
        biliup:
            handlers: [ file ]
            level: INFO
# 默认通过网页接口上传,可选通过操作chrome上传,此时需要填写chromedriver路径
#chromedriver_path: /usr/local/bin/chromedriver
```

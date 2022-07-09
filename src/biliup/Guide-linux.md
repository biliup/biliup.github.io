# 快速上手-linux

本教程在https://blog.waitsaber.org/archives/129基础上修改

本文全程命令行操作，使用系统为 ubuntu-20.04-amd64，登录用户为root用户

- 更新软件源列表

``` shell
sudo apt-get update
```

- 安装python3-dev

```shell
sudo apt install python3-dev
```
中途需要确认直接回车或输入Y回车

确认安装成功，检查版本号

``` shell
python3 -V
```
显示版本号，可能略有差异
``` shell
Python 3.8.10
```

- 安装python3-pip

``` shell
sudo apt install python3-pip
```

中途需要确认直接回车或输入Y回车

确认安装成功，检查版本号

``` shell
pip3 -V
```

显示版本号，可能略有差异

``` shell
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

- 安装ffmpeg（可选，新版已经不需要）

```shell
sudo apt install ffmpeg
```
中途需要确认直接回车或输入Y回车

确认安装成功，检查版本号

``` shell
ffmpeg -version
```

显示版本号，可能略有差异

``` shell
以下是第一行返回内容
ffmpeg version 4.2.7-0ubuntu0.1 Copyright (c) 2000-2022 the FFmpeg developers
……内容较多略过……
```

- 安装nodejs

```shell
sudo apt install nodejs
```
中途需要确认直接回车或输入Y回车

确认安装成功，检查版本号

``` shell
node -v
```

显示版本号，可能略有差异

``` shell
v10.19.0
```

- 安装biliup

```shell
sudo pip3 install biliup 
```

下载迟缓可以尝试以下命令

``` shell
sudo pip3 install biliup -i https://mirrors.aliyun.com/pypi/simple
```

确认安装成功，检查版本号

``` shell
biliup --version
```

显示版本号，可能略有差异

``` shell
v0.3.0
```

- 在保存文件的目录下创建配置文件

本文以“**/home**”文件夹为例子，其他文件夹请自行替换

此目录将保存录播文件、配置文件、登录文件，配置文件本文以toml格式为例，并使用最小配置，biliup同时支持yaml格式，yaml和完整配置后续会出教程

- 下载biliup-rs

https://github.com/ForgQi/biliup-rs/releases 查看最新版本根据系统和架构选择对应文件，本文使用的服务器为linux x86_64

``` shell
cd /home
wget -O biliupR.tar.xz https://github.com/ForgQi/biliup-rs/releases/download/v0.1.9/biliupR-v0.1.9-x86_64-linux.tar.xz
#无法连接可尝试下方镜像
wget -O biliupR.tar.xz https://ghproxy.futils.com/https://github.com/ForgQi/biliup-rs/releases/download/v0.1.9/biliupR-v0.1.9-x86_64-linux.tar.xz
#无法连接可尝试下方镜像
wget -O biliupR.tar.xz https://ghproxy.com/https://github.com/ForgQi/biliup-rs/releases/download/v0.1.9/biliupR-v0.1.9-x86_64-linux.tar.xz
#解压文件
tar -xvf biliupR.tar.xz
#移动文件
mv -fb ./biliupR*/* ./
#删除不必要文件
rm -rf ./biliupR*
```


```shell
sudo touch /home/config.toml
```

使用vi命令编辑文件

```shell
sudo vi /home/config.toml
```

按“**i**”进入插入模式，输入配置项,井号及井号后为注释可以不写

```toml
[streamers."直播录像"]# 设置自定义名称
url = ["https://live.bilibili.com/000000"]# 设置直播间url网址
tags = ["biliup"]# 设置投稿时添加的tag标签
```

输入完成后按“**ESC**”进入编辑模式，输入"**:wq**"保存并退出。

vi编辑器对于小白不太友好，后续会出其他方式编辑的教程

- 登录B站投稿账号

``` shell
cd /home
./biliup login
```

建议选择扫码登录或者浏览器登录

浏览器登录：将显示的复制到浏览器进行登录，建议新开无痕窗口进行

扫码登录：会在终端上显示二维码，可能由于部分终端设置行距导致无法扫码

- 启动biliup

``` shell
cd /home
biliup start
```

启动后没有提示表示运行没有报错

查看进程是否启动成功

``` shell
ps -A | grep biliup
```

查看目录文件

``` shell
ls -lh
```
``` shell
total 100M
-rw-rw-rw- 1 root root  96M Jul  9 23:30 直播录像2022-07-09T23_26_12.flv.part
-rwxr-xr-x 1 1001  121 4.5M Jun  3 22:40 biliup
-rw-r--r-- 1 root root   93 Jul  9 22:49 config.toml
-rw-r--r-- 1 root root 1.3K Jul  9 23:23 cookies.json
-rw-rw-rw- 1 root root    0 Jul  9 23:26 download.log
-rw-r--r-- 1 root root  122 Jul  9 23:26 ds_update.log
-rw-r--r-- 1 root root 4.4K Jul  9 23:22 qrcode.png
-rw-rw-rw- 1 root root    6 Jul  9 23:26 watch_process.pid
```
可以看到.log日志文件，如果此时正在开播，可以看到录像文件，并重复运行命令可以看到文件大小在增加

- 其他命令

``` shell
# 在创建配置文件的目录启动 biliup
$ biliup start
# 退出
$ biliup stop
# 重启
$ biliup restart
# 查看版本
$ biliup --version
# 显示帮助以查看更多选项
$ biliup -h
# 启动 web ui, 默认 0.0.0.0:19159。 可使用-H及-P选项配置。考虑到安全性，建议指定本地地址配合web server或者添加验证。
$ biliup --http start
# 指定配置文件路径
$ biliup --config ./config.yaml start
```


# INSTALLATION
## 通用安装方法
需要`Python 3.8`及以上版本，相关配置示例在config.yaml文件中，如直播间地址，b站账号密码
1. 安装 __FFmpeg__, __pip__
2. 安装 __biliup__：`pip3 install biliup`
## Linux安装
Linux下程序以daemon进程启动，录像和日志文件保存在执行目录下，程序执行过程可查看日志文件。
`ps -A | grep biliup` 查看进程是否启动成功。

详细操作过程可看 [@waitsaber](https://github.com/waitsaber) 写的教程：
* [Ubuntu](https://blog.waitsaber.org/archives/129) 
* [CentOS](https://blog.waitsaber.org/archives/163)


## Docker使用 🔨
### 方式一
```bash
vim /host/path/config.yaml
docker run --name biliup -v /host/path:/opt -d ghcr.io/forgqi/biliup/caution
```
### 方式二
```bash
cd biliup
sudo docker build . -t biliup
sudo docker run -d biliup
```
### 进入容器 📦
```bash
sudo docker ps (找到你的imageId)
sudo docker exec -it imageId /bin/bash     
```

# livego
简单高效的直播服务器：
- 安装和使用非常简单；
- 纯 Golang 编写，性能高，跨平台；
- 支持常用的传输协议、文件格式、编码格式；

#### 支持的传输协议
- RTMP
- AMF
- HLS
- HTTP-FLV

#### 支持的容器格式
- FLV
- TS

#### 支持的编码格式
- H264
- AAC
- MP3

## 安装
直接下载编译好的[二进制文件](https://github.com/gwuhaolin/livego/releases)后，在命令行中执行。

#### 从 Docker 启动
执行`docker run -p 1935:1935 -p 7001:7001 -p 7002:7002 -d --name livego gwuhaolin/livego`启动

#### 从源码编译
首先确保系统已经具备golang环境，环境部署可以参考：https://golang.google.cn/dl/
1. 下载源码 `git clone https://github.com/gwuhaolin/livego.git`
2. 去 livego 目录中 执行 `go build`（如果执行报错提示proxy.golang.org访问超时可以在运行前执行“export GOPROXY=https://goproxy.io”）

## 使用
2. 启动服务：执行 `livego` 二进制文件启动 livego 服务；
3. 上行推流：通过 `RTMP` 协议把视频流推送到 `rtmp://localhost:1935/live/movie`，例如使用 `ffmpeg -re -i demo.flv -c copy -f flv rtmp://localhost:1935/live/movie` 推送；
4. 下行播放：支持以下三种播放协议，播放地址如下：
    - `RTMP`:`rtmp://localhost:1935/live/movie`
    - `FLV`:`http://127.0.0.1:7001/live/movie.flv`
    - `HLS`:`http://127.0.0.1:7002/live/movie.m3u8`

## 执行参数
livego具备指定参数运行：
Usage of ./livego:
  -cfgfile string
        configure filename (default "livego.cfg")
  -filFile string
        output flv file name (default "./out.flv")
  -gopNum int
        gop num (default 1)
  -hls-addr string
        HLS server listen address (default ":7002")
  -httpflv-addr string
        HTTP-FLV server listen address (default ":7001")
  -manage-addr string
        HTTP manage interface server listen address (default ":8090")
  -readTimeout int
        read time out (default 10)
  -rtmp-addr string
        RTMP server listen address (default ":1935")
  -writeTimeout int
        write time out (default 10)

## 已服务形式运行livego
1.添加livego运行服务文件:vi /etc/systemd/system/livego.service

[Unit]
Description=livego

[Service]
Restart=always
RestartSec=3
Type=simple
SyslogIdentifier=livego
PermissionsStartOnly=true
ExecStart=/opt/livego/livego -cfgfile /opt/livego/livego.cfg 

[Install]
WantedBy=multi-user.target

2.设定livego开机启动
systemctl daemon-reload
sudo systemctl enable livego
sudo systemctl start livego

3.查看服务状态
sudo systemctl status livego

● livego.service - livego
   Loaded: loaded (/etc/systemd/system/livego.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2019-10-30 17:28:13 CST; 21min ago
 Main PID: 88531 (livego)
   CGroup: /system.slice/livego.service
           └─88531 /opt/livego/livego -cfgfile /opt/livego/livego.cfg

Oct 30 17:28:13 centos-7-x64-206 livego[88531]: "hlson":"on"
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: }
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: ]
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: }
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 liveconfig.go:49: get config json d...]}]}
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 main.go:62: hls server enable....
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 main.go:70: RTMP Listen On :1935
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 main.go:105: HTTP-Operation listen ...8090
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 main.go:43: HLS listen On :7002
Oct 30 17:28:13 centos-7-x64-206 livego[88531]: 2019/10/30 17:28:13 main.go:87: HTTP-FLV listen On :7001
Hint: Some lines were ellipsized, use -l to show in full.


### [和 flv.js 搭配使用](https://github.com/gwuhaolin/blog/issues/3)

对Golang感兴趣？请看[Golang 中文学习资料汇总](http://go.wuhaolin.cn/)

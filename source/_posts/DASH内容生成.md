---
layout: poster
title: DASH内容生成
date: 2019-07-03 22:36:55
tags:
    - DASH
    - ffmpeg
    - mp4box
    - gpac
categories: Programming
---
示例中big_buck_bunny的不同分辨率设置 https://dash.akamaized.net/akamai/bbb_30fps/bbb_30fps.mpd

254 kbps 320x180
507 kbps 320x180
759 kbps 480x270
1013 kbps 640x360
1254 kbps 640x360
1883 kbps 768x432
3134 kbps 1024x576
4952 kbps 1280x720
9914 kbps 1920x1080
14931 kbps 3840x2160

centos服务器安装ffmpeg
https://linuxize.com/post/how-to-install-ffmpeg-on-centos-7/
centos服务器安装gpac
```
sudo yum install gpac
```

转码
```
ffmpeg -i big_buck_bunny_1080p_stereo.avi -s 3840x2160  -c:v libx264 -b:v 15000k -g 24 -an big_buck_bunny_3840x2160_15000k.mp4
```
切片到dash

windows下
```
mp4box -dash 2000 -frag 2000 -rap -frag-rap -profile dashavc264:live -segment-name Video\$RepresentationID\$\segment -mpd-title Joker -out manifest.mpd big_buck_bunny_320x180_250k.mp4 big_buck_bunny_320x180_500k.mp4 big_buck_bunny_480x270_750k.mp4 big_buck_bunny_640x360_1000k.mp4 big_buck_bunny_640x360_1250k.mp4 big_buck_bunny_768x432_2000k.mp4 big_buck_bunny_1024x576_3000k.mp4 big_buck_bunny_1280x720_5000k.mp4 big_buck_bunny_1920x1080_10000k.mp4 big_buck_bunny_3840x2160_15000k.mp4 
```

release_1
```
mp4box -dash 2000 -frag 2000 -rap -frag-rap -profile dashavc264:live -segment-name Video$RepresentationID$\segment -mpd-title Joker -out manifest.mpd big_buck_bunny_320x180_250k.mp4 big_buck_bunny_320x180_500k.mp4 big_buck_bunny_480x270_750k.mp4 big_buck_bunny_640x360_1000k.mp4 big_buck_bunny_640x360_1250k.mp4 big_buck_bunny_768x432_1600k.mp4 big_buck_bunny_768x432_2000k.mp4 big_buck_bunny_1024x576_2500k.mp4 big_buck_bunny_1024x576_3000k.mp4 big_buck_bunny_1280x720_3500k.mp4
```

centos下：(不能用 原因是centos中gpac版本低，RepresentationID是视频名，与最新版本不同)
```
MP4Box -dash 2000 -frag 2000 -rap -frag-rap -profile dashavc264:live -segment-name Video\$RepresentationID\$/segment -mpd-title Joker -out manifest.mpd big_buck_bunny_320x180_250k.mp4 big_buck_bunny_320x180_500k.mp4 big_buck_bunny_480x270_750k.mp4 big_buck_bunny_640x360_1000k.mp4 big_buck_bunny_640x360_1250k.mp4 big_buck_bunny_768x432_2000k.mp4 big_buck_bunny_1024x576_3000k.mp4 big_buck_bunny_1280x720_5000k.mp4 big_buck_bunny_1920x1080_10000k.mp4 big_buck_bunny_3840x2160_15000k.mp4 
```

dash player链接
https://get-up.cn/dash_server/dash.js/samples/dash-if-reference-player/

big_buck_bunny mpd文件链接
https://get-up.cn/dash_video/gop=24/manifest.mpd
https://get-up.cn/dash_video/release_1/manifest.mpd

RTT trace
```
ping -i 0.2 bilibili.com -D -c 1800000 >ping_bilibili.log  
```
37ms 
```
ping -i 0.2 iqiyi.com -D -c 1800000 >ping_iqiyi.log
```
26ms
```
ping -i 0.2 youku.com -D -c 1800000 >ping_youku.log
```
30ms
```
ping -i 0.2 v.qq.com -D -c 1800000 >ping_tencent.log
```
40ms
```
ping -i 0.2 ieeexplore.ieee.org -D -c 1800000 >ping_ieee.log
```
220ms
```
ping -i 0.2 zhihu.com -D -c 1800000 >ping_zhihu.log   
```
12ms
```
ping -i 0.2 nus.edu.sg -D -c 1800000 >ping_sg.log  
```
300ms
```
ping -i 0.2 www.mtv.com.tw -D -c 1800000 >ping_twmusic.log  
```
80ms
```
ping -i 0.2 www.sonymusic.co.jp -D -c 1800000 >ping_sonyjp.log  
```
100ms

```
ping -i 0.2 zhihu.com -D -c 1800000 >ping_zhihu.log   
```
12ms
```
ping -i 0.2 iqiyi.com -D -c 1800000 >ping_iqiyi.log
```
26ms
```
ping -i 0.2 v.qq.com -D -c 1800000 >ping_tencent.log
```
40ms
```
ping -i 0.2 www.mtv.com.tw -D -c 1800000 >ping_twmusic.log  
```
80ms
```
ping -i 0.2 www.sonymusic.co.jp -D -c 1800000 >ping_sonyjp.log  
```
100ms
```
ping -i 0.2 ieeexplore.ieee.org -D -c 1800000 >ping_ieee.log
```
220ms
```
ping -i 0.2 nus.edu.sg -D -c 1800000 >ping_sg.log  
```
300ms
```
ping -i 0.2 bilibili.com -D -c 1800000 >ping_bilibili.log  
```
37ms

wsl下文件目录
```
cd /mnt/d/My_Files/研一下/视频_多路径_RL/DataSet/Ping_Dataset
```

linux下tcp抓包
```
tcpdump tcp and host 223.104.34.17 -w /home/wwwroot/default/tcpdump_data/dash_LTE.cap
```

Linux其他命令
将一个名为abc.txt的文件重命名为1234.txt
```
[root@station90 root]#mv abc.txt 1234.txt
```

更新wsl
```
sudo apt update && sudo apt upgrade
```

更换中科大源
编辑/etc/apt/sources.list文件, 在文件最前面添加以下条目(操作前请做好相应备份)：
备份
```
sudo cp /etc/apt/sources.list /etc/apt/sources_init.list
```
更换源
```
sudo gedit /etc/apt/sources.list
```

sources.list添加内容
```
##中科大源

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

腾讯云主机tcp报文大于MTU值原因，同时也是为什么中断ACK一般报文的原因

Probably you captured on the host that transmitted the oversized packet, and TCP Large Segment Offload is enabled. (Sometimes abbreviated TSO and sometimes LSO.) The operating system is passing packets larger than MTU to the network adapter, and the network adapter driver is breaking them up so that they fit within the MTU. If you capture from the wire, instead of from an endpoint involved in the communication, you will see that the packets are correctly sized when they are transmitted. This is one reason of several to capture from the wire, instead of on an endpoint.

可能是您在传输超大数据包的主机上捕获，并启用了TCP大段卸载。 （有时缩写为TSO，有时缩写为LSO。）操作系统将大于MTU的数据包传递给网络适配器，网络适配器驱动程序将它们分解，以便它们适合MTU。 如果从线路捕获，而不是从通信中涉及的端点捕获，您将看到数据包在传输时的大小正确。 这是几个从线路捕获而不是在端点上捕获的原因之一。

Enable or Disable TSO on a Linux Virtual Machine
```
ethtool -K ethY tso on

ethtool -K ethY tso off
```
时间同步服务器设置
[win 10 解决办法](http://www.xitongzhijia.net/xtjc/20170822/105357.html)

centos
```
ntpdate ntp.sjtu.edu.cn && hwclock -w
```
hwclock -w命令是将系统时间，写入BOIS

如果出现the NTP socketis in use, exiting
解决办法
```
service ntp stop
ntpdate ntp.sjtu.edu.cn && hwclock -w
```

杀死tcp connection 
```
tcpkill -9 host 114.214.166.200
```
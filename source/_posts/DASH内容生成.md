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
sudo yum install gpac

转码
ffmpeg -i big_buck_bunny_1080p_stereo.avi -s 3840x2160  -c:v libx264 -b:v 15000k -g 24 -an big_buck_bunny_3840x2160_15000k.mp4

切片到dash

windows下
mp4box -dash 2000 -frag 2000 -rap -frag-rap -profile dashavc264:live -segment-name Video$RepresentationID$\segment -mpd-title Joker -out manifest.mpd big_buck_bunny_320x180_250k.mp4 big_buck_bunny_320x180_500k.mp4 big_buck_bunny_480x270_750k.mp4 big_buck_bunny_640x360_1000k.mp4 big_buck_bunny_640x360_1250k.mp4 big_buck_bunny_768x432_2000k.mp4 big_buck_bunny_1024x576_3000k.mp4 big_buck_bunny_1280x720_5000k.mp4 big_buck_bunny_1920x1080_10000k.mp4 big_buck_bunny_3840x2160_15000k.mp4 

centos下：(不能用 原因是centos中gpac版本低，RepresentationID是视频名，与最新版本不同)
MP4Box -dash 2000 -frag 2000 -rap -frag-rap -profile dashavc264:live -segment-name Video$RepresentationID$/segment -mpd-title Joker -out manifest.mpd big_buck_bunny_320x180_250k.mp4 big_buck_bunny_320x180_500k.mp4 big_buck_bunny_480x270_750k.mp4 big_buck_bunny_640x360_1000k.mp4 big_buck_bunny_640x360_1250k.mp4 big_buck_bunny_768x432_2000k.mp4 big_buck_bunny_1024x576_3000k.mp4 big_buck_bunny_1280x720_5000k.mp4 big_buck_bunny_1920x1080_10000k.mp4 big_buck_bunny_3840x2160_15000k.mp4 
---
layout:     post
title:      "我的conky安装及配置——基于 ubuntu 16.04"
subtitle:   " \"conky\""
date:       2017-07-17 12:00:00
author:     "slwang"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Linux
---
#我的conky安装及配置——基于 ubuntu 16.04
> “Yeah,I just do it ”


## 前言
本人的第一篇博客终于正式上线，也不枉我这两天的辛苦。建站期间经历的种种，真是让人心累啊！好了，闲话休烦，步入正题。
<p id = "build"></p>
---

## conky简介

Conky是一种自由软件，用于X视窗系统的系统监视，可以在FreeBSD、OpenBSD和各种Linux发布上使用的自由软件。Conky具有很高的可配置性，可以监视许多系统参数，如：CPU、内存、交换内存、硬盘使用情况等状态；各种硬件的温度；系统的进程（top）；网络状态；电池电量；系统信息和邮件收发；各种音乐播放器MPD、XMMS2、BMPx、Audacious）的控制。不像其他系统监视器那样需要高级别的部件工具箱（widget toolkits）来渲染他们的信息，Conky可以直接在X视窗下渲染，这意味着在相同配置下Conky可以消耗更少的资源。

## conky 安装

一条命令足以

    sudo apt install conky-all

## conky配置

原装的conky感觉什么丑陋，而且功能也没完全发挥出来。所以要对conky进行配置。网上有很多酷炫的conky界面，而我还是比较喜欢简洁的画风。所以我的桌面就是这样的：

![2017-07-17 21-08-27屏幕截图.png-930.2kB][1]


那么该怎么做呢？
打开终端，输入一下内容：（conkyrc即conky配置文件）

    sudo gedit ~/.conkyrc
开始配置，附上我的配置文件
```java
# set to yes if you want Conky to be forked in the background
background yes
cpu_avg_samples 2
net_avg_samples 2
out_to_console no
# X font when Xft is disabled, you can pick one with program    xfontsel
#font 7x12
#font 6x10
#font 7x13
#font 8x13
#font 7x12
#font *mintsmild.se*
#font -*-*-*-*-*-*-34-*-*-*-*-*-*-*
#font -artwiz-snap-normal-r-normal-*-*-100-*-*-p-*-iso8859-1
# Use Xft?
use_xft yes
# Xft font when Xft is enabled
xftfont Sans:size=9
own_window_argb_visual yes
#own_window_colour hotpink
# Text alpha when using Xft
xftalpha 0.8
# on_bottom yes
# mail spool
mail_spool $MAIL
# Update interval in seconds
update_interval 1
# Create own window instead of using desktop (required in nautilus)
own_window yes
own_window_transparent yes
own_window_hints undecorated,below,sticky,skip_taskbar,skip_pager
#own_window_type override
own_window_type normal
# Use double buffering (reduces flicker, may not work for everyone)
double_buffer yes
# Minimum size of text area
minimum_size 100 3
maximum_width 308
# Draw shades?
draw_shades no
# Draw outlines?
draw_outline no
# Draw borders around text
draw_borders no
# Stippled borders?
stippled_borders no
# border margins
border_margin 4
# border width
border_width 0
# Default colors and also border colors
default_color white
default_shade_color white
default_outline_color white
# Text alignment, other possible values are commented
#alignment top_left
minimum_size 10 10
gap_x 20
gap_y 30
alignment top_right
#alignment bottom_left
#alignment bottom_right
# Add spaces to keep things from moving about?  This only affects certain objects.
use_spacer none
# Subtract file system buffers from used memory?
no_buffers yes
# set to yes if you want all text to be in uppercase
uppercase no
# none, xmms, bmp, audacious, infopipe (default is none)
# xmms_player bmp



TEXT
${color white} ${font PizzaDude Bullets:size=16}S${font :size=10}   DATE $hr $font $color
${goto 1000}${time %l:%M}${font}${time %p}
${time %A, %e %B %Y}

${color white}${font OpenLogos:size=18}L${font :size=10}   SYSTEM $hr $font
$color $font$sysname: ${alignr} $kernel ($machine)
$color Hostname: $alignr$nodename
$color Uptime: $alignr$uptime
$color System load: $alignr $loadavg
$color Running Processes: $alignr $running_processes/$processes
$color Running Threads: $alignr $running_threads/$threads
$color Temp: ${alignr}${acpitemp}°C

${color white} ${font StyleBats:size=16}W${font :size=10}   CPU $hr $font $color
$color ${font}CPU@${freq}MHz: ${cpu cpu1}% ${alignr}${cpubar cpu1 8,60}
${color white}Top CPU $alignr PID $alignr $alignc CPU% $alignr MEM%
$color${top name 1} $alignr ${top pid 1} $alignr ${top cpu 1} $alignr ${top mem 1}
$color${top name 2} $alignr ${top pid 2} $alignr ${top cpu 2} $alignr ${top mem 2}
$color${top name 3} $alignr ${top pid 3} $alignr ${top cpu 3} $alignr ${top mem 3}

${color white}${font StyleBats:size=16}A${font :size=10}   MEMORY $hr $font $color
${color}${font}RAM: $mem/$memmax $alignr $memperc%
$color ${membar}
${color}${font}Swap: $swap/$swapmax $alignr $swapperc%
$color ${swapbar}
${color white}Top Mem $alignr PID $alignr CPU% $alignr MEM%
$color${top_mem name 1}$alignr${top_mem pid 1}$alignr${top_mem cpu 1}$alignr${top_mem mem 1}
$color${top_mem name 2}$alignr${top_mem pid 2}$alignr${top_mem cpu 2}$alignr${top_mem mem 2}
$color${top_mem name 3}$alignr${top_mem pid 3}$alignr${top_mem cpu 3}$alignr${top_mem mem 3}

${color white} ${font PizzaDude Bullets:size=16}N${font :size=10}   HDD $hr $font $color
$color home: ${fs_used /home}/${fs_size /home} $alignr ${fs_free_perc /home}%
$color ${fs_bar /home}

${color white} ${font PizzaDude Bullets:size=16}G${font :size=10}   NETWORK $hr $font $color
$color Local IP: $alignr${addr enp2s0}
$color Down ${downspeed enp2s0}/s ${alignr}UpLoad ${upspeed enp2s0}/s
${downspeedgraph enp2s0 25,107} ${alignr}${upspeedgraph enp2s0 25,107}
$color Total ${totaldown enp2s0} ${alignr}Total ${totalup enp2s0}
```
使用该配置时会出现退到桌面时conky消失的现象（具体原因不明），但关闭切换器的“显示桌面图标”就好了。
## conky开机自启动
在dash里找启动应用程序。。。如图

![2017-07-17 22-16-37屏幕截图.png-337.4kB][2]


在之后出来的对话框中点击添加按钮，然后按照如图设置即可～～完成自启

![2017-07-17 22-18-38屏幕截图.png-22.1kB][3]


  [1]: http://static.zybuluo.com/wangsl754/nznljsscqse7eqf5ojmrigzw/2017-07-17%2021-08-27%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png
  [2]: http://static.zybuluo.com/wangsl754/lgga4henlzv3wdvft3syl6y0/2017-07-17%2022-16-37%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png
  [3]: http://static.zybuluo.com/wangsl754/j2nitull5eua40mjemgjdk5k/2017-07-17%2022-18-38%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png

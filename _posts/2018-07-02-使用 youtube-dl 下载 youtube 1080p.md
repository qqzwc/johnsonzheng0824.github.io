---
layout: post
title: "使用 youtube-dl 下载 Youtube 1080p"
date: 2018-07-02
tag: Youtube
---   

## Step1 下载与安装 Python
打开 cmd，再输入 python 并回车执行，如果出现如下界面，则代表安装成功

![](/images/posts/youtube/python.png)

## Step2 安装 youtube-dl
从官网下载 [youtube-dl.exe](http://www.youtube-dl.org/) 然后放在电脑的任意目录下通过 CMD 进入那个目录即可使用;

可以通过下面的命令来更新 youtube-dl
```
youtube-dl -U
```

## Step3 安装 FFmpeg
1. 进入 [FFmpeg](https://ffmpeg.zeranoe.com/builds/) 下载页面，根据自己的操作系统选择下载最新的 32 位或 64 位静态程序版本

2. 在环境变量>系统变量 里找到Path,点击编辑>新建,然后把刚才那个文件夹里的 bin 路径复制到这里

3. 打开 cmd, 回车, 输入以下命令:
   
   `ffmpeg -version`
   
   如果出现如下图所示的版本号信息,则表明FFmpeg安装成功了，你可以在命令提示行中任意文件夹下运行 FFmpeg

    ![](/images/posts/youtube/ffmpeg.png)

## 将系统代理模式改成"全局模式"

## 下载 YouTube 视频

- 查看视频所有类型,只看不下载<br>
  `youtube-dl -F [url]`<br>
  ![](/images/posts/youtube/list.png)

- 下载指定质量的视频和音频并自动合并
  youtube-dl -f [format code] [url]
  通过上一步获取到了所有视频格式的清单，最左边一列就是编号对应着不同的格式.
  由于 YouTube 的 1080p 及以上的分辨率都是音视频分离的, 所以我们需要分别下载视频和音频, 可以使用 137+140 这样的组合.
  如果系统中安装了 ffmpeg 的话, youtube-dl 会自动合并下下好的视频和音频, 然后自动删除单独的音视频文件
  
  ![](/images/posts/youtube/download.png)

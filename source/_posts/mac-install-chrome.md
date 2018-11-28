---
title: mac 安装 chrome
date: 2018-07-21 11:34:22
tags:
  - iso
---

# 将允许安装来源设置为`任何来源`

打开终端运行以下命令

    sudo spctl --master-disable

打开 `系统偏好设置 > 安全性与隐私` 可看到下图中的任何来源被选中。

![iso setting](/images/iso-spctl-1.jpg)

如果要关闭，可允许以下命令

    sudo spctl --master-enable

打开 `系统偏好设置 > 安全性与隐私` 可看到下图中的任何来源已经消失了。

![iso setting](/images/iso-spctl-2.jpg)

# 下载 chrome

> https://chrome.en.softonic.com/mac/download

# 安装

点击下载好的文件即可安装。

安装好后可将允许任何来源安装关闭，并不会影响后续使用。
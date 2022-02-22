# Android studio在中国大陆无法下载/使用等问题
(主要采用的是更改hosts文件方案)

host文件在C:\Windows\System32\drivers\etc下，
先将文件拖到桌面上，右击桌面上那个用记事本打开，
编辑保存后，拖回到原位置，选择替换文件。

## 第一：打开-> http://ping.chinaz.com/ <-这个网站

## 第二：在网站内对 dl.google.com 进行ping检测
[![Hb7bLj.png](https://s4.ax1x.com/2022/02/19/Hb7bLj.png)](https://imgtu.com/i/Hb7bLj)


## 第三：检查完成之后，复制响应时间快的相应IP
[![Hb7HyQ.png](https://s4.ax1x.com/2022/02/19/Hb7HyQ.png)](https://imgtu.com/i/Hb7HyQ)

## 第四：将相应IP + dl.google.com 粘贴到 hosts文件下方
[![Hb77Qg.png](https://s4.ax1x.com/2022/02/19/Hb77Qg.png)](https://imgtu.com/i/Hb77Qg)



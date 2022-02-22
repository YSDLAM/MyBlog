# 深浅色模式切换可能遇到问题及解决方法

***

目前就我遇到的问题,就是当浅色模式下设置状态栏颜色为纯白时#ffffff

[![H03IEt.png](https://s4.ax1x.com/2022/02/12/H03IEt.png)](https://imgtu.com/i/H03IEt)

会与状态栏字体颜色重合,导致状态栏一片白,里面的时间等信息也就看不到了,这并不是我们的本意.

[![H03LvQ.png](https://s4.ax1x.com/2022/02/12/H03LvQ.png)](https://imgtu.com/i/H03LvQ)

网上搜索也找不到什么解决方法,思来想去睡不着,于是求助了一下别人.

具体方法如下:

```
如果sdk>=23,可以

<item name="android:windowLightStatusBar">true</item>
```

就是将代码复制到values下的theme.xml当中,然后按下override resource in values-v23,android studio会自动帮我们创建一个values-v23文件夹,然后删除values/theme.xml刚才复制的内容,当sdk>=23,就会调用这个文件夹里的文件,android:windowLightStatusBar就会起作用.

[![H0J8AI.png](https://s4.ax1x.com/2022/02/12/H0J8AI.png)](https://imgtu.com/i/H0J8AI)

我们再运行一下

[![H0tBYq.png](https://s4.ax1x.com/2022/02/12/H0tBYq.png)](https://imgtu.com/i/H0tBYq)

刚才的问题也就解决了


# Android深/浅色模式的切换(手动)

***

上一篇文章介绍了如何自动切换深/浅色模式,所谓的自动切换就是安卓系统打开暗色模式,根据安卓系统是否打开暗色模式来确定调用哪个文件(values/values-night)

## 现在我们来介绍一下如何手动切换(根据用户的选择,而非安卓系统)

新建一个安卓项目,在layou/activity_mian.xml布局文件下创建三个按钮,分别是日间模式,夜间模式与跟随系统,等会我们按下这三个按钮将实现对应的功能

[![HaU59K.png](https://s4.ax1x.com/2022/02/11/HaU59K.png)](https://imgtu.com/i/HaU59K)

然后再MainActivity.java文件,写下如下代码,点击运行,我们会发现,虽然APP界面的深浅色根据用户的选择来变化,而不是跟随系统.

[![HawXNR.png](https://s4.ax1x.com/2022/02/11/HawXNR.png)](https://imgtu.com/i/HawXNR)

我们主要依靠的就是 AppCompatDelegate.setDefaultNightMode() ,通过它来实现切换,里面的参数可以填一下四个,根据需求选择.

```
- 浅色 - MODE_NIGHT_NO
- 深色 - MODE_NIGHT_YES
- 由省电模式设置 - MODE_NIGHT_AUTO_BATTERY
- 系统默认 - MODE_NIGHT_FOLLOW_SYSTEM
```
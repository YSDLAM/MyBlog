# 安卓开发当中实现手机震动功能

***

主要分为3步

## 第一:开启震动权限
(这一步忘记也没有关系，Android studio会提示我们，点一下帮我们导入)

代码写在AndroidManifest.xml里面，我们的权限命令都写在这个文件里



```
<uses-permission android:name="android.permission.VIBRATE"/>
```

[![HoXNzF.png](https://s4.ax1x.com/2022/02/18/HoXNzF.png)](https://imgtu.com/i/HoXNzF)


## 第二：获取系统服务
(写在你想要开启震动功能的Activity里,这个this要注意,未必所有情况都是this)

```
Vibrator vibrator = (Vibrator)this.getSystemService(VIBRATOR_SERVICE);
```

[![Hojy0s.png](https://s4.ax1x.com/2022/02/18/Hojy0s.png)](https://imgtu.com/i/Hojy0s)

## 第三：使用震动
(个人建议震感一定要上真机慢慢调试出来)
(另外震动一般作为一个选项，可开可关，最好不要写绝，灵活一点)

```
vibrator.vibrate(1000);
也可以
vibrator.vibrate(new long[]{0, 5, 10, 500}, -1);
数组参数意义：
第一个参数为等待指定时间后开始震动，震动时间为第二个参数。
后边的参数依次为等待震动和震动的时间。
第二个参数为重复次数，-1为不重复，0为一直震动
```

[![HovxP0.png](https://s4.ax1x.com/2022/02/18/HovxP0.png)](https://imgtu.com/i/HovxP0)
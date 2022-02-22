# 安卓本地化设置
这里的本地化设置主要指的是自己开发的软件显示的语言与系统语言一致

在[深浅色模式的切换](part_1.md)这篇文章当中提到了两个资源文件夹，分别是values与values-night,浅色模式下会自动使用values里的文件，深色模式则会自动使用values-night下的文件。

同样语言也是一样，刚创建的项目在values文件夹下有一个string.xml文件，这里面就是定义文字资源的的，打开这个文件点击右上角的open editor,会调到这个语言翻译工具，然后点击左上角哪

个小地球来添加你想支持的语言，这里我选择Chinese in China，Android studio会自动帮我们生成values-zh-rCN资源文件夹，里面strings.xml文件就是当系统语言为简体时自动调用的！

以后我们定义控件的text或者其他需要变化的文字就要注意，最好不要写死比如：

```
android:text="你好"
```

建议做法

```
先在values/strings.xml定义一个资源名，比如

<string name="show">Hello World</string>

values-zh-rCN/strings.xml也要与其对应

<string name="show">你好 世界</string>

android:text="@string/show"
```

[![HTWyE4.png](https://s4.ax1x.com/2022/02/18/HTWyE4.png)](https://imgtu.com/i/HTWyE4  ':size=10%')

[![HTWBuT.png](https://s4.ax1x.com/2022/02/18/HTWBuT.png)](https://imgtu.com/i/HTWBuT ':size=10%')

[![HTWDDU.png](https://s4.ax1x.com/2022/02/18/HTWDDU.png)](https://imgtu.com/i/HTWDDU ':size=10%')

[![HTWrbF.png](https://s4.ax1x.com/2022/02/18/HTWrbF.png)](https://imgtu.com/i/HTWrbF ':size=10%')

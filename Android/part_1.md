# Android开发实现深/浅色模式的切换(自动切换)

***

首先用Android Studio新建一个项目,它的目录结构是这样的

<!-- ![logo](_media/part_1/icon_1.png ':size=10%') -->
[![Ha1gvn.png](https://s4.ax1x.com/2022/02/11/Ha1gvn.png)](https://imgtu.com/i/Ha1gvn)

我们看到有两个资源文件夹,分别是values与values-night

* values         里面的文件是浅色模式调用的
* values-night   里面的文件是暗色模式调用

这个是系统自动调用的,无需手动设置

注意:如何删除values-night资源文件夹,那么软件就不支持暗色模式

例如阿里系的部分应用,系统深色浅色对其根本不起效果,可能是根本就没有这个文件夹

我们打开分别打开values与values-night下的themes.xml


* values/themes.xml
```
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.MyApplication" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```

* values-night/themes.xml
```
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.MyApplication" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_200</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/black</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_200</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```

就比如说这个
```
<item name="colorPrimary">@color/purple_200</item>
    colorPrimary决定着Appbar的背景色
```
大家可以参照这一张表

![logo](http://www.aoaoyi.com/wp-content/uploads/2017/01/android_colorPrimary.jpg)

具体可以参照
```
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">

    <!--状态栏颜色，应用的主要暗色调，statusBarColor默认使用该颜色-->
    <item name="android:colorPrimaryDark">@color/material_animations_primary_dark</item>
    <!--状态栏颜色，默认使用colorPrimaryDark-->
    <item name="android:statusBarColor">@color/material_animations_primary_dark</item>
    
    <!--Appbar背景色，应用的主要色调，actionBar默认使用该颜色-->
    <item name="android:colorPrimary">@color/material_animations_primary</item>
    
    <!--页面背景色-->
    <item name="android:windowBackground">@color/light_grey</item>
    
    <!--底部导航栏颜色-->
    <item name="android:navigationBarColor">@color/navigationColor</item>
    
    <!--应用的主要文字颜色，actionBar的标题文字默认使用该颜色-->
    <item name="android:textColorPrimary">@android:color/black</item>
    
    <!--ToolBar上的Title颜色-->
    <item name="android:textColorPrimaryInverse">@color/text_light</item>
    
    <!--应用的前景色，ListView的分割线，switch滑动区默认使用该颜色-->
    <item name="android:colorForeground">@color/colorForeground</item>
    <!--应用的背景色，popMenu的背景默认使用该颜色-->
    <item name="android:colorBackground">@color/colorForeground</item>
    
    <!--各个控制控件的默认颜色-->
    <item name="android:colorControlNormal">@color/colorControlNormal</item>
    <!--一般控件的选种效果默认采用该颜色-->
    <item name="android:colorAccent">@color/colorAccent</item>
    <!--控件选中时的颜色，默认使用colorAccent-->
    <item name="android:colorControlActivated">@color/colorControlActivated</item>
  
    <!--控件按压时的色调-->
    <item name="android:colorControlHighlight">@color/colorControlHighlight</item>
  
    <!--Button，textView的文字颜色-->
    <item name="android:textColor">@color/text_dark</item>
    
    <!--RadioButton checkbox等控件的文字-->
    <item name="android:textColorPrimaryDisableOnly">@color/text_dark</item>
    
    <!--默认按钮的背景颜色-->
    <item name="android:colorButtonNormal">@color/text_dark</item>
    
    <!--对话框的背景是否变暗-->
    <item name="android:backgroundDimEnabled">true</item>  

    <!--Activity 的切换动画。其引用的 activityAnim 也是 style ，需要继承 parent="@android:style/Animation.Translucent"-->
    <item name="android:windowAnimationStyle">@style/activityAnim</item>

    <!--title 标题栏字体设置-->
    <item name="android:titleTextAppearance">@style/MaterialAnimations.TextAppearance.Title</item>


    <!--允许使用transitions(过渡动画)-->
    <item name="android:windowContentTransitions">true</item>
    <!--是否覆盖执行，其实可以理解成前后两个页面是同步执行还是顺序执行-->
    <item name="android:windowAllowEnterTransitionOverlap">false</item>
    <!--与上面相同。即上一个设置了退出动画，这个设置了进入动画，两者是否同时执行。-->
    <item name="android:windowAllowReturnTransitionOverlap">false</item>
</style>
```

我们可以在color.xml里自定义颜色,来达到想要的深色/浅色模式下各部分的颜色,可以在values与values-night下各写一个color.xml文件,方便我们进行区分!

注意:
* color.xml里的名称,相当于变量,是可以自己定义
* themes.xml里的名称,相当于常量,不能自己定义

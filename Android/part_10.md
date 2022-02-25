# Android使用BottomSheetDialog

***
## 第一步：新建一个“可绘资源文件”（Drawable Resource File）
* 右击drawable-New-Drawable Resource File
* 这个文件的目的：等会我们的BottomSheetDialog会使用这个自定义样式

```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <!--    背景颜色-->
    <solid android:color="@color/black" />

    <!--    矩形四个角的弧度-->
    <corners
        android:bottomLeftRadius="0dp"
        android:bottomRightRadius="0dp"
        android:topLeftRadius="30dp"
        android:topRightRadius="30dp" />
</shape>
```

## 第二步：新建一个布局文件（Layout Resource File）
* 右击layout-New-Layout Resource File
* 这个文件的目的：BottomSheetDialog能看到的实体
* 这边为了演示简单一点，主要是为了演示如何对BottomSheetDialog里的控件进行操作
* LinearLayout的样式使用的是我们上面自定义的样式

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:id="@+id/bottomSheetContainer"
    android:background="@drawable/bottomsheet_background"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="测试" />

    <Button
        android:id="@+id/BottomSheet_Button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="控件button" />
</LinearLayout>
```
## 第三步：打开values/themes.xml
* values下的themes.xml还是values-night下的都行
* 目的是添加主题样式，等会我们会在代码里调用
* 在resources下添加以下代码

```
    <style name="BottomSheetDialogTheme" parent="Theme.MaterialComponents.Light.BottomSheetDialog">
        <item name="bottomSheetStyle">@style/BottomSheetStyle</item>
        <item name="android:backgroundDimEnabled">false</item>
    </style>

    <style name="BottomSheetStyle" parent="Widget.Design.BottomSheet.Modal">
        <item name="android:background">@android:color/transparent</item>
    </style>
```

## 第四步：使用
* 调用BottomSheetDialog场景有很多，比如手指上划打开，点击按钮打开等等
* 这里简单的，就在activity_main.xml里添加一个按钮，点击打开
* mainActivity代码如下
* 注意：BottomSheetDialog里的控件要通过bottomSheetView.findViewById()获取

```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button button = findViewById(R.id.button);
        BottomSheetDialog bottomSheetDialog = new BottomSheetDialog(this, R.style.BottomSheetDialogTheme);
        View bottomSheetView = LayoutInflater.from(getApplicationContext()).inflate(R.layout.layout_bottomsheet, findViewById(R.id.bottomSheetContainer));

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                bottomSheetView.findViewById(R.id.BottomSheet_Button).setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(MainActivity.this, "测试", Toast.LENGTH_SHORT).show();
                    }
                });

                bottomSheetDialog.setContentView(bottomSheetView);
                bottomSheetDialog.show();
            }
        });

    }
}
```
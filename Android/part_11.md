# 动态添加控件
***
* 添加布局文件的方法有两种

## 第一种：就是我们常用的，通过布局文件xml文件添加

* 但这种方法有一个小问题，就是当我们想添加很多控件，控件类型一样，里面的具体属性又有些不同时，就比较麻烦
* 比如我们想添加很多的TextView，来布局一个棋盘文件
* 代码如下，要复制粘贴81个，太麻烦了

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.gridlayout.widget.GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/gridLayout"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <TextView
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:gravity="center"
        android:text="1"
        android:textSize="20dp" />

    <TextView
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:gravity="center"
        android:text="2"
        android:textSize="20dp" />

    <TextView
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:gravity="center"
        android:text="3"
        android:textSize="20dp" />

    <!-- 一直复制粘贴到81 -->

    <TextView
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:gravity="center"
        android:text="81"
        android:textSize="20dp" />

</androidx.gridlayout.widget.GridLayout>
```

## 第二种：动态添加，通过代码来添加
* 布局文件activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.gridlayout.widget.GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/gridLayout"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

* MainActivity
```
GridLayout gridLayout = findViewById(R.id.gridLayout);
```
这里的GridLayout是六大布局之一，具体根据你自己的需求来，我这里用GridLayout作为根布局来举例

```
gridLayout.setColumnCount(9);
gridLayout.setRowCount(9);
```
设置gridLayout有多少行多少列，这里是9行，9列

```
for (int i = 1; i < 82; i++) {
    GridLayout.LayoutParams layoutParams = new GridLayout.LayoutParams();
    layoutParams.setGravity(Gravity.CENTER);
    layoutParams.setMargins(5, 5, 5, 5);  


    TextView textView = new TextView(this);
    //设置textView宽度
    textView.setWidth(100);

    //设置textView高度
    textView.setHeight(100);

    //设置textView文字
    textView.setText(" " + i + " ");

    //设置textView文字大小
    textView.setTextSize(20);

    //设置textView文字颜色
    textView.setTextColor(Color.WHITE);

    //设置textView文字对齐方式
    textView.setGravity(Gravity.CENTER);

    //设置textView背景
    textView.setBackgroundColor(Color.BLACK);

    //向gridLayout添加textView，layoutParams是告知gridLayout，textView怎么放，放在哪的
    gridLayout.addView(textView, layoutParams);
}
```
这里的GridLayout.LayoutParams，虽然不是必须的，因为也可以这样写gridLayout.addView(textView)。

但是我们会发现一个问题，就是在xml文件中,TextView有一个android:layout_margin属性，但是动态添加布局的时候是没有的。

那我们想要实现android:layout_margin这个效果，就要用到LayoutParams，也就相当于xml文件里这样写，LinearLayout只是举例，实际上应该是ViewGroup的子类
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.gridlayout.widget.GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridLayout"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="12dp">

        <TextView
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:background="@color/black"/>

    </LinearLayout>
    
</androidx.gridlayout.widget.GridLayout>
```

* 完整代码
```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        GridLayout gridLayout = findViewById(R.id.gridLayout);

        gridLayout.setColumnCount(9);
        gridLayout.setRowCount(9);

        for (int i = 1; i < 82; i++) {
            GridLayout.LayoutParams layoutParams = new GridLayout.LayoutParams();
            layoutParams.setGravity(Gravity.CENTER);
            layoutParams.setMargins(5, 5, 5, 5);
            TextView textView = new TextView(this);
            textView.setWidth(100);
            textView.setHeight(100);
            textView.setText(" " + i + " ");
            textView.setTextSize(20);
            textView.setTextColor(Color.WHITE);
            textView.setGravity(Gravity.CENTER);
            textView.setBackgroundColor(Color.BLACK);
            gridLayout.addView(textView, layoutParams);
        }
    }
}
```

[![buhn1I.png](https://s4.ax1x.com/2022/02/28/buhn1I.png)](https://imgtu.com/i/buhn1I)
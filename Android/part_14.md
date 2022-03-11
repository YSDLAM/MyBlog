# ViewModel（Android Jetpack 的一部分）

***

* 我们刚开始学习的Android的时候，已经做到了视图（layout）与控制代码（activity）进行分离

* 但是我们的数据还写在activity里面，数据少的时候还好，要是多了维护起来很困难

* 所以ViewModel就是用来解决这个问题

* 当我们手机发生旋转的时候，回重新调用onCreate方法，有可能导致数据丢失

* ViewModel管理的数据没有这个问题！

先新建一个项目，为了简单演示，我们布局文件只有一个TextView和两个Button，TextView用来显示数字，第一个button将数字加一，第二个button将数字减一,效果如下：

[![bILiOf.png](https://s1.ax1x.com/2022/03/11/bILiOf.png)](https://imgtu.com/i/bILiOf)

## 布局代码
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:gravity="center"
        android:textSize="40dp"
        android:layout_height="wrap_content"
        android:text="0"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.146" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="44dp"
        android:text="+1"
        app:layout_constraintBottom_toTopOf="@+id/button2"
        app:layout_constraintEnd_toEndOf="@+id/button2"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/button2" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="100dp"
        android:text="-1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.524"
        app:layout_constraintStart_toStartOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## 新建一个类继承ViewModel
* 以后我们要将所有的界面能看到的数据都存放在继承ViewModel的类里！
```
public class MyViewModel extends ViewModel {

    private int number = 0;

    public int getNumber() {
        return number;
    }

    public void setNumber(int number) {
        this.number = number;
    }
}
```

## MainActivity代码
```
public class MainActivity extends AppCompatActivity {

    MyViewModel myViewModel;
    TextView textView;
    Button button_1, button_2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        myViewModel = new ViewModelProvider(this).get(MyViewModel.class);
        textView = findViewById(R.id.textView);
        button_1 = findViewById(R.id.button);
        button_2 = findViewById(R.id.button2);
        textView.setText(String.valueOf(myViewModel.getNumber()));

        button_1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myViewModel.setNumber(myViewModel.getNumber() + 1);
                textView.setText(String.valueOf(myViewModel.getNumber()));
            }
        });
        button_2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myViewModel.setNumber(myViewModel.getNumber() - 1);
                textView.setText(String.valueOf(myViewModel.getNumber()));
            }
        });

    }
}
```
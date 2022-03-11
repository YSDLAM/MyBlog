# Android DataBinding(数据绑定)

***
> 数据绑定与视图绑定都可以取代findViewById

> 如果你数据绑定只是为了取代findViewById，请使用 [视图绑定](/Android/part_12.md)

## 第一步：启用模块
build.gradle
```
android {
    dataBinding {
        enabled = true
    }
}
```

## 第二：新建一个Student类
* DataBinding一般与ViewModel一同使用，本文暂不使用ViewModel
* 所以先用这个Student类进行演示
```
public class Student {
    private String id;
    private String name;

    public Student(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

## 第三：布局文件
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="student"
            type="com.example.databindingdemo.Student" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:gravity="center"
                android:text="学号:"
                android:textSize="20sp" />

            <TextView
                android:id="@+id/name"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:gravity="left"
                android:text="@{student.id}"
                android:textSize="20sp"
                tools:ignore="RtlHardcoded" />
        </LinearLayout>


        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:gravity="center"
                android:text="姓名:"
                android:textSize="20sp" />

            <TextView
                android:id="@+id/age"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:gravity="left"
                android:text="@{student.name}"
                android:textSize="20sp"
                tools:ignore="RtlHardcoded" />
        </LinearLayout>
        
        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:gravity="center"
            android:text="注册"
            android:textSize="20dp" />
    </LinearLayout>
</layout>
```

## 第四：MainActivity中
```
public class MainActivity extends AppCompatActivity {
    ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
        Student student = new Student("0001", "张三");
        binding.setStudent(student);
        binding.button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "点击", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

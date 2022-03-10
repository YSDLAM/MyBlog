# Android View Binding(视图绑定)

***
> 好处:主要就是不用写findViewById，以及更加安全，还是值得一学的！

> 与使用 findViewById 相比，视图绑定具有一些很显著的优点：

* Null 安全：由于视图绑定会创建对视图的直接引用，因此不存在因视图 ID 无效而引发 Null 指针异常的风险。此外，如果视图仅出现在布局的某些配置中，则绑定类中包含其引用的字段会使用 @Nullable 标记。

* 类型安全：每个绑定类中的字段均具有与它们在 XML 文件中引用的视图相匹配的类型。这意味着不存在发生类转换异常的风险。
这些差异意味着布局和代码之间的不兼容将会导致构建在编译时（而非运行时）失败。


## 在 Activity 中使用视图绑定

### 第一：启用库
build.gradle
```
android {
    viewBinding {
        enabled = true
    }
}
```
### 第二：布局和往常一样
主要注意：如果您希望在生成绑定类时忽略某个布局文件，请将 tools:viewBindingIgnore="true" 属性添加到相应布局文件的根视图中
比如
```
<LinearLayout
    tools:viewBindingIgnore="true" >
</LinearLayout>
```

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:gravity="center"
        android:text="Hello World"
        android:textSize="40dp" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:gravity="center"
        android:text="按钮"
        android:textSize="20dp" />

</LinearLayout>
```
### 第三：MainActivity中
* 这里的ActivityMainBinding事系统自动生成绑定类，重要依据就是将 XML 文件的名称转换为驼峰式大小写，并在末尾添加“Binding”一词
* activity_main.xml绑定类就是ActivityMainBinding
```
public class MainActivity extends AppCompatActivity {

    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        View view = binding.getRoot();
        setContentView(view);

        binding.name.setText("我爱你");

        binding.button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "点击了", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```
***

## 在 Fragment 中使用视图绑定

### 第一：在activity_main.xml中
* fragment依附于activity而存在
* 注意：这里的fragment必须有一个id，否则会报错！
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <fragment
        android:id="@+id/fragment"
        android:name="com.example.viewbindingdemo.BlankFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

### 第二：BlankFragment.java
```
public class BlankFragment extends Fragment {
    private FragmentBlankBinding binding;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        binding=FragmentBlankBinding.inflate(inflater,container,false);
        View view=binding.getRoot();
        binding.text.setText("Hello World");
        return view;
    }
    
    public void onDestroyView() {
        super.onDestroyView();
        binding = null;
    }
}
```
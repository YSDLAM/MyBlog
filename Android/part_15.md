# liveData

***

* 在上一节当中我们学习了ViewModel
* 这次的LiveData配合ViewModel将会更加的方便
* 官方解释：LiveData 是一种可观察的数据存储器类，我们重点关注这个“观察”！

## 第一：布局文件

* 布局文件简单，用的约束布局，复制粘贴到activity_main.xml就能看到！

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="124dp"
        android:text="0"
        android:textSize="40dp"
        app:layout_constraintBottom_toTopOf="@+id/button"
        app:layout_constraintEnd_toEndOf="@+id/button"
        app:layout_constraintStart_toStartOf="@+id/button" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="40dp"
        android:text="+1"
        app:layout_constraintBottom_toTopOf="@+id/button2"
        app:layout_constraintEnd_toEndOf="@+id/button2"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="@+id/button2" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="264dp"
        android:text="-1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## 第二：MyViewModel类

* 这边的MyViewModel类与之前的有些区别

* 变量number的类型为MutableLiveData类型

* MutableLiveData是LiveData的一个子类

* 这边也可以这样写：private LiveData< Integer > number

* LiveData不可变 (只有number.getValue()方法) ,MutableLiveData是可变的(number.getValue()/number.setValue()都有)，所以这里用了MutableLiveData

```java
import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;

public class MyViewModel extends ViewModel {
    private MutableLiveData<Integer> number;

    public MutableLiveData<Integer> getNumber() {
        if (number == null) {
            number = new MutableLiveData<>();
            number.setValue(0);
        }
        return number;
    }

    public void setNumber(int n) {
        number.setValue(number.getValue() + n);
    }
}
```

## 第三：MainActivity

* 这里多了一个myViewModel.getNumber().observe(this, new Observer< Integer >() {}

* observe作为一个观察者的存在，onChanged()方法,当myViewModel.getNumber()返回的内容变化时就会调用里面的代码！

```java
import androidx.appcompat.app.AppCompatActivity;
import androidx.lifecycle.Observer;
import androidx.lifecycle.ViewModelProvider;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    MyViewModel myViewModel;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        TextView textView = findViewById(R.id.textView);
        myViewModel = new ViewModelProvider(this).get(MyViewModel.class);
        myViewModel.getNumber().observe(this, new Observer<Integer>() {
            @Override
            public void onChanged(Integer integer) {
                textView.setText(String.valueOf(integer));
            }
        });

        //加一
        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myViewModel.setNumber(1);
            }
        });

        //减一
        findViewById(R.id.button2).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myViewModel.setNumber(-1);
            }
        });
    }
}
```

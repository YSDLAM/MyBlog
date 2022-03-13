# Android DataBinding(数据绑定)

***

* 数据绑定与视图绑定都可以取代findViewById

* 如果你数据绑定只是为了取代findViewById，请使用 [视图绑定](/Android/part_12.md)

## 第一步：启用模块

build.gradle

```text
android {
    dataBinding {
        enabled = true
    }
}
```

## 第二：新建一个MyViewModel类继承ViewModel

* DataBinding与ViewModel和LiveData放在一起使用简直就是绝配

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

## 第三：布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="data"
            type="com.example.databindingdemo.MyViewModel" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/textView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="124dp"
            android:text="@{String.valueOf(data.number)}"
            android:textSize="40dp"
            app:layout_constraintBottom_toTopOf="@+id/button"
            app:layout_constraintEnd_toEndOf="@+id/button"
            app:layout_constraintStart_toStartOf="@+id/button" />

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="40dp"
            android:onClick="@{()->data.setNumber(1)}"
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
            android:onClick="@{()->data.setNumber(-1)}"
            android:text="-1"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.498"
            app:layout_constraintStart_toStartOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

## 第四：MainActivity中

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

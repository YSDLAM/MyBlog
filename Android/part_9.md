# Android开发使用Room 
***
* Room是Google自己的数据库框架

* 它是Jetpack框架的一部分

* Room持久性库在 SQLite 的基础上提供了一个抽象层，让用户能够在充分利用 SQLite 的强大功能的同时，获享更强健的数据库访问机制。

* 等大家SQLite数据库熟悉透彻之后，就可以尝试使用Room


## 第一步：添加依赖
（2022 年 1 月 12 日，目前最新版版本2.4.1）

* 将下面代码添加到build.gradle，dependencies里
* // optional - RxJava2 support for Room表示可选，使用Java的可以将其删除

```
    def room_version = "2.4.1"

    implementation "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"

    // optional - RxJava2 support for Room
    implementation "androidx.room:room-rxjava2:$room_version"

    // optional - RxJava3 support for Room
    implementation "androidx.room:room-rxjava3:$room_version"

    // optional - Guava support for Room, including Optional and ListenableFuture
    implementation "androidx.room:room-guava:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    // optional - Paging 3 Integration
    implementation "androidx.room:room-paging:2.4.1"
```

## 第二步：新建一个类
* 一定要将其注解为@Entity
```
    @Entity
public class Student {
    @PrimaryKey
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

## 第二步：新建一个接口类
* 一定要注解为@Dao
```
import androidx.room.Dao;
import androidx.room.Delete;
import androidx.room.Insert;
import androidx.room.Query;
import androidx.room.Update;

    @Dao
public interface StudentDao {
    @Insert
    void InsertStudent(Student... students);

    @Delete
    void DeleteStudent(Student... students);

    @Update
    void UpdateStudent(Student... students);

    @Query("delete from Student")
    void ClearStudent();

    @Query("select * from student")
    List<Student> GetAllStudent();
}
```

## 第三步：新建一个抽象类继承RoomDatabase
```
import androidx.room.Database;
import androidx.room.RoomDatabase;

    @Database(entities = {Student.class}, version = 1, exportSchema = false)
public abstract class StudentDatabase extends RoomDatabase {
    public abstract StudentDao GetStudentDao();
}
```

## MainActivity.java

```
import android.annotation.SuppressLint;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import java.util.List;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    StudentDatabase studentDatabase;
    StudentDao studentDao;
    TextView textView;
    Button button_add, button_delete, button_update, button_clear;
    EditText editText_add_id, editText_add_name, editText_delete_id, editText_update_id, editText_update_name;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        studentDatabase = Room.databaseBuilder(this, StudentDatabase.class, "Student")
                .allowMainThreadQueries()
                .build();
        studentDao = studentDatabase.GetStudentDao();


        textView = findViewById(R.id.textView);
        button_add = findViewById(R.id.button_add);
        button_clear = findViewById(R.id.button_clear);
        button_delete = findViewById(R.id.button_delete);
        button_update = findViewById(R.id.button_update);
        editText_add_id = findViewById(R.id.editText_add_id);
        editText_add_name = findViewById(R.id.editText_add_name);
        editText_delete_id = findViewById(R.id.editText_delete_id);
        editText_update_id = findViewById(R.id.editText_update_id);
        editText_update_name = findViewById(R.id.editText_update_name);


        button_update.setOnClickListener(this);
        button_delete.setOnClickListener(this);
        button_clear.setOnClickListener(this);
        button_add.setOnClickListener(this);
        RefreshData();
    }

    void RefreshData() {
        List<Student> list = studentDao.GetAllStudent();
        String values = "";
        for (int i = 0; i < list.size(); i++) {
            Student student = list.get(i);
            values += "学号:" + student.getId() + " 姓名:" + student.getName() + "\n";
        }
        textView.setText(values);
    }

    @SuppressLint("NonConstantResourceId")
    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button_add:
                String add_id = "";
                String add_name = "";
                add_id = editText_add_id.getText().toString();
                add_name = editText_add_name.getText().toString();
                if (!add_id.equals("") && !add_name.equals("")) {
                    Student student = new Student(add_id, add_name);
                    studentDao.InsertStudent(student);
                    RefreshData();
                } else {
                    Toast.makeText(this, "不可为空", Toast.LENGTH_SHORT).show();
                }
                break;
            case R.id.button_delete:
                String delete_id = "";
                delete_id = editText_delete_id.getText().toString();
                if (!delete_id.equals("")) {
                    Student student = new Student(delete_id,"");
                    student.setId(delete_id);
                    studentDao.DeleteStudent(student);
                    RefreshData();
                } else {
                    Toast.makeText(this, "不可为空", Toast.LENGTH_SHORT).show();
                }
                break;
            case R.id.button_update:
                String update_id = "";
                String update_name = "";
                update_id = editText_update_id.getText().toString();
                update_name = editText_update_name.getText().toString();
                if (!update_id.equals("") && !update_name.equals("")) {
                    Student student = new Student(update_id, update_name);
                    student.setId(update_id);
                    studentDao.UpdateStudent(student);
                    RefreshData();
                } else {
                    Toast.makeText(this, "不可为空", Toast.LENGTH_SHORT).show();
                }
                break;
            case R.id.button_clear:
                studentDao.ClearStudent();
                RefreshData();
                break;
        }
    }
}
```

## activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/editText_delete_id"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:ems="10"
        android:gravity="center"
        android:hint="删除人学号"
        android:inputType="textPersonName"
        android:textSize="16dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="@+id/guideline6"
        app:layout_constraintTop_toTopOf="@+id/guideline4" />

    <EditText
        android:id="@+id/editText_add_name"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:ems="10"
        android:gravity="center"
        android:hint="添加姓名"
        android:inputType="textPersonName"
        android:textSize="16dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline3"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="@+id/guideline9"
        app:layout_constraintTop_toTopOf="@+id/guideline2" />

    <EditText
        android:id="@+id/editText_update_name"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:ems="10"
        android:gravity="center"
        android:hint="修改后姓名"
        android:inputType="textPersonName"
        android:textSize="16dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="@+id/guideline9"
        app:layout_constraintTop_toTopOf="@+id/guideline3" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.55" />

    <Button
        android:id="@+id/button_add"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="增加"
        app:layout_constraintBottom_toTopOf="@+id/guideline3"
        app:layout_constraintEnd_toStartOf="@+id/guideline6"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline2" />

    <Button
        android:id="@+id/button_update"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="修改"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toStartOf="@+id/guideline6"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline3" />

    <Button
        android:id="@+id/button_delete"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="删除"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toStartOf="@+id/guideline6"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline4" />

    <ScrollView
        android:id="@+id/scrollView2"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <TextView
            android:id="@+id/textView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:text="TextView" />
    </ScrollView>

    <Button
        android:id="@+id/button_clear"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="清空"
        app:layout_constraintBottom_toTopOf="@+id/guideline7"
        app:layout_constraintEnd_toStartOf="@+id/guideline6"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.65" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.75" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.85" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_begin="125dp" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.95" />

    <EditText
        android:id="@+id/editText_add_id"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:ems="10"
        android:gravity="center"
        android:hint="添加学号"
        android:inputType="textPersonName"
        android:textSize="16dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline3"
        app:layout_constraintEnd_toStartOf="@+id/guideline9"
        app:layout_constraintStart_toStartOf="@+id/guideline6"
        app:layout_constraintTop_toTopOf="@+id/guideline2" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline9"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_begin="273dp" />

    <EditText
        android:id="@+id/editText_update_id"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:ems="10"
        android:gravity="center"
        android:hint="修改人学号"
        android:inputType="textPersonName"
        android:textSize="16dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toStartOf="@+id/guideline9"
        app:layout_constraintStart_toStartOf="@+id/guideline6"
        app:layout_constraintTop_toTopOf="@+id/guideline3" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

[![b9HbkV.png](https://s4.ax1x.com/2022/02/23/b9HbkV.png)](https://imgtu.com/i/b9HbkV)
# Android开发使用SQLite数据库

***

这里举一个简单的例子

## 第一步：新建一个类继承SQLiteOpenHelper
```
public class MyDatabaseHelper extends SQLiteOpenHelper {

    public MyDatabaseHelper(@Nullable Context context, @Nullable String name, @Nullable SQLiteDatabase.CursorFactory factory, int version) {
        super(context, "School.db", factory, version);
    }

    private String createTable = "create table student(id integer primary key,name text)";

    @Override
    public void onCreate(SQLiteDatabase sqLiteDatabase) {
        sqLiteDatabase.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {

    }
}
```

## 第二步：使用数据库
数据库的使用无非就是增删改查，在你需要使用数据库的页面中先写入一下代码
```
    MyDatabaseHelper dbHelp = new MyDatabaseHelper(this, "School.db", null, 1);
    SQLiteDatabase db = dbHelp.getWritableDatabase();
```
我们接下来对数据库的增删改查都是通过这个SQLiteDatabase对象db来操作

### 增
```
    ContentValues values = new ContentValues();
    values.put("学号一", "张三");
    db.insert("student", null, values);
```
也可以
```
    String sql = "insert into student ('id','name') values (?,?)";
    db.execSQL(sql, new Object[]{"学号一","张三"});
```

### 删
```
    db.delete("student", "id=?", new String[]{"学号一"});
```
也可以
```
    String sql = "delete from student where 'id'=?)";
    db.execSQL(sql, new Object[]{"学号一"});
```
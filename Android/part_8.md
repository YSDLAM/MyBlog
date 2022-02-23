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
    SQLiteDatabase db = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        db = dbHelp.getWritableDatabase();
    }
```
我们接下来对数据库的增删改查都是通过这个SQLiteDatabase对象db来操作

### 增
```
    String sql_z = "insert into student values(?,?)";
    db.execSQL(sql_z, new Object[]{"0001", "张三"});
```

### 删
```
    String sql_s = "delete from student where id=?";
    db.execSQL(sql_s, new Object[]{"0001"});
```

### 改
```
    String sql_g = "update student set name=? where id=?";
    db.execSQL(sql_g, new Object[]{"李四", "0001"});
```

### 查
```
    String id="";
    String name="";
    Cursor cursor = db.query("student", null, null, null, null, null, null);
    if (cursor.moveToFirst()) {
         do {
            id = cursor.getString(cursor.getColumnIndex("id"));
            name = cursor.getString(cursor.getColumnIndex("name"));
        } while (cursor.moveToNext());
    }
```
* 增删改查其实都有对应的方法
* 增-db.db.insert()
* 删-db.delete()
* 改-db.update()
* 查-db.rawQuery()
* 但是由于本人喜欢SQL命令，所以我个人喜欢上面那种方法，也比较直观！
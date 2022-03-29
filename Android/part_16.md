# Android数据持久化存储技术

***

## SharedPreferences

* 写入
  
```java
    SharedPreferences mysharedpreferences = getSharedPreferences("user", 0);
    SharedPreferences.Editor editor = mysharedpreferences.edit();
    editor.putString("name", "张三");
    editor.putString("pass", "123");
    editor.apply();
```

* 读取

```java
    SharedPreferences mysharedpreferences = getSharedPreferences("user", 0);
    String name = mysharedpreferences.getString("name", null);
    String pass = mysharedpreferences.getString("pass", null);
```

* 清空

```java
    SharedPreferences mysharedpreferences = getSharedPreferences("user", 0);
    SharedPreferences.Editor editor = mysharedpreferences.edit();
    editor.clear();
    editor.apply();
```

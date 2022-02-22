# 使用RecyclerView功能
* 建议用RecyclerView而不是ListView
* RecyclerView能实现Listview的所有功能
* RecyclerView性能更优

同样按照以往的格式，告诉你怎么用分为几步,实用较多，理论较少

这里比如我想做一个 ->新闻<- 的列表功能

## 第一步：新建一个类作为实体类

```
public class News {
    //新闻的标题
    private String news_title;
    //新闻的内容
    private String news_content;
    //新闻的时间
    private String news_time;

    public News(String news_title, String news_content, String news_time) {
        this.news_title = news_title;
        this.news_content = news_content;
        this.news_time = news_time;
    }

    public String getNews_title() {
        return news_title;
    }

    public void setNews_title(String news_title) {
        this.news_title = news_title;
    }

    public String getNews_content() {
        return news_content;
    }

    public void setNews_content(String news_content) {
        this.news_content = news_content;
    }

    public String getNews_time() {
        return news_time;
    }

    public void setNews_time(String news_time) {
        this.news_time = news_time;
    }
}
```

## 第二步：新建一个layout，作为列表对外展示的样子

* 这里有一个注意点 android:layout_height要写成"wrap_content"或者是比如20dp
* 千万不能写成match_parent

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <TextView
        android:id="@+id/TextView_Title"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="标题" />

    <TextView
        android:id="@+id/TextView_Content"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="新闻内容" />

    <TextView
        android:id="@+id/TextView_Time"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="right"
        android:text="时间" />

</LinearLayout>
```
## 第三步：重写适配器
```
public class NewsAdapter extends RecyclerView.Adapter<NewsAdapter.ViewHolder> {

    private final List<News> NewsItemList;
    Context context;

    public NewsAdapter(List<News> newsItemList, Context context) {
        NewsItemList = newsItemList;
        this.context = context;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.news_item, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        News NewsItem = NewsItemList.get(position);
        holder.news_title.setText(NewsItem.getNews_title());
        holder.news_content.setText(NewsItem.getNews_content());
        holder.news_time.setText(NewsItem.getNews_time());
    }

    @Override
    public int getItemCount() {
        return NewsItemList.size();
    }

    /*
     * 绑定数据
     * 将数据与布局文件里的位置进行绑定
     */
    class ViewHolder extends RecyclerView.ViewHolder {

        TextView news_title, news_content, news_time;

        public ViewHolder(@NonNull View itemView) {
            super(itemView);
            news_title = itemView.findViewById(R.id.TextView_Title);
            news_content = itemView.findViewById(R.id.TextView_Content);
            news_time = itemView.findViewById(R.id.TextView_Time);
        }
    }
}
```
## 第四步：使用
activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

MainActivity.java
```
public class MainActivity extends AppCompatActivity {
    private final List<News> newsList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //填充数据
        for (int i = 0; i < 9; i++) {
            News news = new News("标题" + i, "内容" + i, "时间" + i);
            newsList.add(news);
        }

        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(layoutManager);
        NewsAdapter adapter = new NewsAdapter(newsList, this);
        recyclerView.setAdapter(adapter);
    }
}
```

[![HvSxHI.png](https://s4.ax1x.com/2022/02/21/HvSxHI.png)](https://imgtu.com/i/HvSxHI)
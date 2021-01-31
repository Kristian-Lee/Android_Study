## **TextView**

### 跑马灯效果

```xml-dtd
android:ellipsize="marquee"	
android:singleLine="true"	//设置单行，超出范围就使用...
android:focusable="true"	//设置为可获得焦点。textview默认是显示文本无焦点的
android:marqueeRepeatLimit="marquee_forever"	//设置跑马时间无限长，-1同理
android:clickable="true"		//设置可以被点击
```

但是运行后不会自己跑，需要点击让其获得焦点。或者可以直接在activity中使其获取焦点

```java
private TextView tv;
tv = findViewById(R.id.tv);
tv.setSelected(true);
```



### 删除线效果

```
private TextView tv;
tv = findViewById(R.id.tv);
tv.getPaint().setFlags(Paint.STRIKE_THRU_TEXT_FLAG);	//设置中划线
tv.getPaint().setAntiAlias(true);	//去除锯齿
```

ps：`Paint`是Android的画笔类，可以绘制图形或文本，对其颜色、字体大小、粗细、风格、边框等进行处理。



### 下划线效果

```java
private TextView tv;
tv = findViewById(R.id.tv);
tv.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);	//下划线
```

也可以用html实现（略骚

```java
private TextView tv;
tv = findViewById(R.id.tv);
tv.setText(Html.fromHtml("<u>Hello World!</u>"));
```





## Button

### shape标签（用来配置圆角、阴影等）

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    
    <!-- 填充的颜色 -->
    <solid android:color="#f2eeee" />
    <!-- 设置按钮的四个角为弧形 -->
    <!-- android:radius 弧形的半径 -->
    <corners android:radius="2dp" />

    <!-- 描边 -->
    <stroke
        android:width="1dp"
        android:color="#df1f1f" />

</shape>
```

在对应组件的`background`属性通过`@drawable`引用该文件即可（文件在drawable文件夹中）。



### selector标签 （用来配置按压效果）

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- 按压时显示效果 -->
    <item android:state_pressed="true">
        <shape>
            <solid android:color="@color/color_btn_unclick" />
            <size android:width="62dp" android:height="26dp" />
            <corners android:radius="8dp" />
        </shape>
    </item>

    <!-- 没有按压时显示效果 -->
    <item android:state_pressed="false">
        <shape>
            <solid android:color="@color/color_btn_click" />
            <size android:width="62dp" android:height="26dp" />
            <corners android:radius="8dp" />
        </shape>
    </item>
    
    <!-- 默认不填就是除以上情况的其他所有情况的显示效果。注意必须写在最后！ -->
    <item>
        <shape>
        </shape>
    </item>

</selector>
```

item的状态有：`pressed`按压、`focused`获得焦点、`selected`选中、`checkable`可用、`checked`选中、`enable`可用、`window_focused`获得窗口的焦点。

同样在对应组件的`background`属性通过`@drawable`引用该文件即可（文件在drawable文件夹中）。





## EditText

`inputType`属性可以实现对输入的控制。如text、number、textPassword、numberPassword等。

添加文本变化监听器，当输入框发生变化时执行对应操作。

```java
private EditText et;
et = findViewById(R.id.et);
et.addTextChangedListener(new TextWatcher() {
    @Override
    /**  变化前执行 */
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {}
    @Override
    /**  变化时执行 */
    public void onTextChanged(CharSequence s, int start, int before, int count) {}
    @Override
    /**  变化后执行 */
    public void afterTextChanged(Editable s) {}
});
```





## RadioButton

可以通过设置属性`button`的值为`@null`，自定义单选框样式。

设置监听事件

```java
private RadioGroup rg;
rg = findViewById(R.id.rg);
rg.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup group, int checkedId) {
        /** checked为选中的按钮id */
        RadioButton rb = group.findViewById(checkedId);
        /** 消息提示，Toast.LENGTH_SHORT为时长，1s */
        Toast.makeText(MainActivity.this, rb.getText(), Toast.LENGTH_SHORT).show();
    }
});
```





## CheckBox

自定义选择框

> 通过设置属性`button`的值为`@drawable/xxx`，`xxx`为`drawable`文件夹下的布局配置文件，包含`selector`标签，指定选中和未选中的样式。

设置监听事件

```java
private CheckBox cb;
cb = findViewById(R.id.cb);
cb.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        Toast.makeText(MainActivity.this, isChecked?"已选中":"取消选中",Toast.LENGTH_SHORT).show();
    }
});
```





## ImageView

属性`src`用以引用本地图片，如`src=@drawable/xxx`。

属性`scaleType`设置显示效果，`fixXY`拉伸铺满，`fitCenter`最大显示（图片的宽或高与组件贴合），`centerCrop`裁剪铺满（保持原宽高比）。

也可以通过第三方库引用网络图片，如[Glide](https://github.com/bumptech/glide)，注意app需获取网络权限。





## ListView列表布局

需要4个文件：`ListViewActivity`listview的活动、`MyListAdapter`listview的自定义适配器，用来绑定布局和数据、`activity_list_view.xml`listview的布局文件，`layout_list_item.xml`listview中的每个项目的布局。

**activity_list_view.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <ListView
        android:id="@+id/lv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</LinearLayout>
```

**ListViewActivity**

```java
public class ListViewActivity extends AppCompatActivity {
    private ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_view);
        listView = findViewById(R.id.lv);
        /** 为listview绑定适配器 */
        listView.setAdapter(new MyListAdapter(ListViewActivity.this));
        /** 点击事件 */
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent,
                                    View view, int position, long id) {
                Toast.makeText(ListViewActivity.this,
                        "第"+ position + "个item被点击", Toast.LENGTH_SHORT).show();
            }
        });
        /** 长按事件 */
        listView.setOnItemLongClickListener(new AdapterView.OnItemLongClickListener() {
            @Override
            public boolean onItemLongClick(AdapterView<?> parent,
                                           View view, int position, long id) {
                Toast.makeText(ListViewActivity.this,
                        "第"+ position + "个item被长按", Toast.LENGTH_SHORT).show();
                /** 设置为true，即长按完不会触发其他事件，但是不能一直长按了 */
                /** 设置为false，可以一直长按，但长按完后会继续触发其他事件，例如点击事件 */
                return true;
            }
        });
    }
}
```

**MyListAdapter**

```java
public class MyListAdapter extends BaseAdapter {
	/** 需要重写下面四个方法 */
    private Context context;
    private LayoutInflater layoutInflater;
    public MyListAdapter(Context context) {
        this.context = context;
        layoutInflater = LayoutInflater.from(context);
    }

    /** 获取item总数，返回0即是空白 */
    @Override
    public int getCount() {
        return 10;
    }

    /** 获取第几个item的数据 */
    @Override
    public Object getItem(int position) {
        return null;
    }

    /** 获取item的id */
    @Override
    public long getItemId(int position) {
        return 0;
    }

    /** 自定义类，减少清系统负担，避免为每个的item的组件都实例化 */
    static class ViewHolder {
        public ImageView imageView;
        public TextView tvContent, tvName;
    }
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder = null;
        if (convertView == null) {
            /** 将item的布局绑定到listview中 */
            /** 在这里convertView就是将item的xml转化来的view对象 */
            convertView = layoutInflater.inflate(R.layout.layout_list_item, null);
            /** 只实例化一次 */
            holder = new ViewHolder();
            /** 通过holder改变组件的属性，避免实例化 */
            holder.imageView = convertView.findViewById(R.id.iv);
            holder.tvName = convertView.findViewById(R.id.tv_name);
            holder.tvContent = convertView.findViewById(R.id.tv_content);
            /** 保存holder，下次复用 */
            convertView.setTag(holder);
        }
        else {
            holder = (ViewHolder) convertView.getTag();
        }
        holder.tvName.setText("犬来八荒");
        holder.tvContent.setText("不要怕，路是越走越坚定的！");
        holder.imageView.setImageResource(R.drawable.cg);
        return convertView;
    }
}
```

> `LayoutInflater`是用来加载布局的类，`setContentView()`内部也是用他来实现的。而`inflate`就是把xml布局文件变成view的函数，





## GridView网格布局

用法和`ListView`一致。部分常用属性如下：

`numColumns`列数

`horizontalSpacing`网格的横间距

`verticalSpacing`网格的纵间距





## ScrollView滚动布局

用法很简单，根标签为ScrollView，直接子标签只能有一个。

横向滚动布局的标签是HorizontalScrollView，同样，直接子标签只能有一个。

两者可以混合使用

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>
        <HorizontalScrollView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content">
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content">
                <ImageView
                    android:layout_width="100dp"
                    android:layout_height="100dp"/>
                <ImageView
                    android:layout_width="100dp"
                    android:layout_height="100dp"/>
            </LinearLayout>
        </HorizontalScrollView>
    </LinearLayout>
</ScrollView>
```


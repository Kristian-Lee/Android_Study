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


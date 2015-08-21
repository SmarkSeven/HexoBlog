title: ToolBar详解
date: 2015-08-11 14:12:14
comments: true
tags: [Android,教程]
categories: [教程,Android]

---

# 概述 #
虽然Google在Android 3.0推了ActionBar这个控件，视图改善Android中纷乱的界面设计。然而它的体验并不优秀。在最新的V7包Google推出了ToolBar用于取代不够优秀的ActionBar。
# 使用步骤 #
## 1.style ##

我们先在 res/values/styles.xml 里增加一个名为 AppTheme.Base 的style

    <style name="AppTheme.Base" parent="Theme.AppCompat">
      <item name="windowActionBar">false</item>
      <item name="android:windowNoTitle">true</item>
    </style>
然后将原本 AppTheme 的 parent 属性 改为上面的AppTheme.Base，代码如下：
   
	 <resources>
      <!-- Base application theme. -->
      <style name="AppTheme" parent="AppTheme.Base.NoActionBar">
      </style>
      <style name="AppTheme.Base" parent="Theme.AppCompat">
      </style>
    </resources>
再来调整Android 5.0的style：  /res/values-v21/styles.xml，也将其 parent 属性改为  AppTheme.Base：

    <?xml version="1.0" encoding="utf-8"?>
    <resources>
    <style name="AppTheme" parent="AppTheme.Base">
    </style>
    </resources>
## 2.Layout ##
在 activity_main.xml 里面添加 Toolbar 控件：

    <android.support.v7.widget.Toolbar
      android:id="@+id/toolbar"
      android:layout_height="wrap_content"
      android:layout_width="match_parent"
      android:minHeight="?attr/actionBarSize">
    </android.support.v7.widget.Toolbar>
请记得用 support v7 里的 toolbar，不然然只有 API Level 21 也就是 Android 5.0 以上的版本才能使用。
## 3.程序 ##
在 MainActivity.java 中加入 Toolbar 的声明：

    Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
    setSupportActionBar(toolbar);
  
## 自定义颜色(Customization color) ##
1. colorPrimaryDark（状态栏底色）：在风格 (styles) 或是主题 (themes) 里进行设定。
2. colorPrimary（Appbar底色）：在风格 (styles) 或是主题 (themes) 里进行设定
3. navigationBarColor（导航栏底色）：仅能在 API v21 也就是 Android 5 以后的版本中使用， 因此要将之设定在 res/values-v21/styles.xml 里面
4. windowBackground（主视窗底色）：则直接在风格 (styles) 或是主(themes) 里进行设定

(res/values/styles.xml)

    <style name="AppTheme.Base" parent="Theme.AppCompat。NoActionBar">
      <item name="windowActionBar">false</item>
      <item name="android:windowNoTitle">true</item>
      <!-- Actionbar color -->
      <item name="colorPrimary">@color/accent_material_dark</item>
      <!--Status bar color-->
      <item name="colorPrimaryDark">@color/accent_material_light</item>
      <!--Window color-->
      <item name="android:windowBackground">@color/dim_foreground_material_dark</item>
    </style>

v21 的style中 (res/values-v21/styles.xml)

    <style name="AppTheme" parent="AppTheme.Base.NoActionBar">
      <!--Navigation bar color-->
      <item name="android:navigationBarColor">@color/accent_material_light</item>
    </style>

Toolbar 的 background 进行设定

    <android.support.v7.widget.Toolbar
      android:id="@+id/toolbar"
      android:layout_height="wrap_content"
      android:layout_width="match_parent"
      android:background="?attr/colorPrimary"
      android:minHeight="?attr/actionBarSize">
    </android.support.v7.widget.Toolbar>

## 5.控件 (component) ##
预设常用的几个元素

setNavigationIcon，即设定 up button 的图标

setLogo，APP 的图标

setTitle，主标题

setSubtitle，副标题

setOnmenuItemClickListener,设定菜单各按鈕的动作

在 MainActivity.java 中的代码：

    Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
    // App Logo
    toolbar.setLogo(R.drawable.ic_launcher);
    // Title
    toolbar.setTitle("My Title");
    // Sub Title
    toolbar.setSubtitle("Sub title");
    setSupportActionBar(toolbar);
    // Navigation Icon 要設定在 setSupoortActionBar 才有作用
    // 否則會出現 back button
    toolbar.setNavigationIcon(R.drawable.ab_android);

设置监听器OnMenuItemClickListener：

    private Toolbar.OnMenuItemClickListener onMenuItemClick = new Toolbar.OnMenuItemClickListener() {
      @Override
      public boolean onMenuItemClick(MenuItem menuItem) {
    String msg = "";
    switch (menuItem.getItemId()) {
      case R.id.action_edit:
    msg += "Click edit";
    break;
      case R.id.action_share:
    msg += "Click share";
    break;
      case R.id.action_settings:
    msg += "Click setting";
    break;
    }
     
    if(!msg.equals("")) {
      Toast.makeText(MainActivity.this, msg, Toast.LENGTH_SHORT).show();
    }
    return true;
      }
    };

需要將之设定在 setSupportActionBar 之后才有作用。


最终效果图如下：

![](http://i.imgur.com/G2WXSny.png)

点击时有系统自带的水波纹效果

![](http://i.imgur.com/wvNJcDN.png)




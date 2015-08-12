title: Android Support Design
date: 2015-08-12 10:12:14
comments: true
tags: [Android,MaterialDesign]
categories: [Android,MaterialDesign]

---
**Androud Support Design库使我们可以在低版本Android上实现MaterialDesign。**
# 添加依赖 #
    compile ‘com.android.support:design:22.2.0’
    compile ‘com.android.support:appcompat-v7:22.2.0’

# Snackbar #
**Snackbar同时具有Dialog和Toast的特征，其实用方式与Toast类似。**
  
    public void click(View view) {
    Snackbar.make(view, "测试提示?", Snackbar.LENGTH_LONG)
    .setAction("取消", new View.OnClickListener() {
    @Override
    public void onClick(View v) {
    Toast.makeText(MainActivity.this, "你真的取消了！",
    Toast.LENGTH_SHORT).show();
    }
    }).show();

# FloatingActionButton #
**顾名思义：这是一个浮动按钮。在外观上其实就是一个悬浮按钮，更常见的是漂浮在界面上的圆形按钮，简称FAB。FloatingActionButton的使用也很简单，它直接继承ImageView，所以ImageView的所有属性都可以用在FloatingActionButton上。**
  
    <android.support.design.widget.FloatingActionButton
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:onClick="click"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:layout_margin="0dp"
        android:src="@drawable/add"
        app:backgroundTint="#FF00FF00"
        app:rippleColor="#FF0000FF"
        app:borderWidth="0dp"
        app:fabSize="normal"
        app:elevation="10dp"
        app:pressedTranslationZ="20dp"/>

1. **app:backgroundTint是指定默认的背景颜色**
2. **app:rippleColor是指定点击时的背景颜色**
3. **app:borderWidth border的宽度**
4. **app:fabSize是指FloatingActionButton的大小，可选normal|mini**
5. **app:elevation 可以看出该空间有一个海拔的高度**
6. **app:pressedTranslationZ 哈，按下去时的z轴的便宜**

# CoordinatorLayout #
**CoordinatorLayout这个控件的作用是让它的子view更好的去协作，这里我们只是简单的用CoordinatorLayout来包裹一下FloatingActionButton来达到和Snackbar更好协作的效果。修改我们的布局:**

      <android.support.design.widget.CoordinatorLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_alignParentBottom="true"
    android:layout_alignParentRight="true"&gt;
    <android.support.design.widget.FloatingActionButton
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_margin="0dp"
    android:onClick="click"
    android:layout_gravity="right"
    android:src="@drawable/add"
    app:backgroundTint="#FF00FF00"
    app:borderWidth="0dp"
    app:elevation="20dp"
    app:fabSize="normal"
    app:pressedTranslationZ="10dp"
    app:rippleColor="#FF0000FF" />
      </android.support.design.widget.CoordinatorLayout>




---
**------**
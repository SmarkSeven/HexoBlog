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
顾名思义：这是一个浮动按钮。
  

---
**------**
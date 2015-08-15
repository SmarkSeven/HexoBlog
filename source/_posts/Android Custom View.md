title: Android 弧形进度条
date: 2015-08-14 16:12:14
comments: true
tags: [Android,View]
categories: [Android,View]

---
此处省略开场白十万字。

## 1.在Value文件夹下新建attrs.xml ##

在attrs.xml里定义我们所需的属性，然后就可以像Android自带的控件的属性一样在布局中引用。

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
 	   <declare-styleable name="CircleProgressBar">  
  	      <attr name="circleColor" format="color"/>
  	      <attr name="circleProgressColor" format="color"/>
  	      <attr name="circleWidth" format="dimension"></attr>
    	    <attr name="textColor" format="color" />  
    	    <attr name="textSize" format="dimension" /> 
    	    <attr name="max" format="integer"></attr> 
    	    <attr name="textIsDisplayable" format="boolean"></attr>
    	    <attr name="style">
    	        <enum name="STROKE" value="0"></enum>
    	        <enum name="FILL" value="1"></enum>
    	    </attr>
    	</declare-styleable> 
	</resources>

## 2.接着新建一个类CircleProgressBar extends View ##

我们需要在构造方法中获取自定义的属性。
TypedArray mTypedArray = context.obtainAttributes(attrs,R.styleable.CircleProgressBar);





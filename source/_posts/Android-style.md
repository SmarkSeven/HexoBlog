title: Android Style
date: 2015-08-17 15:44:35
comments: true
tags: [Android,Style]
categories: [Android,Style]

---
# 一、Styles #
  **在Android中，style被用来指定窗体或视图的样式，比如视图的宽高、补白(padding)、背景，字体颜色等。style不需我们在代码中进行设置， 可以在xml文件中按照 DTD格式 进行配置。 Android中的style其实跟css的思想一样，允许我们把功能实现和外观设计分离开，View配置也提供了html中如id、name属性一样的功能标签style，让我们有能力把所有的样式集中在一个地方.**

举个栗子(main.xml)：

    <? xml version= "1.0" encoding = "utf-8"?>
	 <LinearLayout xmlns:android ="http://schemas.android.com/apk/res/android"
     android:layout_width= "match_parent"
 	 android:layout_height= "match_parent"
 	 android:orientation= "vertical">
   
 	 <TextView
 	   android:id ="@+id/tv1"
 	   android:layout_width ="wrap_content"
 	   android:layout_height ="wrap_content"
 	   android:textColor ="#FFFFFF" />

 	 <TextView
 	   android:id ="@+id/tv2"
  	  android:layout_width ="wrap_content"
  	  android:layout_height ="wrap_content"
  	  android:textColor ="#FFFFFF" />

  	<TextView
 	   android:id ="@+id/tv3"
 	   android:layout_width ="wrap_content"
 	   android:layout_height ="wrap_content"
 	   android:textColor ="#FFFFFF" />
   
	</LinearLayout>

　在上面的布局中，三个TextView的layout_width、layout_height、textColor样式属性都一样，这种情况下可以单独定义一个style,以一种更好的方式来代替上面的写法，此处可以把该style理解为"模版"。
　
　example_style.xml(注：style文件必须在values目录中)
      
	<resources xmlns:android ="http://schemas.android.com/	apk/res/android">
    	<style name= "example">
    	    <item name= "android:layout_width">wrap_content </item>
    	    <item name= "android:layout_height">wrap_content </item>
    	    <item name= "android:textColor">#00FF00 </item >
	    </style>
	</resources>

接下来我我们对main.xml进行改造:

(注： 使用时只需使用style属性引用我们定义好的style即可，这种方式可以有效的避免做重复性的工作，简化我们的工作)

    <? xml version= "1.0" encoding = "utf-8"?>
	< LinearLayout xmlns:android ="http://schemas.android.com/apk/res/android"
 	 android:layout_width= "match_parent"
 	 android:layout_height= "match_parent"
 	 android:orientation= "vertical" >

  	<TextView
  	  android:id ="@+id/tv1"
  	  style= "@style/example"
  	  android:text ="@string/sample1_tv1" />

 	 <TextView
 	   android:id ="@+id/tv2"
 	   style= "@style/example"
 	   android:text ="@string/sample1_tv2" />

 	 <TextView
 	   android:id ="@+id/tv3"
 	   style= "@style/example"
  	  android:text ="@string/sample1_tv3" />
	</ LinearLayout>

# 二、Style的继承 #
**Android系统为我们提供了一整套的style，比如说显示字符串最基本的style是TextAppearance，它定义了一些最基本的属性**

## 继承的方式有两种： ##
(1)parent="@父Style名字"  或  parent = "@style/父Style名字"

(2)继承android系统自带的style样式: parent = "@android:系统自带style的名字" 或  parent = "@android:style/系统自带style的名字" 	

	<style name="TitleStyle.MyStyle"parent="@android:Theme.Light"> 
	<style name="TitleStyle.MyStyle" parent="@android:style/Theme.Light">

注：在引用其他样式(style)时，style/可以省略。

	<style name="TextAppearance">
		<item name="android:textColor">#336699</item>
		<item name="android:textSize">16sp</item>
		<item name="android:textStyle">normal</item>
	</style>

如果我们只想改变style中的某一项属性时，这时可以使用继承来实现

    <style name= "Sample1" parent= "@TextAppearance" >
   		 < item name= "android:textColor" >#00FF00 </item >
	</style >



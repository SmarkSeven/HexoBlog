title: Android Attrs
date: 2015-08-17 17:20:35
comments: true
tags: [Android,Attrs]
categories: [Android,Attrs]

---
**attrs看字面意思就是一组属性的集合，那attrs有什么用呢，在自定义View的时候，一般会自定义一些属性，
通过构造方法中AttributeSet参数的封装，取到为View配置的属性。**
# 定义attrs文件及在其他文件中引用这些属性： #
## 1 定义单个属性: ##
    <attr name="attrsName">values</attr>
  eg:  

	<attr name="myattrs">300dp</attr>
# 在其他资源文件中引用属性设置:?attr/属性名称  #
(注：?:表示引用属性，用这个标记，你所提供的资源名必须能够在主题属性中找到)
## a 引用系统自带的attrs##
	eg: ?android:attrs/系统资源ID

## b 引用自定义的属性attrs##
 
	eg: ?attrs/自定属性ID

## 使用同资源文件 ##
a、在sytles.xml中使用这睦属性：

       <style name="MyStyles">
          <item name="android:width">?attr/myattrs</item>
       </style>

b、在<TextView>标签中使用设置的属性：

      <TextView android:width="?attr/myattrs" />

#2.为自定义控件设置自定义属性declare-styleable #

## 1）在attrs.xml定义自定义属性 ##

format="color | dimension | integer ........"，每一种format属性，在自定义View中都有对应的get方式来获取。
format="color" ------> getColor();
format="dimension" -------> getDimension();
format="integer" --------> getInteger();
format="reference" --------> getResourceId();  //引用类型
format="boolean" ---------> getBoolean();
format="float" ---------> getFloat();
format="fraction" -------> getFraction(); //百分数
此处还有两种类型，放在后面讲枚举和标志位
	
	<?xml version="1.0" encoding="utf-8"?>
	<resources>
	    <declare-styleable name="CircleProgressBar">
		//自定属性，此处name值一般设置为自定义控件类名字相同
	        <attr name="circleColor" format="color"/>
	        <attr name="circleProgressColor" format="color"/>
	        <attr name="circleWidth" format="dimension"/>
	        <attr name="textColor" format="color"/>
	        <attr name="textSize" format="dimension"/>
	        <attr name="roundWidth" format="dimension"/>
	        <attr name="max"   format="integer"/>
	        <attr name="progress"   format="integer"/>
	        <attr name="textIsDisplayable" format="boolean"/>
	        <attr name="style">
	        	<enum name="STROKE" value="0"/>
	            <enum name="FILL" value="1"/>
	        </attr>
	    </declare-styleable>
	</resources>


## 2）自定义View控件CircleProgressBar ##
**通过TypedArray自取自定义的declare-styleable属性组**
 
	// 获取自定义属性和默认值
    TypedArray mTypedArray = context.obtainStyledAttributes(attrs, R.styleable.CircleProgressBar);

    circleColor = mTypedArray.getColor(R.styleable.CircleProgressBar_circleColor,0xff47d8af);

    circleProgressColor = mTypedArray.getColor(R.styleable.CircleProgressBar_circleProgressColor, Color.WHITE);

    textColor = mTypedArray.getColor(R.styleable.CircleProgressBar_textColor, Color.WHITE);

    textSize = mTypedArray.getDimension(R.styleable.CircleProgressBar_textSize, 40);

    roundWidth = mTypedArray.getDimension(R.styleable.CircleProgressBar_circleWidth, 3);

    max = mTypedArray.getInteger(R.styleable.CircleProgressBar_max, 100);

    progress = mTypedArray.getInteger(R.styleable.CircleProgressBar_progress, 100);

    textIsDisplayable = mTypedArray.getBoolean(R.styleable.CircleProgressBar_textIsDisplayable, true);

    style = mTypedArray.getInt(R.styleable.CircleProgressBar_style, 0);
    mTypedArray.recycle();

## 3）在XML布局文件中，使用该CircleProgressBar自定义组件及自定义属性 ##

	<com.yun.sevenone.customview.CircleProgressBar
        android:id="@+id/cp"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_centerInParent="true"
        android_custom:circleColor="#47d8af"
        android_custom:circleProgressColor="@android:color/black"
        android_custom:circleWidth="3dp"
        android_custom:progress="0"
        android_custom:textColor="@android:color/black"
        android_custom:textIsDisplayable="true"
        android_custom:textSize="20sp" />

# 4）attrs中自定义枚举类型的使用 #
**如在定义LinearyLayout中，设置横向排列或是纵向排列时，用到了枚举**
	
	（android:orientation="horizontal"）

##（1）在<declare-styleable>中定义 ##
基本格式：

	<declare-styleable name="名字">
	    <attr name="枚举名称">
	        <enum name="枚举健名" value="枚举值名" />
	        <enum name="枚举健名 " value="枚举值名" />
	    </attr>
	</declare-styleable>

实例：
	
	<attr name="style">
        <enum name="STROKE" value="0"/>
        <enum name="FILL" value="1"/>
    </attr>
## （2） 自定义View控件MyView，通过TypedArray自取自定义的declare-styleable属性组 ##
取得枚举是类型是通过TypeArray的getInt()函数取得。
	
	style = mTypedArray.getInt(R.styleable.CircleProgressBar_style, 0);

## （3）在布局文件中，使用该属性 ##
	
	android_custom:style="STROKE"

# 5 标志位、位或运算在attrs中的定义和使用（在Android横坚屏切换中，会有用到） #
## （1）定义格式 ##

	<declare-styleable name="名称">  //name:一般与自定义View类的名称相同
        <attr name="flagsName">      //name:标志位名称
            <flag name="flagKey" value="flagValue" />
        </attr>
	</declare-styleable>
	
实例：

	<declare-styleable name="SelfView">
        <attr name="flags">
            <flag name="stateUnspecified" value="0" />
            <flag name="stateUnchanged" value="1" />
            <flag name="stateHidden" value="2" />
            <flag name="stateAlwaysHidden" value="3" />
            <flag name="stateVisible" value="4" />
            <flag name="stateAlwaysVisible" value="5" />
            <flag name="adjustUnspecified" value="0x00" />
            <flag name="adjustResize" value="0x10" />
            <flag name="adjustPan" value="0x20" />
            <flag name="adjustNothing" value="0x30" />
        </attr>
	</declare-styleable>

## 2）、自定义View控件SelfView。通过TypedArray自取自定义的declare-styleable属性组 ##
	
	TypedArray typeArray;
	 int flags;
	 public SelfView(Context context, AttributeSet attrs) {
	  super(context, attrs);
	  typeArray = context.obtainStyledAttributes(attrs, R.styleable.SelfView, R.attr.flags, 0);
	  flags = typeArray.getInt(R.styleable.SelfView_flags, 0);
	 }

## 3）、在XML布局文件中，使用该SelfView自定义组件 ##
使用自定义View控件时，要引入命名空间
 
	<com.fs.myselfview.SelfView
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        test:flags="adjustPan|stateVisible">
    </com.fs.myselfview.SelfView>
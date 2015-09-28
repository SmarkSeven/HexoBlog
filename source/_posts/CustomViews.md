title: android自定义view之创建一个View类
date: 2015-09-26 11：30
comments: true
tags: [View,Android]
categories: [View,Android]

---

 
一个设计良好的自定义view和其他设计良好的类很像。封装了某个具有易用性接口的功能组合，这些功能能够有效地使用CPU和内存，并且十分开放的。但是，除了开始一个设计良好的类之外，一个自定义view应该：

符合安卓标准

提供能够在Android XML布局中工作的自定义样式属性。

发送可访问的事件

与多个Android平台兼容。

Android框架提供了一套基本的类和XML标签来帮您创建一个新的，满足这些要求的view。

本节讨论如何使用Android框架来创建View类的核心功能。

view的子类

Android框架中定义的所有view类都继承了View。您的自定义view也可以直接继承View，或者您为了节省时间也可以继承其他已存在的view子类，例如Button。

为了允许Android开发者工具（ADT）能够与您的view交互，您必须提供一个能够获取Context和作为属性的AttributeSet对象的构造函数。这个构造函数允许布局编辑器建立和编辑一个您的view的实例。


	class PieChart extends View {
    	public PieChart(Context ctx, AttributeSet attrs) {
    	    super(ctx, attrs);
    	}
	}
自定义属性

为了在您的用户接口添加内建的（Android自带的）view，您可以在一个XML元素中指定它，并通过元素的属性来控制其外观和行为。写的好的自定义view一样可以通过XML添加和设置样式。为了能够为您的自定义view添加这些行为，您必须：

在资源元素<declare-styleable>中为您的view定义自定义属性。

在您的XML布局中指定属性的值。

在运行时取得属性值。

将取回的属性值应用到您的view中。

本节讨论如何定义自定义属性并指定他们的值。下一节则是有关在运行时取得并应用这些值。

为了定义已经有属性，请在您的项目组添加<declare-styleable>资源。这些资源通常是放在res/values/attrs.xm文件里。如下是attrs.xml文件的一个例子：


	<resources>;
 	  <declare-styleable name="PieChart">
       <attr name="showText" format="boolean" />
       <attr name="labelPosition" format="enum">
           <enum name="left" value="0"/>
           <enum name="right" value="1"/>
       </attr>
  	 </declare-styleable>
	</resources>
这些代码声明了两个自定义属性:"showText"和"labelPosition"，他们属于一个叫做PieChart的样式实体。按照惯例， 样式实体的名字是和声明的自定义view类名是相同的。尽管遵循这个惯例不是绝对必要的，但很多有名的代码编写者都基于这个命名惯例来提供声明。

一旦您定义了自定义属性，您可以在布局XML文件中像内建属性一样使用它们。唯一的不同是，您的自定义属性属于不同的命名空间。它们属于http://schemas.android.com/apk/res/[your package name] 以取代默认的 http://schemas.android.com/apk/res/android命名空间。本例显示如何为PieChart使用这些定义过的属性：

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	   xmlns:custom="http://schemas.android.com/apk/res/com.example.customviews">
 	<com.example.customviews.charting.PieChart
 	    custom:showText="true"
  	   custom:labelPosition="left" />
	</LinearLayout>

为了不重复这个长的命名空间URI，本例使用了一个xmlns指示。这个指示指定了别名custom为命名空间http://schemas.android.com/apk/res/com.example.customviews您可以为您的命名空间选择任意的别名。

注意您用来向布局中添加自定义view的XML标签的名字。这是自定义view类的完全表述。如果您的view内是一个内部类，您必须使用 外部类的名字进一步限定它。例如，PieChart类有一个叫做PieView的内部类。为了使用这个类中的自定义属性，您必须使用标签 com.example.customviews.charting.PieChart$PieView。



当view从XML布局中创建了之后，XML标签中所有的属性都从资源包中读取出来并作为一个AttributeSet传递给view的构造函数。尽管从AttributeSet中直接读取值是可以的，但是这样做有一些缺点：

带有值的资源引用没有进行处理

样式并没有得到允许

取而代之的是，将AttributeSet传递给obtainStyledAttributes()方法。这个方法传回了一个TypedArray数组，包含了已经解除引用和样式化的值。

为了时能能够更容易的调用obtainStyledAttributes()方法，Android资源编译器做了大量的工作。res文件夹 中的每个<declare-styleable>资源，生成的R.java都定义了一个属性ID的数组以及一套定义了指向数组中的每一个属性 的常量。您可以使用预定义的常量从TypedArry中读取属性。下例是PieChart类是如何读取这些属性的：

	public PieChart(Context ctx, AttributeSet attrs) {
   		super(ctx, attrs);
   		TypedArray a = context.getTheme().obtainStyledAttributes(
        attrs,
        R.styleable.PieChart,
        0, 0);
    
  		 try {
    		   mShowText = a.getBoolean(R.styleable.PieChart_showText, false);
    		   mTextPos = a.getInteger(R.styleable.PieChart_labelPosition, 0);
   		} finally {
    		   a.recycle();
   		}
	}
注意,TypedArry对象是一个共享的资源，使用完毕必须回收它。



添加属性和事件

属性是控制view的行为和外观的强有力的方式，但是只有view在初始化后这些属性才可读。为了提供动态的行为，需要暴露每个自定义属性的一对getter和setter。下面的代码片段显示PieChart是如何提供showText属性的。


	public boolean isShowText() {
		return mShowText;
	}
    
	public void setShowText(boolean showText) {
    		 mShowText = showText;
    		  invalidate();
    		 requestLayout();
	}

注意，setShowText调用了invalidate()和requestLayout()。这些调用关键是为了保证view行为是可靠的。你必须在改变这个可能改变外观的属性后废除这个view，这样系统才知道需要重绘。同样，如果属性的变化可能影响尺寸或者view的形状，您需要请求一个新的布局。忘记调用这些方法可能导致难以寻找的bug。

自定义view同样需要支持和重要事件交流的事件监听器。例如，PieChart暴露了一个叫做OnCurrentItemChanged的自定义事件来通知监听器，用户旋转了饼状图来把焦点放在饼状图的另一部分。

忘记提供属性和事件是很容易的，尤其是当您是这个自定义view的唯一用户时。请花一些时间来仔细的定义您view的接口以减少未来维护时所耗费的时间。一个应该遵从的准则是：暴露您view中所有影响可见外观的属性或者行为。

您的自定义view应该支持很广的用户范围。这包括那些有残疾而使得他们不能看或者使用一个触摸屏。为了能够支持残疾人用户，您应该：

使用android:contentDescription属性标示您的输入控件。

在适当的时候通过调用sendAccessibilityEvent()发送无障碍事件。

支持辅助控制器，例如D-pad和轨迹球 （注：D-pad是上下左右按键放在一块的那种）

获得更多关于创建无障碍view的信息，请浏览Android Developers Guide 中的Making Applications Accessible 一章
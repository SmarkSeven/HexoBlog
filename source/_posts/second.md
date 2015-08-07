title: Android沉浸式状态栏 
date: 2015-08-05 10:12:14
comments: true
tags: [Android,随笔]
categories: Android

---
   **正常情况下手机状态栏的背景都是黑色的，和应用界面有和明显的色差，这样的视觉体验并不好。**

**非沉浸式状态栏：**
 ![非沉浸式状态栏](http://i.imgur.com/0fYNAK9.png) 


**沉浸式状态栏：**
 ![沉浸式状态栏](http://i.imgur.com/e0QBW8v.png)  

   
**好在Android在4.4中曾加了透明状态栏的功能，利用这个新特性可以实现Android的沉浸式状态栏。**

**要实现沉浸式状态栏的代码相当简单**
	
	if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            Window window = getWindow();
            View view = findViewById(R.id.img);
            window.addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
            int statusBarHeight = ScreenUtil.getStatusBarHeight(this.getBaseContext());
            view.setPadding(0, statusBarHeight, 0, 0);
            Toast.makeText(getApplicationContext(), " " + statusBarHeight, Toast.LENGTH_SHORT).show();
        }

**这里先获得状态栏的高度，然后设置FragLayout的padding属性，即：**

	view.setPadding(0, statusBarHeight, 0, 0);
**来达到最终的效果**

** 其中ScreenUtil.getStatusBarHeight(this.getBaseContext())的代码如下：**

	 /**
     * 用于获取状态栏的高度。 使用Resource对象获取（推荐这种方式）
     *
     * @return 返回状态栏高度的像素值。
     */
    public static int getStatusBarHeight(Context context) {
        int result = 0;
        int resourceId = context.getResources().getIdentifier("status_bar_height", "dimen",
                "android");
        if (resourceId > 0) {
            result = context.getResources().getDimensionPixelSize(resourceId);
        }
        return result;
    }
   
**布局文件如下：**


	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <FrameLayout
        android:id="@+id/img"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#FED928"
        android:clipToPadding="true">
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </FrameLayout>
	</RelativeLayout>

**通过这种方式实现的沉浸式状态栏的主要思想是，用一个FragLayout来包裹界面顶部内容，使FragLayout内容区域延伸到StateBar，然后通过设置FragLayout的paddingTop的值为StateBar的高度，让顶部内容恰好显示在StateBar的下方。**

<!-- 多说评论框 start -->
	<div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="Android沉浸式状态栏" data-url="http://seven.nodepie.com/2015/08/05/second/"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"sevenyun"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- 多说公共JS代码 end -->

<!-- 多说最新评论 start -->
	<div class="ds-recent-comments" data-num-items="5" data-show-avatars="1" data-show-time="1" data-show-title="1" data-show-admin="1" data-excerpt-length="70"></div>
<!-- 多说最新评论 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"sevenyun"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- 多说公共JS代码 end -->




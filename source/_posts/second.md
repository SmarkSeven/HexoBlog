title: Android沉浸式状态栏 
date: 2015-08-05 10:12:14
tags: Android
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
   
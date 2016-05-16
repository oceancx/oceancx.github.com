---
---

1. 去掉5.0 Button外边的阴影
		
	android:style="?android:attr/borderlessButtonStyle"


2. NDK support:

	http://ph0b.com/new-android-studio-ndk-support/


3. 怎么设置状态栏跟导航栏透明:

	if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
	    Window window = getWindow();
	    // Translucent status bar
	    window.setFlags(
	    	WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS, 
	    	WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
	    // Translucent navigation bar
	    window.setFlags(
	    	WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION, 
	    	WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION);
	}


4. ListView中存在ImageButton CheckBox等控件,Item获取不到焦点的解决方案:


	android:descendantFocusability

	Defines the relationship between the ViewGroup and its descendants when looking for a View to take focus.

	Must be one of the following constant values.
	<table>
		<tr>
			<td>Constant</td>	<td>Value</td> 	<td>Value</td>
		</tr>
		<tr>
			<td>beforeDescendants</td> 	<td>0</td>	<td>The ViewGroup will get focus before any of its descendants.</td>
		</tr>
		<tr>
			<td>afterDescendants</td> 	<td>1</td>	<td>The ViewGroup will get focus only if none of its descendants want it.</td>
		</tr>
		<tr>
			<td>blocksDescendants</td> 	<td>2</td>	<td>The ViewGroup will block its descendants from receiving focus.</td>
		</tr>
	</table>

5. Java正则表达式匹配出⺴⽹网⻚页图⽚片地址并替换 

		String patternStr="(?i)<img[^>]*?src=\"([])\"";
		Pattern pattern=Pattern.compile(patternStr);
		Matcher matcher = pattern.matcher(content);
		//如果匹配到了img 
		System.out.println("matcher.matches() =="+matcher.matches());
		if(matcher.matches()){
			result=content.replaceAll(matcher.group(1),(replaceHttp+matcher.group(1)));
		}


1. 怎么设置statusBar透明兼容4.4 / 5.x / 6.x

	/**
     * 设置statusBar透明兼容4.4 / 5.x / 6.x
     * 适用于有图片为头部的页面
     *
     * @param activity flag_status：0表示取消，WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS 表示透
     */
    public static void setStatusBarTranslucentCompat(Activity activity) {
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.KITKAT)
            return;

        Window window = activity.getWindow();
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            window.getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                    | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
        } else {
            window.setFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS,
                    WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        }
    }




1. 如何将CollapseToolbar的title给去掉
	[Show CollapsingToolbarLayout Title ONLY when collapsed](http://stackoverflow.com/questions/31662416/show-collapsingtoolbarlayout-title-only-when-collapsed)或者  

		collapsingToolbarLayout.setTitleEnabled(false);

2. PagerAdapter要实现两个方法:

		@Override
		public Object instantiateItem(ViewGroup container, int position) {
			ImageView im = new ImageView(container.getContext());
			ViewGroup.LayoutParams params = new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
			container.addView(im, params);
			im.setScaleType(ImageView.ScaleType.FIT_XY);
			Glide.with(container.getContext()).load(imgUrls[position]).into(im);
			return im;
		}

		@Override
		public void destroyItem(ViewGroup container, int position, Object object) {
			container.removeView((ImageView) object);
		}













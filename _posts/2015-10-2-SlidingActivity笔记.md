---
---

### sliding_activity.xml 布局分析

	MultiShrinkScroller(id:"multiscroller")
		LinearLayout("vertical")
			View(id:"transparent_view" 150dp)
			RelativeLayout
				FrameLayout(id:"toolbar_parent")
					layout:@layout/sliding_header
				layout:@layout/sliding_content(below:"toolbar_parent")
				FloatingActionButton(below:"toolbar_parent" id:("fab"))


#### sliding_header.xml

	FrameLayout
		View(id:"photo_background" "gone")
		ImageView(id:"photo" clickable:false)
		View(id:"photo_touch_intercept_overlay")
		View(id:"title_gradient" layout_gravity:"bottom")
		View(id:"action_bar_gradient" layout_gravity:"top")
		Toolbar(id:"toolbar" layout_gravity:"end|top")

#### sliding_content.xml

	TouchlessScrollView(id:"content_scroller")
		FrameLayout(id:"content_container" orientation:"vertical")


### MultiShrinkScroller
	scroller = new Scroller(context,INTERPOLATOR)
	touchSlop = configuration.getScaledTouchSlop()
	minimumVelocity
	maximumVelocity
	
	toolbarElevation = 4.5dp
	isTwoPanel ture:false

##### 常量
	transparentStartHeight 150dp
	maximumTitleMargin 16dp
	dismissDistanceOnScroll 100dp  滑动超过100dp时dismiss
	dismissDistanceOnRelease 40dp  释放时距离超过40dp时dismiss
	snapToTopSlopHeight      33dp
	photoRatio	0.7
	actionbarSize 55dp
	minimumHeaderHeight 55dp

##### View变量
	
	scrollView(id:content_scroller)
	scrollViewChild(id:content_container)
	toolbar		(id: toolbar_parent)
	photoViewContainer	(id:toolbar_parent)
	transparentView		(id:transparent_view)
	largeTextView		(id:large_title)
	invisiblePlaceholderTextView	(id:placeholder_textview)
	startColumn			(id:empty_start_column)
	
#### 函数名解释

	getScrollUntilOffBottom  getScroll | Until | OffBottom 返回从底部划出来的高度 正值


**研究速记**	

1. UpdateFab
2. 退出时上面150dp的transparentView透明度变化原因 winscrim
3. scrollOffBottom 原因 translateAnimation 
	 listener.onStartScrollOffBottom();
	   exitAnimationListner->  listener.onScrolledOffBottom();
4.  this.isOpenImageSquare 是否expandHeader
5. expandHeader原理  
	给header做动画：ObjectAnimator animator = ObjectAnimator.ofInt(this, "headerHeight",
                    maximumHeaderHeight);
    给scrollView做动画 ：    
     ObjectAnimator.ofInt(scrollView, "scrollY", -scrollView.getScrollY()).start();	
6 .     collapsedTitleStartMargin = ((Toolbar) findViewById(R.id.toolbar)).getContentInsetStart();  16dp

7.  updateFabStatus(scrollView.getScrollY()); 滑动超过100dp时小时 小于时 显示

8. maximumHeaderHeight 300dp intermediateHeaderHeight 180dp

9. setHeaderHeight  非常关键的方法
	{
	   toolbarLayoutParams.height = height;
        toolbar.setLayoutParams(toolbarLayoutParams);
		 updatePhotoTintAndDropShadow();
        updateHeaderTextSizeAndMargin();
	}

9. getMaximumScrollableHeaderHeight{   return isOpenImageSquare ? maximumHeaderHeight : intermediateHeaderHeight;}

10. maximumHeaderTextSize = largeTextView.getHeight(); 105px (35dp	)( Nexus N5)

**MultiShrinkScroller滑动指南**

	@Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        if (velocityTracker == null) {
            velocityTracker = VelocityTracker.obtain();
        }
        velocityTracker.addMovement(event);

        // The only time we want to intercept touch events is when we are being dragged.
        return shouldStartDrag(event);
    }
  

在onInterceptTouchEvent中返回shouldStartDrag


    private boolean shouldStartDrag(MotionEvent event) {
       
        switch (event.getAction()) {
            // If we are in the middle of a fling and there is a down event, we'll steal it and
            // start a drag.
            case MotionEvent.ACTION_DOWN:
                updateLastEventPosition(event);
                if (!scroller.isFinished()) {
                    startDrag();
                    return true;
                } else {
                    receivedDown = true;
                }
                break;

            // Otherwise, we will start a drag if there is enough motion in the direction we are
            // capable of scrolling.
            case MotionEvent.ACTION_MOVE:
                if (motionShouldStartDrag(event)) {
                    updateLastEventPosition(event);
                    startDrag();
                    return true;
                }
                break;
        }

        return false;
    }

updateLastEventPosition 记录每次按下的位置

startDrag {   isBeingDragged = true;scroller.abortAnimation();}


    private boolean motionShouldStartDrag(MotionEvent event) {
        final float deltaY = event.getY() - lastEventPosition[1];
        return deltaY > touchSlop || deltaY < -touchSlop;
    }
		



    @Override
    public boolean onTouchEvent(MotionEvent event) {
       if (!isBeingDragged) {
            if (shouldStartDrag(event)) {
                return true;
            }
            if (action == MotionEvent.ACTION_UP && receivedDown) {
            	//中间没有MOVE event 因此是click
                receivedDown = false;
                return performClick();
            }
            return true;
        }

        switch (action) {
            case MotionEvent.ACTION_MOVE:
                final float delta = updatePositionAndComputeDelta(event);
                scrollTo(0, getScroll() + (int) delta);
                receivedDown = false;

                if (isBeingDragged) {
                    final int distanceFromMaxScrolling = getMaximumScrollUpwards() - getScroll();
                    if (delta > distanceFromMaxScrolling && Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
                        // The ScrollView is being pulled upwards while there is no more
                        // content offscreen, and the view port is already fully expanded.
                        edgeGlowBottom.onPull(delta / getHeight(), 1 - event.getX() / getWidth());
                    }

                    if (!edgeGlowBottom.isFinished()) {
                        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
                            postInvalidateOnAnimation();
                        } else {
                            postInvalidate();
                        }
                    }

                    if (shouldDismissOnScroll()) {
                        scrollOffBottom();
                    }

                }
                break;

            case MotionEvent.ACTION_UP:
            case MotionEvent.ACTION_CANCEL:
                stopDrag(action == MotionEvent.ACTION_CANCEL);
                receivedDown = false;
                break;
        }

        return true;
    }

updatePositionAndComputeDelta 返回滑动delta，对下拉的Move事件做惰性处理

	final int distanceFromMaxScrolling = getMaximumScrollUpwards() - getScroll();
	if (delta > distanceFromMaxScrolling && Build.VERSION.SDK\_INT >= Build.VERSION_CODES.LOLLIPOP) {
		// The ScrollView is being pulled upwards while there is no more
		// content offscreen, and the view port is already fully expanded.
		edgeGlowBottom.onPull(delta / getHeight(), 1 - event.getX() / getWidth());
	}

	private int getMaximumScrollUpwards() {
            return transparentStartHeight
                    // How much the ScrollView can scroll. 0, if child is smaller than ScrollView.
                    + Math.max(0, scrollViewChild.getHeight() - getHeight());
    }




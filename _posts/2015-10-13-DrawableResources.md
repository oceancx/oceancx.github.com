---
tags: [drawable]
categories: [android]
---
###DrawableResources
可绘制资源是一种通用的概念，所指的一方面是能用
`getDrawable(int)`等诸如此类的APIs来得到的图形，
另一方面是指应用在`anroid:drawable`或`android:icon`等
属性上的XML文件中的图形。下面介绍几种不同类型的drawables:

- Bitmap File
	
	一种位图图形文件。(*.png*,*.jpg*,or *.gif*)。对应的class是 *BitmapDrawable*

- Nine-Patch File

	一种PNG文件拥有一块可伸缩的区域，这块区域能根据图片的内容来伸缩（*.9.png*）。对应的class是 *NinePatchDrawable*

- Layer List
	这种Drawable用于管理一个由多个Drawable组成的数组。数组里的Drawable绘制的顺序由位置觉得
A Drawable that manages an array of other Drawables. These are drawn in array order, so the element with the largest index is be drawn on top. Creates a LayerDrawable.

- State List
An XML file that references different bitmap graphics for different states (for example, to use a different image when a button is pressed). Creates a StateListDrawable.
- Level List
An XML file that defines a drawable that manages a number of alternate Drawables, each assigned a maximum numerical value. Creates a LevelListDrawable.
Transition Drawable
An XML file that defines a drawable that can cross-fade between two drawable resources. Creates a TransitionDrawable.
- Inset Drawable
An XML file that defines a drawable that insets another drawable by a specified distance. This is useful when a View needs a background drawble that is smaller than the View's actual bounds.
- Clip Drawable
An XML file that defines a drawable that clips another Drawable based on this Drawable's current level value. Creates a ClipDrawable.
- Scale Drawable
An XML file that defines a drawable that changes the size of another Drawable based on its current level value. Creates a ScaleDrawable
- Shape Drawable
An XML file that defines a geometric shape, including colors and gradients. Creates a ShapeDrawable.





#The Transitions Framework
#转换框架包含:

1. _Group-level animations_ 组级别动画

    Applies one or more animation effects to all of the views in a view hierarchy.
    对ViewHierarchy中的多个视图应用多种动画效果

2. _Transition-based animation_

    Runs animations based on the changes between starting and ending view property values.
    根据开始结束的property来应用动画 

3. _Built-in animations_

    Includes predefined animations for common effects such as fade out or movement.
    对公用的特效比方说淡出和移动,就用这种提前预定义好的动画

4. _Resource file support_
    
    Loads view hierarchies and built-in animations from layout resource files.

5. _Lifecycle callbacks_

    Defines callbacks that provide finer control over the animation and hierarchy change process.





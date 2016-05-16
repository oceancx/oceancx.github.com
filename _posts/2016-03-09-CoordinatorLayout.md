---
---

1. CoordinatorLayout is a super power FrameLayout
2. CoordinatorLayout 包含的子节点一般 : RecyclerView FloatingActionButton AppLayout SnackBar
3. RecyclerView <-> AppBarLayout AppBarLayout <-> FAB <-> SnackBar
4. Behavior建立了两个视图之间的RelationShip,Bahavior定义方式有两种,1在自定义View的类名上加注解,例如
		
		@CoordinatorLayout.Bahavoir(FooterBarBehavior)

	另一种就是在XML文件里:

		app:behavior="....FooterBarBehavior"

5. 要自定义Behavior,一般要重写以下方法:
	1. layoutDependsOn
		这个方法是你用来定义去附加的View
	2. onLayoutChildren
	3. onMeasureChildren
	4. onDependentViewChanged
	5. onDependentViewRemoved


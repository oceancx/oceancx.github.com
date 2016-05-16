---
---
0和1是计算机的基础,数理逻辑中0和1代表两种状态,真与假.0和1看似简单,其实变化无穷.
今天我就来聊聊android控件中拥有着0和1这种特性的魔力控件checkbox.

先来讲讲Checkbox的基本使用.在XML中定义.


	<?xml version="1.0" encoding="utf-8"?>
	<CheckBox xmlns:android="http://schemas.android.com/apk/res/android"
		android:id="@+id/cbx"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:checked="false" />


在Activity中使用


	CheckBox cbx = (CheckBox) findViewById(R.id.cbx);
	cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	    @Override
	    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	    	//do something
	    }
	});


no big deal,很简单.要注意的是,CheckBox本身是一个视图,是展示给用户看的,因此我们要用数据来控制它的展示.所以,我们的CheckBox在Activity中要这么写


	boolean isChecked= false;
	CheckBox cbx = (CheckBox) findViewById(R.id.cbx);
	cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	  	@Override
	    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	        if(isChecked){
	           	//do something
	        }else{
	            //do something else
	        }
	    }
	});
	cbx.setChecked(isChecked);


这样,我们改变数据的时候,视图的状态就会跟着数据来做改变了.注意,监听器一定要这setChecked之前设置,这样才能体现出来数据来控制视图的展示.

单独用CheckBox很easy,接下来,复杂的情况来啦,CheckBox如何跟ListView/RecyclerView(以下简称LV/RV)配合使用.这就不能简单的考虑问题啦,要知道LV/RV中的视图个数跟数据集的里面的数据并不一致,真正的视图个数远小于数据集中数据项的个数.因为屏幕上在列表中的视图是可以复用的.关于这点,初学者请看[The World Of ListView](http://v.youku.com/v_show/id_XOTE1MzI0MDU2.html).由于LV/RV的复用机制,如果我们没有用数据来控制CheckBox状态的话,将会导致CheckBox的显示在列表中错乱.比方说你只对第一个Item中的CheckBox做了选中操作,当列表向上滚动的时候,你会发现,下面的Item中居然也会有被选中的.当然,我刚学Android时候也遇到过这种情况,问题的关键就在于要用数据来控制视图的显示.因此在getView/onBindViewHolder中,我们应该这么写.


	holder.cbx.setTag(item);
	holder.cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	    @Override
	    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	        Item item =(Item) buttonView.getTag();
	        if(isChecked){
	            item.setCheckState(true);
	            //do something
	        }else{
	            item.setCheckState(false);
	            //do something else
	        }
	    }
	});
	cbx.setChecked(item.getCheckState());


这种方法基本正确,但是我们要额外的给每个数据项里面添加一个字段来记录状态,这代价就有点大了.一是不必这么做,二是这会导致本地数据结构跟服务端结构不一致.通常,列表中使用CheckBox的话,很明显是把选中的item给记录下来,可以这么理解,选中的状态是列表给的,而item本身应该是无状态的.那么,如果重构我们的代码呢,Android为我们提供了一种完美的数据结构来解决这个问题.你可以用SparseArray<Boolean>,也可以用SparseBooleanArray,我现在习惯使用SparseBooleanArray,ok,请看代码


	private class Adapter extends RecyclerView.Adapter<RecyclerView.ViewHolder>{
	    SparseBooleanArray mCheckStates=new SparseBooleanArray();
	    @Override
	    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
	        //...
	    }
	    @Override
	    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
	        holder.cbx.setTag(position);
	        holder.cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	            @Override
	            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	                int pos =(int)buttonView.getTag();
	                if(isChecked){
	                    mCheckStates.put(pos,true);
	                    //do something
	                }else{
	                    mCheckStates.delete(pos);
	                    //do something else
	                }
	            }
	        });
	        cbx.setChecked(mCheckStates.get(position,false));
	    }
	    @Override
	    public int getItemCount() {
	        //...
	    }
	}




这样列表就能正常显示了,而且在你选中CheckBox的时候,会自动触发onCheckedChanged来对mCheckStates来进行更新.此时,如果你想用程序来选中某个item的时候,那么直接这样就行了.


	mCheckStates.put(pos,true);
	adapter.notifyDatasetChanged();


如果我们想要取出列表列中所有的数据项,那么有了SparseBooleanArray,这个就非常好办啦.


	ArrayList<Item> selItems=new ArrayList<>();
	for(int i=0;i < mCheckStates.size();i++){
	    if(mCheckStates.valueAt(i)){
	        selItems.add(allItems.get(mCheckStates.keyAt(i)));
	    }
	}


竟然是如此的节省空间和时间,这样的代码谁不喜欢呢.但是,这还不完美.
如果我们列表中要求将某个Item的CheckBox的check状态不可改变的话,那么我们该怎么办呢.首先的话,你可以用setEnable(false)来禁用cbx,另一种方法呢,就是比较笨的方法啦,我们可以在onCheckedChanged中把buttonView设置成!isCheck的不就行了嘛.


	holder.cbx.setTag(position);
	holder.cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	    @Override
	    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	    	int pos = (CheckBox) buttonView.getTag();
	    	if( pos == immutablePos)
	        	buttonView.setChecked(!isChecked);
	        //...
	    }
	});


但是这么写的话,就会调用buttonView的onCheckedChanged,其实buttonView就是外面的holder.cbx,这就会造成死循环.因此我们如果用cbx本身去改变状态的话,那么一定要加锁.


	boolean lockState=false;
	holder.cbx.setTag(position);
	holder.cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	    @Override
	    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	    	if(lockState)return;
	    	//...
	    	//不让cbx改变状态.
	    	lockState=true;
	        buttonView.setChecked(!isChecked);
	        lockState=false;
	        //...
	    }
	});


对cbx加锁其实还是挺常用的,比方说在onCheckedChanged中,你要发一个请求,而请求的结果反过来会更新这个cbx的选中状态,你就必须要用lockState来直接改变cbx的状态了,以便于cbx的状态跟mCheckStates里面的是一致的.

mada mada,还有一种情况,如果在onCheckedChanged的时候,isChecked跟mCheckStates.get(pos)一致的话,这会导致什么情况呢.
		

	@Override
	public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
		int pos =(int)buttonView.getTag();
	    if(isChecked){
			mCheckStates.put(pos,true);
		    //do something
		}else{
		    mCheckStates.delete(pos);
		    //do something else
		}
	}


这就会让你的//do something做两次,这么做就是没有必要的啦,而且如果你的//do something是网络请求的话,这样就会导致更大问题.所以,我们有必要对这种情况做过滤.


	@Override
	public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	    if(lockState)return;
	    int pos =(int)buttonView.getTag();
	    if(mCheckStates.get(pos,false) == isChecked)return;
	    if(isChecked){
	    	mCheckStates.put(pos,true);
	    	//do something
	    }else{
	    	mCheckStates.delete(pos);
	    	//do something else
	    }
	}


好啦,如果你能将CheckBox跟SparseBooleanArray联用,并且能考虑到加锁和过滤重选的话,那么说明你使用CheckBox的姿势摆正了.但是,我要讲的精彩的地方才刚刚开始.

一个列表仅仅能让用户上滚下滑,那是最简单的使用,通常,由于列表项过多,产品会给列表项添加筛选的功能,而通常我们做筛选,会考虑到使用Spinner来做,但是,有用android自身提供的Spinner扩展性太差,而且长得丑,往往导致大家一怒之下,弃而不用.我呢,就是这么干的.经过本人的奇思妙想,本人终于找到了一种很巧妙的机制来很优雅的实现列表的筛选.下面我就来给大家分享一下.

接下来清楚我们今天的另一位主角,那就是PopupWindow([介绍](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/0702/1627.html)),我先介绍一下原理,首先给CheckBox设置setOnCheckedChangeListener,然后在onCheckedChanged里面,isChecked分支中弹出PopupWindow,!isChecked中,读取Popupwindow中的结果,用新的筛选条件来更新列表.ok,上代码:

MainActivity:


	public class MainActivity extends AppCompatActivity {

	    String[] filter_type_strs = {"音乐", "书籍", "电影"};
	    CheckBox cbx;
	    private boolean lockState=false;
	    int current_filter_type=0;
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        cbx = (CheckBox) findViewById(R.id.cbx);

	        cbx.setText(filter_type_strs[current_filter_type]);
	        cbx.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
	            @Override
	            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
	                if (lockState) return;
	                try {
	                    if (isChecked) {
	                        //此处传入了cbx做参数
	                        PopupWindow pw = new FilterLinePw(buttonView.getContext(), cbx, filter_type_strs);
	                        pw.showAsDropDown(cbx);
	                    } else {
	                        //此处的buttonView就是cbx
	                        Integer pos = (Integer) buttonView.getTag();
	                        if (pos == null || pos == -1) return;
	                        current_filter_type = pos;

	                        Toast.makeText(MainActivity.this, "搜索"+filter_type_strs[current_filter_type], Toast.LENGTH_SHORT).show();
	                    }
	                } catch (NullPointerException e) {
	                    //以防万一
	                    lockState = true;
	                    buttonView.setChecked(!isChecked);
	                    lockState = false;
	                }
	            }
	        });

	    }
	}



FilterLinePw:


	public class FilterLinePw extends PopupWindow {
	    RadioGroup radioGroup;
	    CheckBox outCbx;

	    //为动态生成radioButton生成id
	    int[] rbtIds = {0, 1, 2};

	    public FilterLinePw(Context context, CheckBox outCbx, String[] items) {
	        super(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
	        View contentview = LayoutInflater.from(context).inflate(R.layout.filter_line_popupwindow, null);
	        setContentView(contentview);
	        setFocusable(true);
	        setOutsideTouchable(true);
	        this.outCbx = outCbx;
	        contentview.setOnKeyListener(new View.OnKeyListener() {
	            @Override
	            public boolean onKey(View v, int keyCode, KeyEvent event) {
	                if (keyCode == KeyEvent.KEYCODE_BACK) {
	                    dismiss();
	                    return true;
	                }
	                return false;
	            }
	        });
	        contentview.setFocusable(true); // 这个很重要
	        contentview.setFocusableInTouchMode(true);
	        contentview.setOnClickListener(new View.OnClickListener() {
	            @Override
	            public void onClick(View v) {
	                dismiss();
	            }
	        });

	        init(context, contentview,items);

	    }

	    private void init(Context context, View contentview, String[] items) {
	        /**
	         * 用传入的筛选条件初始化UI
	         */
	        radioGroup = (RadioGroup) contentview.findViewById(R.id.filter_layout);
	        radioGroup.clearCheck();
	        if (items == null) return;
	        for (int i = 0; i < items.length; i++) {

	            RadioButton radioButton = (RadioButton) LayoutInflater.from(context).inflate(R.layout.line_popupwindow_rbt, null);
	            radioButton.setId(rbtIds[i]);
	            radioButton.setText(items[i]);

	            radioGroup.addView(radioButton, -1, radioGroup.getLayoutParams());

	            if (items[i].equals(outCbx.getText())) {
	                outCbx.setTag(i);
	                radioButton.setChecked(true);
	            }

	        }
	        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
	            @Override
	            public void onCheckedChanged(RadioGroup group, int checkedId) {
	                dismiss();
	            }
	        });
	    }

	    //重点内容,重写dismiss();
	    @Override
	    public void dismiss() {
	        if (outCbx != null && outCbx.isChecked()) {
	            int id = radioGroup.getCheckedRadioButtonId();
	            RadioButton rbt = (RadioButton) radioGroup.findViewById(id);
	            Integer old_tag = (Integer) outCbx.getTag();
	            if (old_tag == null) {
	                super.dismiss();
	                return;
	            }
	            if (old_tag != id) {
	                outCbx.setTag(id);
	                outCbx.setText(rbt.getText());
	            } else {
	                outCbx.setTag(-1);
	            }
	            //下面执行之后,会执行MainActivity中的onCheckedChanged里的否定分支
	            outCbx.setChecked(false);
	        }
	        super.dismiss();
	    }
	}


效果图:

<img src="{{site.production_url}}/image/cbx/1.gif" >

[源码](https://github.com/oceancx/Android-CheckBox)

简单解释一下:其实重点在PopupWindow里面,MainActivity的CheckBox作为参数传递到了
PopupWindow里.首先,用户点击MainActivity的CheckBox,接着会执行isChecked分支,这样PopupWindow就展示给了用户,这样用户操作的环境就到了PopupWindow里面,等用户选择好筛选条件后,PopupWindow就把筛选条件设给outCbx,然后改变outCbx状态,从而触发MainActivity中onCheckedChanged中的否定分支,此时展示的是一个Toast,实际应用中可以是一个网络请求.同时,由于PopupWindow的代码并没有阻塞操作,所以会接着执行下一句 super.dismiss(),这样你在MainActivity就不用担心PopupWindow的关闭问题啦.最后,在MainActivity中还加入了try-catch来以防万一,这种机制真是太神奇啦.这种机制把筛选操作从Activity中分离了出来,以后我们写筛选可以完全独立于Activity啦,真的是一种很软件工程的做法.

随后我会把其他筛选的情况开源,但是最精妙的原理就在于这个简单的例子上.各位看完之后不妨亲自动手试试,感受一下.

好啦,精彩的地方讲完了,是不是不过瘾啊.好吧,最后,我再拿点私房菜出来.
CheckBox是继承自TextView,很多时候,我们的CheckBox的button属性设置的图片都不大,这就导致点击CheckBox的区域也小,因此,我们需要用到TouchDelegate来扩大CheckBox的可点击区域
上代码:


	public class FrameLayoutCheckBox extends FrameLayout {
	    CompoundButton cbx;

	    public FrameLayoutCheckBox(Context context) {
	        super(context);
	    }

	    public FrameLayoutCheckBox(Context context, AttributeSet attrs) {
	        super(context, attrs);
	    }

	    public FrameLayoutCheckBox(Context context, AttributeSet attrs, int defStyleAttr) {
	        super(context, attrs, defStyleAttr);
	    }

	    private CheckBox findCheckBox(View view) {
	        //无递归广度优先遍历寻找CheckBox - -!我只是想重温一下C
	        ArrayList<View> views = new ArrayList<>();
	        views.add(view);
	        while (!views.isEmpty()) {
	            View c = views.remove(0);
	            if (c instanceof CheckBox) {
	                return (CheckBox) c;
	            } else if (c instanceof ViewGroup) {
	                ViewGroup fa = (ViewGroup) c;
	                for (int i = 0; i < fa.getChildCount(); i++) {
	                    views.add(fa.getChildAt(i));
	                }
	            }
	        }
	        return null;
	    }

	    @Override
	    protected void onFinishInflate() {
	        super.onFinishInflate();
	        if (getChildCount() > 0) {
	            View child = findCheckBox(this);
	            if (child instanceof CompoundButton) cbx = (CompoundButton) child;
	        }
	    }

	    @Override
	    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
	        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
	        if (cbx != null) {
	            Rect bounds = new Rect(getPaddingLeft(), getPaddingTop(), getPaddingLeft() + getMeasuredWidth() + getPaddingRight(), getPaddingTop() + getMeasuredHeight() + getPaddingBottom());
	            TouchDelegate delegate = new TouchDelegate(bounds, cbx);
	            setTouchDelegate(delegate);
	        }
	    }
	}


这个类可以当成FrameLayout,我们可以把CheckBox放里面,然后CheckBox的点击区域就是整个FrameLayout的区域啦.当然这个类也适用于RadioButton,但是你不能放多个CompoundButton在里面(- -!)

ok,今天的详解就到这儿了.视反馈程度更新博文和[源码](https://github.com/oceancx/Android-CheckBox)哦!






---
title: "如何做字幕"
---

作为一个程序员,英语是要过关的.而提高英语有一个很好的方式就是来做字幕.
今天,我来分享一下自己做字幕的经验.


## 工欲善其事,必先利其器.

[Aegisub][_aegisub]是你的不二之选.

**开源!** **免费!** **好用!** **有中文!** **还需要更多的理由来爱它吗?**


## 做事情要有方法论,谋定而后动

做字幕分三步:

1. 打时间轴
2. 听写,翻译
3. 审阅,修正,后期加工

当然你也可以边做翻译边打时间轴

ok,什么是打时间轴呢?

你的Aegisub下载好,安装后之后.用它打开你要做字幕的视频.

<img src="{{site.production_url}}/image/zm/aeg_win.png" width="1018">

点击 视频->关闭视频.

<img src="{{site.production_url}}/image/zm/EBE4DABD-4597-4C7B-B064-0C18E41DEC2D.png" width="1018">

这里,我们看的到就是时间轴了.

<img src="{{site.production_url}}/image/zm/72A6AF21-CCAD-47AD-9078-AE10F2945AF6.png" width="1018">

显而易见,时间轴的长度就是视频的长度.
我们说的打轴,就是将时间轴进行分割成一段一段的.
分割出的每一段都对应着一段字幕.
例如,我把我翻译过的视频分成了916段.
<img src="{{site.production_url}}/image/zm/AFE3E59C-6B01-414E-984F-25375B1F62E1.png" width="1018">

时间段并不总是连续的,时间段之间空出来的时间,字幕就不必显示.当然,你可以考虑利用这点时间插播点广告啥的.

每个时间段的长度也不总是相同的,这是显而易见的.你要对说话人的内容进行断句,合理拆分句子,尽量保证字幕的字数不超过字幕的行宽.如果超过了,就要考虑对时间轴进行调整了.

打轴其实对字幕质量影响挺大的,一方面,你可以先打轴,再做字幕.另一方面,你可以边做边打轴.两种方法我都用过.我也说不上哪个更好.先打轴的话,你肯定要先把视频给听一遍,才能把轴打完.这时候,你再回头做的时候,打轴听的一遍就浪费了.我感觉好处是这样打出来的轴更准确,时间段的选择会更平均一点.边做边打的话,你可能做着省时点,但是时间段之间的方差可能会更大点.


## Get your hands dirty,do something,follow me!


使用Aegisub打轴非常方便,首先,用Aegisub打开一个视频文件.
用鼠标在时间轴上拖拽一段距离,按下回车键,这个时间段就打好了.

当然,我作为一个过来人,把我的经验传授给你吧.先点击一下编辑框,然后狂按10几下回车,接下来点击一下时间轴,就可以用尝试一下我们的快捷键了.

<img src="{{site.production_url}}/image/zm/aegisub_1.gif" width="1018">

>Z , X 用于时间段的上下移动

<img src="{{site.production_url}}/image/zm/aegisub_2.gif" width="1018">

>A , F 用于时间轴的左移 右移 

<img src="{{site.production_url}}/image/zm/aegisub_3.gif" width="1018">

>C , V 用于扩展时间段,C是向左扩展,V是向右扩展

<img src="{{site.production_url}}/image/zm/aegisub_4.gif" width="1018">

>空格键,R,B 作用是一样的,都是播放当前时间段

<img src="{{site.production_url}}/image/zm/aegisub_6.gif" width="1018">

>回车键 提交对此时时间段的修改

<img src="{{site.production_url}}/image/zm/aegisub_5.gif" width="1018">

最常用的就是这些了.多用用就熟了.

打好了时间轴,咱就要开始进行字幕制作了.
之前我用过Subtitle Workshop来做字幕,
这个软件在做字幕的时候,是带着视频的,这样你的字幕修改提交后,
立马就能在视频上显示出来.这样是很直观的.
但是当我刚开始用Aegisub的时候,我点打开文件,发现
只有一段音频的时间轴.我花了很大劲才找到开视频的方法.
后来才发现,做字幕只要音频就行了,开着视频反而会降低效率.
等把字幕做好后,调效果的时候才会用到开启视频.
因此,我们在做字幕的时候,把视频关了就好,你的全部工作
只在音频的时间轴上和编辑框.

我翻译视频的时候,一般是先打轴,听写英文,一遍下来之后,再来翻译.
这里要注意的是,英文字母是可以自适应行宽的,比方说你的字幕长度超过了
视频宽度的时候,字幕显示的时候是可以自动换行的.
而中文并没有这个特性,因此,你要依据实际显示的效果,来主动设定换行符.
**\N**-> 是一个强制换行符. 快捷键是 shift+Enter

因此一行中英混排的字幕一般是这样子的.

<img src="{{site.production_url}}/image/zm/9647DD9C-3D98-4C0E-80F1-DFA28A57556C.png" width="1018">


## 字幕样式制作


先科普一下srt,ass字幕的区别.简单点,srt字幕是最原始的字幕,只能显示纯文本,
而ass不光能显示文本,可以定制很多字幕转换的效果,比方说动画什么的,还有字体显示的特性增强
了不少,字体会发光还能带阴影什么的.而aegisub就是转门做ass字幕的软件.刚我提到的在aegisub中都可以轻松实现.

**推荐** [\[字幕软件Aegisub ASS代码使用指南\]\[ASS Tags of Aegisub\]by和路雪。V1.2](http://pan.baidu.com/s/1c0Ujzc0)

实战演示一下,为了要做中英字幕,
咱们需要准备两套样式,一种用来显示英文,一种用来显示中文.
点击 样式管理器, 新建,取个名字,然后设定一下字体 字号 颜色等等
建立好两份字幕后就保存.

回到字幕编辑框这里,点击样式,就可以用刚才的样式了.
<img src="{{site.production_url}}/image/zm/aegisub_7.gif" width="1018">

但是这样,我们一行字幕只能使用一种样式.这时,我们就要用到**{\ren}**来重新指定样式
这样一行字幕就完美了.

当然,这样换很麻烦,我一般是打完轴之后,在box中选中所有行,来设置好中文样式,
<img src="{{site.production_url}}/image/zm/aegisub_8.gif" width="1018">
然后用sublime的多行编辑(快捷键:shift+command+l),打开字幕文件,每行添加**\N{\ren}**这样 **\N**之前的放中文字幕,
**{\ren}**之后放英文字幕即可.

<img src="{{site.production_url}}/image/zm/aegisub_9.gif" width="1018">

再次打开如图:
<img src="{{site.production_url}}/image/zm/946C456D-C740-4F33-915C-E016E772670D.png" width="1018">

有时候,我们需要插播一些广告或者显示一些注释.这时候,我们可以在注释显示的时刻附近的时间段上,
右键->插入行 ,然后对这行的时间段进行调整,然后把内容插入到这行即可.
<img src="{{site.production_url}}/image/zm/30156FF4-94A0-4D6E-B947-4D4D54CF50CA.png" width="1018" >
这样,我们翻译完一个视频以后,我们就可以点击保存了,保存的时候要注意,
设定字幕的视频分辨率,这样置一般跟你的视频分辨率一样,这样你调出来的效果才正确.
在 文件->配置 里面设定分辨率
<img src="{{site.production_url}}/image/zm/D8B60527-AB3B-4999-B5FB-C2E528E19541.png" width="1018">

补充一点,关于字幕的特效,动画,什么的,内容全在[\[字幕软件Aegisub ASS代码使用指南\]\[ASS Tags of Aegisub\]by和路雪。V1.2](http://pan.baidu.com/s/1c0Ujzc0)这里面,一定记得要下载下来看看哦!我就不做演示了.


## 将字幕写入视频


以上的步骤,我们只是做出了字幕文件,这样离线观看是没问题的.
但是如果要上传的视频网站,分享出去的话,咱们就要把字幕给烧录到视频中.
在osx下,推荐[mp4tools](http://www.emmgunn.com/mp4tools-home/),[mkvtools](http://www.emmgunn.com/mkvtools-home/),[avitools](http://www.emmgunn.com/avitools-home/)等系列产品,
当然这些产品是收费的,这个没办法. 但是,你懂的 ^_6!
用起来很方便,把视频字幕,拖到里面即可,然后设定好码率(一般遵从优酷上传标准,不然是没有高清,超清的),点击convert即可.


## 相关链接

[Aegisub官方手册](http://docs.aegisub.org/3.2/)

[\[字幕软件Aegisub ASS代码使用指南\]\[ASS Tags of Aegisub\]by和路雪。V1.2](http://pan.baidu.com/s/1c0Ujzc0)

[mp4tools](http://www.emmgunn.com/mp4tools-home/),[mkvtools](http://www.emmgunn.com/mkvtools-home/),[avitools](http://www.emmgunn.com/avitools-home/)

[_aegisub]: http://www.aegisub.org/ "Optional Title Here"












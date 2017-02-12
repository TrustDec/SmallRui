---
title: ViewPagerAndroid
---

此章节的Demo大概是我最用心的一篇,以React Native内部版本：0.37源代码为准,如果你认真看完,会发现没有想象中那么复杂,反而简单的让你不知所云~

<!-- More -->

>允许在子视图之间左右翻页的容器.
>子容器为单独的页且会被拉伸填满.
><span style='color:red'>子视图必须是纯View，不能自定义容器.</span>

<font style='color:green;'>本章节代码已上传Github</font>
>URL:https://github.com/TrustTheBoy/React-Native-BBS
>找不到的请聊天区留言.
><span style='color:red'>效果图在文章最后.</span>
>时间太紧,页面没怎样美化但是该涉及道德属性一个都不拉.

官网提供的属性：

<span style='color:#188eee;font-size:15;font-weight:bold'>initialPage</span>
>加载到页面显示第几个View,下标始为0.
>可以用setPage函数来翻页.
>用onPageSelected来监听页的变化.

	例：initialPage={1}

<span style='color:#188eee;font-size:15;font-weight:bold'>keyboardDismissMode</span>
>拖拽视图会让键盘消失。
>默认none:不会消失;
>on-drag:拖拽开始时让键盘消失

	例：keyboardDismissMode='on-drag'

<span style='color:#188eee;font-size:15;font-weight:bold'>onPageScroll</span>
>当在页间切换时执行。
>回调参数event.nativeEvent对象包含数据:

- event.nativeEvent.position:从左数起第一个当前可见的页面的下标。
- event.nativeEvent.offset:当前页面切换的状态
- 值x表示现在"position"所表示的页有(1 - x)的部分可见，而下一页有x的部分可见。	

> 

	例：onPageScroll={(e)=>console.log(e)}

<span style='color:#188eee;font-size:15;font-weight:bold'>onPageScrollStateChanged</span>
>页面滑动状态变化时调用此回调函数,状态有:


- idle:空闲,当前没有交互。
- dragging:拖动中,当前页面正在被拖动
- settling:处理中,当前页面发生过交互,正在结束开头或收尾的动画。

> 
    
	例：onPageScrollStateChanged={(state)=>console.log(state)}

<span style='color:#188eee;font-size:15;font-weight:bold'>onPageSelected</span>
>页面切换完成后调用.

  - event.nativeEvent.position 当前被选中的页面下标

> 

    例：onPageSelected={(e)=>console.log(e.nativeEvent.position)} 

<span style='color:#188eee;font-size:15;font-weight:bold'>scrollEnabled</span>
>设为false时可禁止滚动.默认值为true.

    例：scrollEnabled={true} 
<span style='color:#188eee;font-size:15;font-weight:bold'>pageMargin</span>
>每个Page中间相隔距离.

    例：pageMargin={50}

<span style='color:#188eee;font-size:15;font-weight:bold'>setPage</span>
>滑动有动画效果.

    例：this.viewPager.setPage(defaultPage);
<span style='color:#188eee;font-size:15;font-weight:bold'>setPageWithoutAnimation</span>
>滑动没有动画效果.

    例：this.viewPager.setPageWithoutAnimation(defaultPage);

####关键代码(全部代码已上传到Github)


欢迎你的加入！

<font style='color:green;font-weight:bold'>公众号：Domeday</font>

推送时间为：
>AM 7：00 ~ AM 8：30 
>PM 9：30 ~ PM 11：00

<span style='color:#B2B2B2'>在互联网这个行业，技术的更新迭代速度很快，唯有不断学习和尝试，我们才能立于不败之地，人都是做自己原本不能胜任的事情中，才能快速成长。所以，不要让任何事情成为你不去学习的理由！，你学过的每一样东西，都会在你一生的某个时候派上用场的。
</span>


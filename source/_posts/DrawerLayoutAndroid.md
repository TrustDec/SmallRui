---
title:DrawerLayoutAndroid
---

>仅限Android平台的抽屉式组件.
>子视图会成为主视图.
>可在指定的窗口侧面拖拽出来.

<!-- More -->

<font style='color:green;'>本章节代码已上传Github</font>
>URL:https://github.com/TrustTheBoy/React-Native-BBS
>找不到的请聊天区留言.
>效果图在文章最后.
>时间太紧,页面没怎样美化.

效果图：
<img src="http://reactnative.cn/static/docs/0.35/img/components/drawerlayoutandroid.png" style="width:20%;height:35%;"/>

官网提供的属性：
<span style='color:#188eee;font-size:15;font-weight:bold'>drawerBackgroundColor</span>
>指定抽屉的背景颜色。
>默认值为白色。
>若要设置抽屉的不透明度,请使用rgba。

	例：drawerBackgroundColor="rgba(188,0,202,0.5)"
<span style='color:#188eee;font-size:15;font-weight:bold'>drawerWidth</span>
>指定抽屉从屏幕边缘拖进的视图宽度。

	例：drawerWidth={230}
<span style='color:#188eee;font-size:15;font-weight:bold'>drawerPosition</span>
>指定抽屉可以从屏幕的哪一边滑入,可选参数：
>DrawerLayoutAndroid.positions.Left
>DrawerLayoutAndroid.positions.Right

	例：drawerPosition={DrawerLayoutAndroid.positions.Left}
<span style='color:#188eee;font-size:15;font-weight:bold'>drawerLockMode</span>
>设置抽屉的锁定模式,三种模式
  - unlocked：不锁定，可以响应打开和关闭操作，默认值;
  - locked-losed：抽屉保持关闭，不能用手势打开
  - locked-open：抽屉保持打开，不能用手势关闭;

    例：drawerLockMode='locked-open'

<span style='color:#188eee;font-size:15;font-weight:bold'>keyboardDismissMode</span>
>设置拖动过程中是否隐藏软键盘,可选值有
  - none：不隐藏,默认值
  - on-drag：拖动时隐藏 

    例：keyboardDismissMode="on-drag"

<span style='color:#188eee;font-size:15;font-weight:bold'>statusBarBackgroundColor</span>
>使抽屉占满整个屏幕，并设置状态栏颜色(支持API21+/安卓系统5.0以上)

	例：statusBarBackgroundColor='red'

<span style='color:#188eee;font-size:15;font-weight:bold'>onDrawerStateChanged</span>
>抽屉的状态变化时调用此回调函数
  - Idle：空闲状态，即没有发生任何交互；
  - Dragging：正在拖动状态，用户正在进行交互；
  - Settling：依靠中状态，用户刚刚结束交互;

    例：onDrawerStateChanged={(state)=>this._DrawerStateChanged(state)}
<span style='color:#188eee;font-size:15;font-weight:bold'>onDrawerOpen</span>
>每当导航视图（抽屉）被打开之后调用此回调函数

	例：onDrawerOpen={()=>{console.log('打开了')}}
<span style='color:#188eee;font-size:15;font-weight:bold'>onDrawerClose</span>
>每当导航视图（抽屉）被关闭之后调用此回调函数

	例：onDrawerClose={()=>{console.log('关闭了')}}
<span style='color:#188eee;font-size:15;font-weight:bold'>onDrawerSlide</span>
>每当导航视图（抽屉）产生交互的时候调用此回调函数

	例：onDrawerSlide={()=>console.log("正在交互......")}
<span style='color:#188eee;font-size:15;font-weight:bold'>renderNavigationView</span>
>用于渲染一个可以从屏幕一边拖入的导航视图

	例：renderNavigationView={() => navigationView}

```
let navigationView = (
  <View style={{flex:1}}>			
    <Text style={{margin:10,fontSize:24,color:'#188eee',}}>
      我是侧边栏/抽屉
    </Text>
	<TextInput style={{width:300,height:40,backgroundColor:'#fff'}}/>
	<TouchableHighlight
	  onPress={()=>this.closeDrawer()}
	  style={{padding:10,backgroundColor:'#e6e6e6',marginTop:20}}
	>
	  <Text style={{textAlign:'center',}}>
		closeDrawer:关闭抽屉
	  </Text>
	</TouchableHighlight>
 </View>
);
```
<font style='color:red;'>navigationView视图中如果设置了backgroundColor,会覆盖drawerBackgroundColor，不少人在这里遇坑，以为drawerBackgroundColor不起作用</font>

DrawerLayoutAndroid 属性 code

```
<DrawerLayoutAndroid
	ref={'drawerLayout'}
	drawerBackgroundColor="rgba(188,0,202,0.5)"
	drawerWidth={230} 	
	drawerPosition={DrawerLayoutAndroid.positions.Left} 	     
	renderNavigationView={() => navigationView} 
	onDrawerOpen={()=>{console.log('打开了')}} 
	onDrawerClose={()=>{console.log('关闭了')}}
	onDrawerSlide={()=>console.log("正在交互......")}
	onDrawerStateChanged={(state)=>this._DrawerStateChanged(state)} 
	drawerLockMode='locked-open' 		
	keyboardDismissMode="on-drag"	
	statusBarBackgroundColor='red'
>

```
此组件还提供openDrawer 与 closeDrawer

>openDrawer:打开抽屉
>closeDrawer：关闭抽屉

<font style='color:green'>无论抽屉处于那种状态，
都仍然可以调用openDrawer/closeDrawer这两个方法打开和关闭</font>

实例：部分代码

```
openDrawer(){		
	this.refs.drawerLayout.openDrawer()
}
closeDrawer(){
	this.refs.drawerLayout.closeDrawer()
}
.....(省略其他)...
<TouchableHighlight
  onPress={()=>this.openDrawer()}
  style={{padding:10,backgroundColor:'#e6e6e6'}}
>
  <Text style={{textAlign:'center',}}>
    openDrawer:打开抽屉
  </Text>
</TouchableHighlight>
<TouchableHighlight
  onPress={()=>this.closeDrawer()}
  style={{padding:10,backgroundColor:'#e6e6e6'}}
>
  <Text style={{textAlign:'center',}}>
    closeDrawer:关闭抽屉
  </Text>
</TouchableHighlight>
```

最后的效果:



欢迎你的加入！

<font style='color:green;font-weight:bold'>公众号：Domeday</font>

推送时间为：
>AM 7：00 ~ AM 8：30 
>PM 9：30 ~ PM 11：00

<span style='color:#B2B2B2'>在互联网这个行业，技术的更新迭代速度很快，唯有不断学习和尝试，我们才能立于不败之地，人都是做自己原本不能胜任的事情中，才能快速成长。所以，不要让任何事情成为你不去学习的理由！，你学过的每一样东西，都会在你一生的某个时候派上用场的。
</span>
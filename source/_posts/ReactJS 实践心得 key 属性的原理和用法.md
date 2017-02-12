---
title: ReactJS 实践心得 key 属性的原理和用法
---
ReactJS && ReactNative ：
>本章主要讲解key属性的原理和用法

<!-- More -->

首先你需要知道：

<span style="color:#188eee;font-weight:bold">React与ReactJS && React Native</span>

>**React**是由**ReactJS**与**React Native**组成

>其中**ReactJS**是Facebook开源的一个前端框架

>**React Native**是**ReactJS**思想在*native*上的体现！

<span style="color:red">既然学过React Native，那你么reactJS呢？</span>

>这已经是一个非常流行的框架，其实作为react-native入门，了解一些就够用了。 

<span style="color:red">那么JSX呢？</span>

**JSX并不是一门新的语言，仅仅是个语法糖**，允许开发者在JavaScript中书写HTML语法。
最后**每个HTML标签都转化为JavaScript代码来运行**。

<span style="color:#188eee;font-weight:bold">Start 今天的正题</span>

我们知道，React 元素可以具有一个特殊的属性 key，这个属性不是给用户自己用的，而是给 React 自己用的。

如果我们动态地创建 React 元素，而且 React 元素内包含数量或顺序不确定的子元素时，我们就需要提供 key 这个特殊的属性。

如果你有下面这样的代码：
```javascript
const UserList = props => (
 <div>
  <h3>用户列表</h3>
  {props.users.map(
	u => <div>{u.id}:{u.name}</div>)
  }  
	// 没有提供 key
 </div>
)
```
React 会在控制台打印出报警信息：
<span style="color:red">
```
Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of `App`. See https://fb.me/react-warning-keys for more information.
```
</span>
你必须为数组中的元素提供唯一的 key 属性，就像下面这样：
```javascript
const UserList = props => (
	  <div>
	    <h3>用户列表</h3>
	   {props.users.map(u => <div key={u.id}>{u.id}:{u.name}</div>)}  
		// 提供了 key
	  </div>
)
```
为什么呢？

我们知道当组件的属性发生了变化，其 render 方法会被重新调用，组件会被重新渲染.
比如: 

UserList组件的users属性改变了，就得重新渲染UserList组件，
包括外部的`<div>`（容器），内部的一个`<h3>`和若干个`<div>`（每一个描述一个用户）。

对后一种 `<div>`（表示用户的），由于其处在一个长度不确定的数组中，
React 需要判断，对数组中的每一项，到底是新建一个元素加入到页面中，还是更新原来的元素。

比如以下几种情况：

	[{name: '张三', age: 20}] => [{name: '张三', age: 21}]
>这种情况明显只需要更新元素，没有必要重新创建元素。

因为人还是那个人，除了 age，其他信息没有变，显示用户姓名的那个（更小的）元素，是不需要更新（被 ReactDOM 操作到）的。

	[{name: '张三'}] => [{name: '张三'}, {name: '李四'}] 
>这种情况，显然需要添加一个新元素来表示李四，这个新元素对应的 DOM 元素会被插入到页面中。

	[{name: '张三'}] => [{name: '李四'}]
>这种情况就有点复杂了，似乎两种方案都可以。
>可以把表示张三的元素删掉，为李四新建一个，当然是非常合理的选择。
>但是直接把张三的元素换成李四，似乎也无不可。

实际上，如果真的认为上述第3种的后一种方案也无不可，那可是大错特错了。为什么呢：
考虑这种情况：

	[{name: '张三'}, {name: '李四'}] => [{name: '李四'}, {name: '张三'}]

<span style="color:red">难道也需要把张三的元素更新成李四的，李四的元素更新成张三的吗？</span>

那么，为数组中的元素传一个唯一的 key（比如用户的 ID），就很好地解决了这个问题。

React 比较更新前后的元素 key 值，如果相同则更新，如果不同则销毁之前的，重新创建一个元素。

<span style="color:red">那么，为什么只有数组中的元素需要有唯一的 key，而其他的元素（比如上面的`<h3>`用户列表`</h3>`）则不需要呢？</span>

>答案是：React 有能力辨别出，更新前后元素的对应关系。

这一点，也许直接看 JSX 不够明显，看 Babel 转换后的 React.createElement 则清晰很多：
>转换前:

```javascript
const element = (
  <div>
    <h3>example</h3>
    {[<p key={1}>hello</p>, <p key={2}>world</p>]}
  </div>
)
```
>转换后

```javascript
"use strict";
var element = React.createElement(
  "div",
  null,
  React.createElement("h3",null,"example"),
  [
    React.createElement("p",{ key: 1 },"hello"), 
    React.createElement("p",{ key: 2 },"world")
  ]
)
```
不管 props 如何变化，数组外的每个元素始终出现在 React.createElement() 参数列表中的固定位置，这个位置就是天然的 key。

题外话:

初学 React 时还容易产生另一个困惑;

<span style="color:red">那就是为什么 JSX 不支持 if 表达式来有选择地输出</span>
例如：

	{if(yes){ <div {...props}/>}})

而必须采用三元运算符来完成这项工作
必须这样：

	{yes ? <div {...props}/>} : null)

那是因为，React 需要一个 null 去占住那个元素本来的位置。

吐个槽：

曾经，我天真的以为 key 这个元素只应在数组中使用,直到我在一个复杂的项目中写出了及其恶心的 **componentWillReceiveProps**方法。我尝试寻找销毁和重建组件，触发*componentDidMount** 方法,重置 state，然后才突然发现 key 这个属性已经在那里了。

举个例子:

我们有一个展示用户信息的**UserDashboard**组件。
传给组件的**props**只有用户的 基本信息（ID，姓名等）,而有关用户的详细信息（比如当前是否在线等）是需要请求过来的。
组件内的一些操作（比如尝试与该用户聊天）也会使用请求,组件本身也有各种状态（比如是否显示聊天框）。

整个界面上最多只有一个**UserDashboard**,但某些操作（比如点击旁边的 UserList）可能会切换 UserDashboard 的目标用户。

<span style="color:red">那么问题就来了：</span>

学挖掘机技术哪家强。。。。。咳咳咳串错场子了

当目标用户切换的时候，UserDashboard 仅仅是一个普通的更新操作，触发的是 **componentWillReceiveProps**,**shouldComponentUpdate**,**componentWillUpdate**，**componentDidUpdate** 这一套方法。

我们需要在 **componentWillReceiveProps** 中做太多的事情：

- 检测这次 props 的更新是否改变了用户的 ID，如果是的话，我们需要检查 UserDashboard 发出去的请求是否都得到了响应，对还未收到响应的请求注销其响应函数（否则上一个用户的在线状态有可能显示在这一个用户上）；

- 我们还要更新 UserDashboard 上的几乎所有状态（切换用户的时候总要把聊天框关闭吧）；

如果我们还不幸地用的 ref 做了一些神奇的 hack，那么你还要去手动把之前做的事情复原回来，这简直要成一团乱麻了！
当 UserDashborad 的逻辑，你的componentWillReceiveProps方法里会充斥着晦涩难懂的只有你能看懂的代码（两周后你自己也看不懂了）。

<span style="color:red">解决方案是什么？</span>

就是用 key 属性。在 JSX 中使用UserDashboard的时候，不仅把userInfo传入，把 userInfo.id 作为名为 key 的 props 传入（尽管 UserDashboard 不是数组中的组件）。

这样切换目标用户的时候，key 属性也变了，React 会自动销毁之前的组件，用一个全新的组件来渲染新的用户：

我们可以从容地在**componentWillUnmount**里作清理工作（注销请求的响应函数，防止其更新一个 unmounted component），至于重置 state 这些工作已经不需要做了，由于组件不再是更新，而是销毁和重建，已经是天然完成的。

当然，你可以质疑这样做是否会影响性能。
我认为，只要目标用户的切换不够频繁，对性能的影响是很小的。
如果不使用 key 触发组件的销毁和重建，任由组件自行[更新],每次切换时更新的内容也是很多的。

这时，我们使用 key 带来的性能损耗是完全可以接受的，而带来的收益却非常大。

所以，我想说的结论是：为了组件内部逻辑的清晰，你几乎应该在任何复杂的有状态组件（尤其是有具体对应对象的）上使用key属性（只要 key 属性的改变不是很频繁），这样做，才能在合适的时候触发组件的销毁与重建，组件才能有一个健康的**生命周期**。

又是一个题外话

配合 react-router 时，通常要为 route 组件赋 key，但通常情况下我们是没法传 props 给 route 组件的。

解决的方案是 createElement 方法，如下所示。

```javascript
class App extends Component {
 static createElement = (Component, ownProps) => {
   const {userId} = ownProps.params;
    switch (Component) {
     case UserDashboard:
      return <Component key={userId} {...ownProps}/>;
      default:
       return <Component {...ownProps}/>;
    }
  };
  render() {
	return (
	 <Provider store={store}>
	  <Router createElement={App.createElement}
		history={syncHistoryWithStore(hashHistory, store)}>
		 <Route path="/" component={Home}>
		  <IndexRoute component={Index}/>
			<Route path="users/:userId" 
				component={UserDashboard}/>
			</Route>
		</Router>
	</Provider>
	)
  }
}
```
欢迎你的加入！
<font style='color:green;font-weight:bold'>公众号：Domeday</font>
推送时间为：
>AM 7：00 ~ AM 8：30 
>PM 9：30 ~ PM 11：00

<span style='color:#B2B2B2'>在互联网这个行业，技术的更新迭代速度很快，唯有不断学习和尝试，我们才能立于不败之地，人都是做自己原本不能胜任的事情中，才能快速成长。所以，不要让任何事情成为你不去学习的理由！，你学过的每一样东西，都会在你一生的某个时候派上用场的。
</span>


```javascript
	GitHub=> React Native BBS 组件已更新
```
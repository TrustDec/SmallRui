---
title: LayoutAnimation
---
><span class='fontSmall' style='color:green'>修改于2016年10月29日
>RN版本:0.36</span>  

<!-- More -->

Demo将上传至Github
><span class='fontSmall'>布局发生变化时,自动将视图运动到它们的新位置.
>调用LayoutAnimation.configureNext.
>然后调用setState.</span>

一些简单的界面切换、更新的时候可以用RN提供的LayoutAnimation来实现。

打开LayoutAnimation的文档,可以看到提供的属性并不多，用起来也并不难。

RN提供的方法：
configureNext:安排一个动画在下一个布局中发生,参数如下：
><span style='color:green;font-weight:bold'>config：</span>指定动画属性参数有:
> - duration:动画持续的时间
> - create:创建一个新视图所使用的动画
> - update:当视图被更新的时候所使用的动画
> - delete:删除一个视图使用的动画

><span style='color:green;font-weight:bold'>onAnimationDidEnd：</span>当动画完成的时候调用的方法 <span style='color:red'>(iOS)</span>
><span style='color:green;font-weight:bold'>onError:</span>当动画发生错误的时候调用的方法 <span style='color:red'>(iOS)</span>

create:创建configureNext所需要的config参数

RN提供的属性：
Types:动画类型,主要有:
> - spring:弹跳
> - linear:线性
> - easeInEaseOut:缓入缓出
> - easeIn:缓入
> - easeOut:缓出
> - keyboard:键入

Properties:类型定义在LayoutAnimation.Properties中
> - opacity：透明度
> - scaleXY：缩放

configChecker:配置检查器

Presets:对象类型(动画效果默认配置项)

easeInEaseOut:默认配置项，在LayoutAnimation.Presets中

linear:默认配置项，在LayoutAnimation.Presets中

spring:默认配置项，在LayoutAnimation.Presets中

<span style='color:188eee;font-weight:bold'>注：安卓平台使用 LayoutAnimation 动画必须加入下面这句，iOS设备默认打开。具体代码如下:</span>
```javascript
if (Platform.OS === 'android') {
  UIManager.setLayoutAnimationEnabledExperimental(true)
}
```

当布局更新时,使用LayoutAnimation进行设置一下动画配置:

```javascript
componentWillUpdate() {   
  // LayoutAnimation.easeInEaseOut();
  //或者可以使用如下的自定义的动画效果
  LayoutAnimation.configureNext(CustomLayoutAnimation);
}
```
自定义动画：
```javascript
var CustomLayoutAnimation = {
    duration: 800,
    create: {
      type: LayoutAnimation.Types.spring,
      property: LayoutAnimation.Properties.scaleXY,
    },
    update: {
      type: LayoutAnimation.Types.spring,
      property: LayoutAnimation.Properties.scaleXY,
    },
    
  };

```
以上是部分,需要全部代码请去：
	https://github.com/TrustTheBoy/React-Native-BBS

建议查看源代码进行学习：
	https://github.com/facebook/react-native/blob/d4e7c8a0550891208284bd1d900bd9721d899f8f/Libraries/LayoutAnimation/LayoutAnimation.js

<font style='color:green;font-weight:bold'>公众号：Domeday</font>

推送时间为：
>AM 7：00 ~ AM 8：30 
>PM 9：30 ~ PM 11：00

<span style='color:B2B2B2'>枕头里藏满了发霉的梦，梦里住满了得不到的人</span>
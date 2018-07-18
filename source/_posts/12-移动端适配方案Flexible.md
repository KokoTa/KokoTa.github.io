---
title: 12-移动端适配方案Flexible
date: 2018-07-03 10:29:53
tags:
  - 前端工程
---
<img src="/images/index/12.jpg" />
<!--more-->

# 吐槽
我：那个...在PC端要写一个样式，在移动端也要写一个样式的专有名词是什么？  
他：响应式布局~  
我：那个...移动端布局是用rem吗？  
他：是的哟，因为rem是根据根元素来计算得到px的，移动端有Flexible适配方案哟。  
我：那是啥玩意儿？  
他：哎~那我还是科普一下好了= =。

# 前置知识
自适应布局，顾名思义，“在不同的设备下显示的效果都是一样的”，因此 px 这种单位是不可用的。  
因为书写 CSS 用 px，指定的是 CSS像素，而这个像素，在不同设备下显示的大小都是一样的，比如 100px 的宽度：  
在 `1000*1000` 的分辨率下显示了 `100` 的长度，在 `500*500` 还是显示了 `100` 的长度，  
而我们的目的是希望在 `500*500` 的情况下显示 `50` 的长度，即同比例缩放。  

Flexible适配方案实现了这种效果。  
让我们来谈谈这个方案的原理吧。  
它的核心思想就是设置自定义的 **data-dpr** 和根元素的 **font-size**，然后搭配 **页面缩放** 和 **rem** 来实现自适应布局。  
那么问题来了，为什么要设置这两个属性呢？  

## CSS像素 和 设备像素

首先我们需要知道一个知识点，那就是 **CSS像素** 和 **设备像素**。  
前者顾名思义，就是我们平常写代码时的像素，比如：`12px`。  
后者指代的是设备屏幕的像素，一般来说，安卓机 `1设备像素 = 1CSS像素`。  
但是苹果有Retina屏幕，它的设备像素高于普通的屏幕，即 `2设备像素 = 1CSS像素`。  
我们把 `设备像素/CSS像素` 的值称为 **dpr**。  
综上可得：同样的CSS像素，同样的显示，不同的设备像素。  

了解了上面这个知识有什么用？  
很明显，我们需要根据不同的 dpr 来作对应的适配处理。  

比如图片，假设我们用的 200x300 分辨率的图，显示大小也都是 200x300，  
在 dpr = 1 时显示正常，但是在 dpr = 2 时，图片会比在 dpr = 1 时模糊。  
原因是虽然显示时都是 200x300 的 **CSS像素**，但在高分辨率下，1px 的 **物理像素** 被强制撕扯到 2px。  
也就是说在 dpr = 1 的情况下，图片 200x300 分辨率没问题。  
在 dpr = 2 的情况下，图片需要 400x600 的分辨率才能显示得和前者一样。  
也就是说对于Retina屏幕，200x300 的图片需要无损放大到 400x600，才能保证图片不模糊。  

## Viewport

接下来我们需要了解 `viewport` 标签：  
而在此之前，又得科普一下 **layoutView** 和 **visualView**。  
前者是页面的布局大小，也就是网页的占用大小。  
后者是手机屏幕上你想显示的页面的大小。  
简单来说就是，前者是一份报纸，后者是一个可以调节大小的放大镜。  
默认情况下，layoutView === visualView，也就是默认情况下，整张页面都呈现在了手机中。  
这就导致了如果页面布局很宽很大，那么一坨东西都将塞在这个小小的屏幕中。  
所以一般来说，我们会设置如下的 viewport 标签来避免这种情况。  
```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
```
[关于 viewport 的其他内容](http://www.w3cplus.com/css/viewports.html)

## Rem

**rem** 单位是以根元素(html)的字号大小为基准进行计算的。  
即假设根元素字号为 16px，那么 1rem === 16px，这个特性对我们很有用。  

# Flexible方案
我们创建 html 模板并引入 flexible 库：
```html
<!DOCTYPE html>
<html lang="en"> 
  <head> 
    <meta charset="utf-8"> 
    <meta content="yes" name="apple-mobile-web-app-capable"> 
    <meta content="yes" name="apple-touch-fullscreen"> 
    <meta content="telephone=no,email=no" name="format-detection"> 
    <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script> 
    <link rel="apple-touch-icon" href="favicon.png"> 
    <link rel="Shortcut Icon" href="favicon.png" type="image/x-icon"> 
    <title>再来一波</title> 
  </head> 
  <body> 
    <!-- 页面结构写在这里 --> 
  </body> 
</html>著作权归作者所有。
```
这个库的功能是：
1. 动态改写`<meta>`标签
2. 给`<html>`元素添加`data-dpr`属性，并且动态改写`data-dpr`的值
3. 给`<html>`元素添加`font-size`属性，并且动态改写`font-size`的值

我们要知道该方案的默认设计稿宽度是 750px，flexible 将视觉稿分成了100份，单位为 a，同时规定 10a === 1rem，即：
```js
1a = 7.5px
1rem = 75px
```
那么我们这个示例的稿子就分成了 10a，也就是整个宽度为 10rem，html元素对应的 **font-size** 为75px(rem基准值)：  
这样一来，对于视觉稿上的元素尺寸换算，只需要 `原始的px值/rem基准值` 即可。  
例如，尺寸是 `176px * 176px`，转换结果为 `2.346667rem * 2.346667rem`。  

当然，这是在 dpr = 1 的情况下。  
当 dpr = 2 时，如果不改变 **meta** 标签，则设计稿实际呈现的是 750 * 2 的视觉效果。  
这很明显不是我们想要的，所以该库对视图进行了缩放：  
```html
<!-- dpr = 1--> 
<meta name="viewport" content="initial-scale=scale,maximum-scale=scale,minimum-scale=scale,user-scalable=no"> 
<!-- dpr = 2--> 
<meta name="viewport" content="initial-scale=0.5,maximum-scale=0.5,minimum-scale=0.5,user-scalable=no"> 
<!-- dpr = 3--> 
<meta name="viewport" content="initial-scale=0.3333333333,maximum-scale=0.3333333333,minimum-scale=0.3333333333,user-scalable=no">
```
感觉有点像高分辨率下图片的处理。  

OK，现在就是根据设计稿来写样式了。  
但问题来了，难道我每次都要手动 `原始的px值/rem基准值` 计算得到 rem？  
当然不是，我们可以通过安装 [CSSREM](https://github.com/flashlizi/cssrem) 插件来使 px -> rem。  
或者利用 SCSS 的混合器或 postcss 来进行换算。  

最后，对于字体来说，我们希望它不进行自动适配。  
但由于该方案进行了视图缩放，使 dpr = 1 时，字号16px；dpr = 2 时，字号8px。  
所以这里 **data-dpr** 的作用就体现出来了，我们写CSS时，通过判断这个属性值来决定是否放大字体。  
dpr = 1 时，字号16px；dpr = 2 时，字号16px * 2。  
这样就能保证不同 dpr 下，字体显示的大小保持一致。  

我们可以使用 SCSS 混合器来进行换算：  
```css
@mixin font-dpr($font-size){ 
  font-size: $font-size; 
  [data-dpr="2"] & { 
    font-size: $font-size * 2; 
  } 
  [data-dpr="3"] & { 
    font-size: $font-size * 3; 
  } 
}
```

OK，结束，完结撒花^ ^  

# 参考
[手淘移动端适配方案](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)  
[移动端上的设计和适配](http://www.w3cplus.com/mobile/mobile-design-and-adapter.html)  
[移动端高清、多屏适配方案](https://div.io/topic/1092)
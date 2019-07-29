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
我：那个...响应式布局和移动端适配有虾米区别？  
他：前者是用来适配PC端和移动端的，后者是用来适配不同移动端的，是两个概念。  
我：？？？  
他：哎~响应式布局自己百度，这里我给你讲讲移动端适配。  

# 前置知识

移动端适配，顾名思义，“在各种的设备下显示的效果都是一样的”，因此 px 这种单位是不可用的。  
因为书写 CSS 用 px，指定的是 CSS像素，而这个像素，在不同设备下显示的大小都是一样的，比如 100px 的宽度：  
在 `1000*1000` 的分辨率下显示了 `100` 的长度，在 `500*500` 还是显示了 `100` 的长度，  
而我们的目的是希望在 `500*500` 的情况下显示 `50` 的长度，即同比例缩放。  

<!-- Flexible适配方案实现了这种效果。  
让我们来谈谈这个方案的原理吧。  
它的核心思想就是设置自定义的 **data-dpr** 和根元素的 **font-size**，然后搭配 **页面缩放** 和 **rem** 来实现自适应布局。  
那么问题来了，为什么要设置这两个属性呢？   -->

## CSS像素 和 设备像素

首先我们需要知道一个知识点，那就是 **CSS像素** 和 **设备像素**。  
前者顾名思义，就是我们平常写代码时的像素，比如：`1px`。  
后者指代的是设备屏幕的像素，一般来说，安卓机 `1CSS像素 = 1设备像素`。  
但是苹果有Retina屏幕，它的设备像素高于普通的屏幕，即 `1CSS像素 = 2设备像素`。  
我们把 `设备像素/CSS像素` 的值称为 **dpr**。  
综上可得：同样的CSS像素，同样的显示，不同的设备像素。  

了解了上面这个知识有什么用？  
很明显，我们需要根据不同的 dpr 来作对应的适配处理。  

比如图片，假设我们用的 200x300 分辨率的图，显示大小也都是 200x300，  
在 dpr = 1 时显示正常，但是在 dpr = 2 时，图片会比在 dpr = 1 时模糊。  
原因是虽然显示时都是 200x300 的 **CSS像素**，但在高分辨率下，1px 的 **物理像素** 被强制撕扯到 2px。  
也就是说在 dpr = 1 的情况下，图片 200x300 分辨率没问题。  
在 dpr = 2 的情况下，图片需要 400x600 的分辨率才能显示得和前者一样。  
也就是说对于Retina屏幕，200x300 的图片需要无损放大到 400x600 的分辨率后再以 200x300 的大小显示，才能保证图片不模糊。  
在实际过程中，我们可以使用 `window.devicePixelRatio` 或者 `@media only screen and (min-device-pixel-ratio: 2){}` 来根据不同的 dpr 应用不同分辨率的图片。  

比如 1px 问题，假设我们使用了 1px 的边框，  
在 dpr = 1 的情况下，显示正常，因为 `1CSS像素 = 1设备像素`，  
但在 dpr = 2 的情况下，由于 `1CSS像素 = 2设备像素`，导致这个 1 个 CSS 像素要被撕扯成 2个设备像素，也就是说实际上这个边框要占据 2个设备像素 的宽度，  
所以在Retina屏幕下，感觉边框变粗了，也变模糊了。  
解决的方式[很多](https://www.w3cplus.com/css/fix-1px-for-retina.html)，  
但使用 Flexible 则不用花心思到这个问题上(dpr = 2 的情况下，由于缩放 0.5 倍，原本 1css 占用 2设备像素，变为了 1css 占用 1设备像素)。  

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
设置后我们的 visualView 就被设置为设备的宽度了，比如 375px。  
此时如果 layoutView > 375，那么就会出现内容溢出，会出现滚动条。  
[关于 viewport 的其他内容](http://www.w3cplus.com/css/viewports.html)

## Rem

**rem** 单位是以根元素(html)的字号大小为基准进行计算的。  
即假设根元素字号为 16px，那么 1rem === 16px，这个特性对我们很有用。  

# Flexible方案

## 原理

```html
  <meta charset="utf-8"> 
  <meta content="yes" name="apple-mobile-web-app-capable"> 
  <meta content="yes" name="apple-touch-fullscreen"> 
  <meta content="telephone=no,email=no" name="format-detection"> 
  <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script> 
```

这个库的功能是：  

1. 动态改写`<meta>`标签，通过判断 dpr 来对 viewport 进行缩放：
    ```html
    <!-- dpr = 1 -->
    <meta name="viewport" content="initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no"> 
    <!-- dpr = 2 -->
    <meta name="viewport" content="initial-scale=0.5,maximum-scale=0.5,minimum-scale=0.5,user-scalable=no"> 
    <!-- dpr = 3 -->
    <meta name="viewport" content="initial-scale=0.3333333333,maximum-scale=0.3333333333,minimum-scale=0.3333333333,user-scalable=no">
    ```
2. 给`<html>`元素添加`data-dpr`属性，该值为 `dpr` 的值。

3. 给`<html>`元素添加`font-size`属性，该值为 `设备分辨率宽度 / 10` 的值(注意这里是设备分辨率宽度，而非屏幕宽度)。

这里，第 2 点和第 3 点的作用是什么呢？

假设设计稿宽度为 750px，我们将其分为 100份，单位为 a，同时规定 10a === 1rem，即：

```js
1a = 7.5px
1rem = 75px
```

使得 html 元素的 font-size 对应 `font-size = 1rem = 75px`，  
这样一来，对于视觉稿上的元素尺寸，只需要遵循 `rem值 * rem基准值 = 原始的px值` 的公式即可。  
例如，750px 设计稿下的尺寸 `150px * 150px` 将被转换为 `2rem * 2rem`。  

当然，上面的情况是假设 `设备分辨率宽度 = 750`，即设计稿是 750px 的情况，然而不同移动端设备分辨率宽度也是不同的。  
举个栗子，iphone6（dpr=2, 414x763）的设备下，1rem 是多少值呢？  
通过公式可以知道，`414 x 2 / 10 = 82.8`，即 `font-size = 1rem = 82.8px`。  

通过 `rem值 * rem基准值 = 原始的px值` 这个公式，我们可以知道 rem值 是固定的，rem基准值 和 原始的px值 成正相关，即基准值越大，原始px值 越大。当然，rem基准值 的计算是通过 设备分辨率宽度 得到的，不同设备其 rem基准值 不同，也就导致 原始px值 不同，这也是自适应布局的本质原理。

## 使用

OK，现在就是根据设计稿来写样式了。  
但问题来了，难道我每次都要手动 `原始的px值/rem基准值` 计算得到 rem？  
当然不是，我们可以通过安装 [CSSREM](https://github.com/flashlizi/cssrem) 插件来使 px -> rem（通常我们的设计稿是 750px，那么其 rem基准值 应设为 75px）。  
或者利用 SCSS 的混合器或 postcss 来进行换算。  

最后，对于字体来说，我们希望它不进行自动适配。  
但由于该方案进行了视图缩放，使 dpr = 1 时，字号16px；dpr = 2 时，字号8px(视图缩放了 0.5 倍)。  
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

# 参考

[移动端布局概念](https://juejin.im/post/5b39905351882574c72f2808#heading-21)
[手淘移动端适配方案](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)  
[移动端上的设计和适配](http://www.w3cplus.com/mobile/mobile-design-and-adapter.html)  
[移动端高清、多屏适配方案](https://div.io/topic/1092)
[前端移动端适配总结](https://segmentfault.com/a/1190000011586301)
[再聊移动端页面的适配 - vw/vh 方案](https://www.w3cplus.com/css/vw-for-layout.html)
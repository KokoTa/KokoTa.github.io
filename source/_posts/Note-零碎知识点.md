---
title: Note-零碎知识点
date: 2017-10-09 02:19:27
tags:
	- 笔记
---
<img src="/images/index/X1.jpg" />
<!--more-->

# HTML
* viewport原理
[Link](http://www.cnblogs.com/pigtail/archive/2013/03/15/2961631.html)

# CSS
* position的几个属性有什么区别和使用情况  
[Link](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)

* CSS中伪类选择器选择列表最后一个元素  
[Link](http://www.w3school.com.cn/cssref/css_selectors.asp)

* 应不应该使用inline-block代替float
[Link](http://www.w3cplus.com/css/inline-blocks.html)

* CSS选择器优先级
[Link](http://blog.csdn.net/lzgs_4/article/details/43446303)

* transition与animation的区别  
[link](http://blog.csdn.net/jdk137/article/details/50474129)

# JS
* 写一个对象来满足`A == '1'`  
``` javascript
var A = {
	toString () {
		return '1';
	},
	valueOf () {
		return '1';
	}
}
```

* ==和===的区别：  
==：等于，会进行隐式转换，不比较值的类型
===：严格等于，不进行隐式转换，比较值的类型

* `(010)111111 -> 010-111111`  
```javascript
'(010)111111'.replace(/[\(\)]/g, "-").slice(1);
```

* this指向  
	* 由new 调用？绑定到新创建的对象。
	* 由call 或者apply（或者bind）调用？绑定到指定的对象。
	* 由上下文对象调用？绑定到那个上下文对象。
	* 默认：在严格模式下绑定到undefined，否则绑定到全局对象。

* bind的polyfill  
[link](https://github.com/KokoTa/All-demo/blob/master/other/bind.js)

* Ajax的Promise封装  
[Link](http://javascript.ruanyifeng.com/advanced/promise.html)

* Ajax的readyState五种状态  
[Link](http://blog.163.com/freestyle_le/blog/static/183279448201269112527311/)

* 如何解决回调地狱  
[Link](http://javascript.ruanyifeng.com/advanced/promise.html)

* 数组深拷贝  
[Link](http://www.cnblogs.com/matthew-2013/p/3524297.html)

* 如何使用ES6的generator函数来进行异步的调用  
[Link](http://es6.ruanyifeng.com/#docs/generator-async#Generator-函数)

* ES6中箭头运算符this指向问题  
[Link](http://es6.ruanyifeng.com/?search=this&x=0&y=0#docs/function#箭头函数)

* n个元素的数组，求出一个数字最少的组合，使得这个组合的和为m，使用动态规划  
[Link-如何理解动态规划](https://www.zhihu.com/question/39948290)  
具体答案母鸡，可参考leetcode 40  

* dropdown效果
[Link](http://www.imooc.com/learn/12)

* debounce  
[Link-简单代码](http://javascript.ruanyifeng.com/advanced/timer.html)  
[Link-详细了解](http://www.css88.com/archives/7010)
[Link-整合代码](https://github.com/KokoTa/All-demo/blob/master/other/debounce.js)

* 轮播的实现  
[Link](http://www.imooc.com/learn/18)

* 图片懒加载和预加载  
[Link-懒加载原理](https://i.jakeyu.top//2016/11/26/%E5%AE%9E%E7%8E%B0%E5%9B%BE%E7%89%87%E6%87%92%E5%8A%A0%E8%BD%BD/)  
[Link-懒加载示例](https://codepen.io/dcorb/pen/eJLMxa)
[Link-预加载实践](http://www.imooc.com/learn/502)

* 事件代理(delegate)  
[Link1](https://zhuanlan.zhihu.com/p/27554181)
[Link2](https://zhuanlan.zhihu.com/p/27653120)

* canvas基本使用(画圆)  
[link](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)  

* 数据类型相关试题(蛋疼无用的考试题)  
[link](http://blog.csdn.net/zr15829039341/article/details/78252897)  

* 数组扁平化(一开始想到的是递归,Emm...)  
[link](http://blog.csdn.net/crystal6918/article/details/77130948)

# HTTP
* URL的组成：  
	* *http://www.abc.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name*  
		1. http -> HTTP协议
		2. www.abc.com -> 域名
		3. 8080 -> 端口号
		4. news -> 虚拟目录
		5. index.asp -> 文件名
		6. ?boardID=5&ID=24618&page=1 -> 查询字符串
		7. #name -> 哈希字符串  
	* *www.m.baidu.com*  
		1. .com -> 顶级/一级域名
		2. baidu.com -> 二级域名
		3. m.baidu.com -> 三级域名
		4. www.m.baidu.com -> 四级域名  

* HTTP了解  
[Link](http://www.ruanyifeng.com/blog/2016/08/http.html)

* HTTP协议中常用的报文头  
[Link](http://www.cnblogs.com/xumengxuan/p/3761314.html)  
[Link](http://blog.csdn.net/a19881029/article/details/14002273)

* Etag，Expires与Cache-control  
[Link](http://www.cnblogs.com/huangzhilong/p/4999207.html)

* HTTPS的原理  
[Link-协议简述](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)  
[Link-运行机制](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)  
[Link-全面了解](https://zhuanlan.zhihu.com/p/22142170)  
参考书籍《图解HTTP》

* 端口号的作用  
[Link-作用](https://baike.baidu.com/item/%E7%AB%AF%E5%8F%A3/103505?fr=aladdin#5)  
[Link-网络基本概念](https://wenku.baidu.com/view/fd755438b9f3f90f77c61b67.html)  
[Link-IP/掩码/网关概念](https://www.zhihu.com/question/20717354)  
参考书籍《图解TCP/IP》

* 一个proxy服务器上有一个很大的域名黑名单，如果快速对于通过proxy的请求进行过滤。    
[Link-代理基础](https://zhuanlan.zhihu.com/p/27424255)  
具体答案母鸡  

* CDN
[link](https://www.zhihu.com/question/37353035)

# Node
* fs.readFile的Promise实现  
```javascript
var fs = require('fs');
var __dirname = "foo";

// promisify fs.readFile()
fs.readFileAsync = function (filename) {
    return new Promise(function (resolve, reject) {
        try {
            fs.readFile(filename, function(err, buffer){
                if (err) reject(err); else resolve(buffer);
            });
        } catch (err) {
            reject(err);
        }
    });
};

// utility function
function getImageAsync(i) {
    return fs.readFileAsync(__dirname + "/image1" + i + ".png");
}
```

# Git
* git的一些命令，git pull和git fetch的区别
区别：pull=fetch+merge，pull的话，下拉远程分支并与本地分支合并。fetch只是下拉远程分支，怎么合并，可以自己再做选择。  
参考书籍《Github入门与实践》  
[Link-Pro Git](https://bingohuang.gitbooks.io/progit2/content/)  

# 其他
* 页面加载时间  
[Link](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigation_timing_API)

* 域名收敛  
[Link](http://www.cnblogs.com/coco1s/p/5365179.html)

* Restful  
[Link-架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)
[Link-API设计](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)

* 开发规范  
[Link-H5](https://www.douban.com/note/557518831/?type=like)
[Link-H5](http://blog.csdn.net/sinat_34719507/article/details/53891959)
[Link-All](https://github.com/JohnsenZhou/Front-End-Checklist)

* Vue/Webpack源码解析(醍醐灌顶的感觉)  
[link](https://github.com/youngwind/blog/issues)
### meta属性及其作用

我们比较熟悉的meta用法，大概就是这样了：

	<meta charset="UTF-8">

但是，很多网站的meta标签都是很长一串。存在即合理，合理的东西需要去研究一下。

>The <meta> tag provides metadata about the HTML document. Metadata will not be displayed on the page, but will be machine parsable.
>Meta elements are typically used to specify page description, keywords, author of the document, last modified, and other metadata.

meta的主要属性有以下这几个：

 - name
 - http-equiv
 - content
 - charset

##### name属性

基本语法：

	<meta name="参数" content="具体值">

1、keywords/description

和SEO有关，告诉搜索引擎网站的关键字和主要内容，下面是新浪博客的meta。如果我们在搜索引擎里输入“名人博客”，查出来靠前的就是新浪博客。当然，搜索引擎的算法没有这么简单，这只是简单的一个例子。

	<meta name="keywords" content="新浪博客,名人博客,博客,新浪,blog">
	<meta name="description" content="新浪网博客频道是全中国最主流，最具人气的博客频道。拥有最耀眼的娱乐明星博客、最知性的名人博客、最动人的情感博客，最自我的草根博客。">

2、viewport(移动端的窗口)

	<meta name="viewport" content="initial-scale=1.0,user-scalable=no,maximum-scale=1,width=device-width">

content属性有：width、height、initial-scale、minimum-scale、maximum-scale、user-scalable

![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/meta/meta1.png)

width：用来控制layout viewport（布局视口）的宽度，一般为了自适应布局，普遍的做法是将width设置为device-width；

height：不常用的属性；

initial-scale：用于指定页面的初始缩放比例。关键是这两个单词不熟悉，如果熟悉这两个单词，这个属性就叫“顾名思义”了；

maximum-scale：指定用户能够放大的最大比例；

minimum-scale：指定用户能够缩小的最大比例。一般这个属性不使用，因为字变得小了，将难以阅读；

user-scalable：控制用户是否可以通过手势对页面进行缩放。该属性的默认值为yes，可被缩放，你也可以将该值设置为no，表示不允许用户缩放网页。

3、其他不常用属性，这里不做介绍。

##### http-equiv属性

1、content-type(指定网页字符集)

	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
	
旧的html，不推荐使用，html5中直接这样使用：

	<meta charset="utf-8">

2、其他类似刷新、缓存的属性，这里也不做介绍，因为我从来没用过，也不想去复制别人的博客。

##### webApp里面的meta

1、全屏显示

	<meta name="apple-touch-fullscreen" content="yes">

2、删除默认的苹果工具栏和菜单栏，默认显示

	<meta name="apple-mobile-web-app-capable" content="yes" />

3、设置状态栏样式，默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。

	<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
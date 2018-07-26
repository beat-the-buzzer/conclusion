### 手机端开发的原生交互(不定期更新)

参考地址：[http://www.html5plus.org/doc/h5p.html](http://www.html5plus.org/doc/h5p.html)

1、获取版本号。

这是在做自动更新功能的时候用到的，我们需要获取当前安装的app的版本号，然后通过请求获取最新版本的版本号，然后进行对比。如果安装的版本比较旧，就去下载最新版本的apk/ipa。

网上有很多获取版本号的方法，例如：

	plus.runtime.version;
	plus.runtime.innerVersion;
	navigator.appVersion;

但是，上面的方法有时候没有效果。

这里我们需要用到的方法是：

	plus.runtime.getProperty(plus.runtime.appid, function(res){
		// res里面有我们想要的信息
	});

[http://www.html5plus.org/doc/zh_cn/runtime.html#plus.runtime.getProperty](http://www.html5plus.org/doc/zh_cn/runtime.html#plus.runtime.getProperty)

如我所料不错，这个方法获取的版本号是manifest.json里面的版本号。所以，我们在发布新版本的时候，一方面要修改H5资源的manifest.json，另一方面，需要更新远程安装包地址的文件，并且需要更新获取最新版本号的接口。

2、获取网络连接状态

这是在做视频播放功能的时候用到的，我们需要获取当前设备的网络环境是否是WiFi。如果不是WiFi环境，就需要提醒用户是否要播放视频。现在4G网络很快，说不定，你一打开视频，流量就跑完了。

首先，我找到了navigator.onLine，随后发现，这个方法仅仅是判断当前设备有没有连上网，并不能判断网络的类型。并且，如果连上的是未配置的路由器，这个值也是true。所以，这是个鸡肋的属性。

可行的方法是navigator.connection.type


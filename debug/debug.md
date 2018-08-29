### Windows下调试webApp（android）

>原料：Hbuilder，安卓测试机，数据线，谷歌浏览器，翻墙

Hbuilder不是唯一的代码编辑器，这里以Hbuilder为例。

1、首先把手机设置成开发者模式，不同的手机有不同的设置方式，这个需要大家根据手机类型自己去寻找答案了，一般是多次点击版本号；

2、用数据线连上电脑，确保点击图中位置的按钮时，能看到你的手机型号；
![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/debug/debug1.bmp)

3、在Hbuilder中打开你的项目，然后点击上图中的按钮，如果手机成功连接的话，就会把app安装到你的手机上，安装期间，可能需要在手机上点击确定；

4、安装成功之后，打开app；

5、打开谷歌浏览器，在地址栏输入：chrome://inspect/#devices；

6、此时在浏览器上就能看到打开的H5页面了，这时候点击你想查看的页面，如果翻墙成功，就能看到你想要的东西了；

下面是成功的案例：
![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/debug/debug2.png)
![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/debug/debug3.jpg)

选中DOM，图中就会显示对应的DOM，点亮'element'左边的图标，DOM会在浏览器上显示，再点击一次，就能在手机上看到对应的DOM了。

注：这里解释一下翻墙的意义：翻墙的目的，可以理解为去下载一些谷歌提供的依赖文件。所以，如果你开了翻墙却没有效果的话，可能是依赖文件没有下载完成，这时候需要多试几次。只要翻墙可以成功，那么极大概率会成功。最后注意：如果翻墙成功了，就顺手把谷歌浏览器更新到最新版本吧。

### Windows下调试webAPP（ios）

知乎上面有这样一句话：个人不建议在win下做ios开发，除非您有超人般的意志和忍耐力，更或者您写的程式可以完美无误地运行，不然光是调试这一步，你都想要砸电脑了。

显然，ios开发如果不使用Mac，如同踢足球不用脚一样。但在这里，我说的是webApp的ios版，这里的调试都是针对H5代码，而ios的核心功能开发，都是用Mac的。因为ios和安卓的差异，有时候会遇到一些难以预料的问题，这时候，我们需要看到控制台console，但是正常情况下，我们看不到ios的控制台日志，这时候，就需要下面的一波操作了。另外，如果我们改动了H5代码，需要在设备上查看效果时，就必须要去再次打包，非常费时。下面的方法虽然也比较费时，但在Windows系统下，是一个比较不错的方法了。

>原料：Hbuilder，打包机，ifunbox、越狱后的ios设备，数据线，360免费WiFi，谷歌浏览器

1、点击Hbuilder上的发行->制作移动app资源升级包，得到一个文件，把这个文件的名字改成Pandora.zip；

![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/debug/debug4.bmp)

2、使用打包机生成ipa文件；

3、使用ifunbox等工具把ipa文件安装到ios设备上；

4、打开360免费WiFi，不管是什么工具，总之，需要开一个无线网，让你的手机连上你的无线网；

5、在手机设置中找到网络的IP地址；

6、打开安装的app,并且在浏览器的地址栏输入第5步的IP地址，如果看到这样的画面，就成功了一半；

![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/debug/debug5.png)

7、把第1步得到的文件上传到页面上，或者直接把这个文件拖动上去，上传成功之后，手机会提示你重启app;

8、至此，所有步骤完成，如果有代码的修改，就只需要修改Pandora.zip里面的H5代码，不需要再次打包成ipa;

9、控制台输出端口号：8081

在没有Mac的情况下，上面的操作算是高端操作了，不过要像安卓那样看到DOM结构貌似实现不了，UI类的问题，还是在安卓机上调试吧。




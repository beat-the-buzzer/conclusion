### 上传代码到GitHub

下图显示了git进行各种操作的流程，虽然有点复杂，但是初学的话，掌握几个常用的就行了；

![](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/upload/upload1.jpg)

1、正常上传代码、文件

	git init;

	git add .

	git commit -m '注释语句';

	git remote add origin https://
	
	git push -u origin master;

2、将代码、文件上传到指定目录

	git mergetool;
	//重复上述操作

由于指定目录可能存在一些文件，一般的解决方法是先pull，再push，或者强行push，但这些方法我都不喜欢。使用合并工具可以轻松解决问题。

3、在GitHub上展示项目（以redux-demo项目为例）

项目git地址：[https://github.com/beat-the-buzzer/redux-demo.git](https://github.com/beat-the-buzzer/redux-demo.git)

第一步：redux-demo是基于create-react-app的项目，因此，我执行这个命令：

	npm run build

得到了react项目的资源目录build

第二步：将build目录上传至master分支

第三步：执行命令：

	git subtree push --prefix=build origin gh-pages

执行完这个命令之后，资源文件全部放进了gh-pages分支

第四步：输入地址，访问页面

	https://beat-the-buzzer.github.io/redux-demo/
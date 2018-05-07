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
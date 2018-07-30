### react开发时候的一些问题(webpack版)

1、使用autoprefixer自动添加浏览器前缀

可以使用webpack配置autoprefixer-loader(deprecated配置)，不过这个方法以及被废弃了，现在推荐使用postcss来配置autoprefixer（后续会介绍postcss）

2、使用polyfill解决一些浏览器不支持ES6的问题

我在项目中使用了ES6中的Set，运行之后，在低版本的安卓手机上出错了，Set is not defined.

例如，我们现在用ES6写一个add方法，这个方法可以计算任意数量的参数的累加值：

	var add = (...args) => args.reduce((a,b) => a + b);

我们打开babel的官网，编译这段代码，结果是这样的：

	var add = function add() {
	  for (var _len = arguments.length, args = Array(_len), _key = 0; _key < _len; _key++) {
	    args[_key] = arguments[_key];
	  }
	
	  return args.reduce(function (a, b) {
	    return a + b;
	  });
	};

我们看到，我们所谓万能的babel，实际上只是在语法上把ES6转成ES5，但是，reduce方法是ES6才有的，如果浏览器不支持这个方法，就会出现错误。所以，我们需要polyfill来实现一个reduce方法，事实上，效果就类似这样：

	if(Array.prototype.reduce) {
		// do nothing
	} else {
		// 实现reduce
		Array.prototype.reduce = function(){
			// ......
		}
	}

polyfill添加方式：

	npm install -s babel-polyfill

在app.jsx中引入：

	import 'babel-polyfill';

3、使用eslint实现代码规范校验

eslint的配置可能会因为编辑器的不同而略有差异。这里使用的是VSCode。

首先安装ESlint扩展；

然后，运行命令eslint init一步一步进行配置；

这里需要一些安装依赖；

	"eslint": "^4.10.0",
    "eslint-config-airbnb": "^16.1.0",
    "eslint-plugin-import": "^2.10.0",
    "eslint-plugin-jsx-a11y": "^6.0.3",
    "eslint-plugin-react": "^7.8.1",

其中第二个是eslint按照哪种规则，第四、第五个是react特有的一些校验；

在VSCode中，点击文件->首选项->设置，可以查看用户设置。

	{
	    "window.zoomLevel": 1,
	    "eslint.options": {
	        "configFile": "D://.eslintrc.js"
	    },
	    "eslint.autoFixOnSave": false,
	    "git.autofetch": true,
	    "editor.tabSize": 2,
	    "explorer.confirmDelete": false,
	    "powermode.enabled": false
	}

其中比较关键的配置项是eslint.options，这个配置项可以指定ESlint规则文件的路径。另外，我们需要设置editor.tabSize为2，因为两空格缩进是airbnb规则的一部分。

再看.eslintrc.js文件，这个文件应该是执行init命令的时候自动生成的。我们要修改某些规则，就需要去修改这个文件的规则项。

	module.exports = {
	    "extends": "airbnb",
	    "rules": {
	        "linebreak-style": "off",
	        "no-plusplus": "off",
	        "no-restricted-syntax": "off",
	        "import/no-unresolved": "off",
	        "import/no-extraneous-dependencies": "off",
	        "import/extensions": "off",
	        "no-console": "off",
	        "class-methods-use-this":"off",
	        "no-else-return":"off",
	        "consistent-return":"off",
	        "react/prop-types":"off",
	        "prefer-destructuring":"off",
	        "jsx-a11y/label-has-for":"off",
	        "no-param-reassign":"off",
	        "quote-props":"off",
	        "jsx-a11y/click-events-have-key-events":"off",
	        "jsx-a11y/no-static-element-interactions":"off",
	        "jsx-a11y/no-noninteractive-element-interactions":"off",
	        "jsx-a11y/anchor-is-valid":"off",
	        "max-len":"off",
	        "no-nested-ternary":"off",
	    }
	};

4、使用fastclick解决移动端click延迟

移动端应用一般都会加上fastclick，为了解决click在移动端的延迟。另外，antd-mobile的Modal.alert模拟的警告框，如果没有fastclick，就会在ios设备中无法关闭。

webpack配置：
	
	npm install -s fastclick

在入口文件app.jsx中，引入fastclick.js，然后添加如下代码：

	if ('addEventListener' in document) {
	  document.addEventListener('DOMContentLoaded', function() {
	    FastClick.attach(document.body);
	  }, false);
	}

5、使用url-loader/file-loader解决文件加载路径问题

我们在开发的时候，一定会遇到和路径有关的问题。例如，把图片设置为div的background。在react开发中，我们可能使用了一种路径，但是打包之后，文件目录变了，这样容易造成找不到文件的问题。所以，我们需要配置url-loader/file-loader。

使用方法：

	npm install --save-dev url-loader
	npm install --save-dev file-loader

webpack里需要这样配置：

	{
	    test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
	    loader: 'url',
	    options: {
	      limit: 8192,
	      name: 'img/[name].[hash:4].[ext]',
	    },
	}




## Webpack

现今的web，都很丰富，它们拥有着复杂的JavaScript代码，一大堆依赖包，为了简化开发的复杂度，前端世界出现了很多很好的实践方法。

- 模块化
- sass,less等预处理器
- jsx，jade的模板
- 类似于TypeScript这种在JavaScript基础上拓展的开发语言
- ...

这些改进确实大大提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别，而手动处理是非常繁琐的，webpack应运而生！

### 什么是webpack

模块打包机：它做的事情就是，分析你的项目结构，找到javascript模块，以及其它的一些浏览器不能直接运行的拓展语言(less,sass,jsx),并将其打包为合适的格式，供浏览器使用。

### webpack 与 grunt/gulp相比

其实webpack跟它们俩没什么可比性，不是一回事。

- Grulp/Grunt是一种构建工具，能够优化前端的开发流程的工具.
	工作方式： 在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，这个工具之后可以自动替你完成这些任务。
- Webpack是一种模块化的解决方案，不过webpack的优点使得它可以代替Grunt/Gulp类工具。
	工作方式：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。

![webpack](images/webpack.png)

## 安装

```js
	npm install -g webpack //全局
	npm install --save-dev webpack //当前目录
	npm install --save-dev webpack@version
```

##  使用

- 终端命令行使用
	```js
		webpack <entry.js> <result.js>
		//其中entry.js是入口文件，result.js是打包后的输出文件
	```
	如果在终端中进行复杂的操作，还是不太方便且容易出错的.

- 使用配置文件

Webpack拥有很多其它的比较高级的功能（比如说本文后面会介绍的loaders和plugins），这些功能其实都可以通过命令行模式实现，但是正如已经提到的，这样不太方便且容易出错的，一个更好的办法是定义一个配置文件，这个配置文件其实也是一个简单的JavaScript模块，可以把所有的与构建相关的信息放在里面

	```js
		webpack --config webpack.custom.config.js
	```
- 更快捷的执行打包任务

npm可以引导任务执行,对其进行配置后可以使用简单的npm start命令来代替这些繁琐的命令。

```json
	//package.json
	"scripts": {
		"start": "webpack" //配置的地方就是这里啦，相当于把npm的start命令指向webpack命令
	 },
```
常用的命令：

- webpack 最基本的启动webpack命令
- webpack --config XXX.js //使用另一份配置文件(比如webpack.config2.js)来打包
- webpack --env.production  //指定构造环境，传入webpack配置文件中使用的环境变量。
- webpack --colors 输出结果带彩色，比如: 用红色显示耗时较长的步骤
- webpack --profile 输出性能数据，可以看到每一步的耗时
- webpack --display-modules 默认情况下node_modules下的模块会被隐藏，加上这个参数可以显示这些被隐藏的模块
- webpack --output-x //输出配置
- webpack --debug
- webpack --devtool
- webpack --progress
- webpack --module-x //模块配置
- webpack --watch 
- webpack --optimize-x 性能优化配置
- webpack --resolve 
- webpack 统计数据
- 高级配置

## 配置文件

项目中静态资源文件较多，使用配置文件进行打包方便很多，这个文件告诉webpack要做什么。

基本配置
```js
	module.exports = {
		entry: "./app/entry", //string | object | array 应用开始执行，webpack开始打包
		output: {
			path: path.resolve(__dirname, "dist"), //string
			filename: 'bundle.js',
			publickPath: "",
			library: '',
			library: 'umd',
			...
		},
		module: { //模块配置
			rules: [
				//模块规则【配置loader,解析器等选项
			]
		},
		resolve: { //解析模块请求的选项
			modules: [],
			extensions: ['', '.js', '.json', '.scss'],
			alias: {
				AppStore: 'js/stores/AppStores.js',
				...
			}
		},
		devtool: "source-map",
		context: "",
		target: "",
		externals: [],
		devServer: [],
		plugins: [],
		performance: {},
		...
	}
```
它是高度可以配置的，但是，在开始前，你需要先理解四个核心概念: 入口(entry), 输出(output), loaders, 插件(plugins).

- entry
	webpack 将创建所有应用程序的依赖关系图表(dependency graph).图表的起点被称为入口起点(entry point).入口起点告诉webpack从哪里开始，并遵循着依赖关系图表知道要打包什么。可以认为是app的第一个启动文件。
	```js
	module.exports = {
		entry: './path/to/my/entry/file.js'
	}
	```
- output
	将所有的资源归拢在一起后，还需要告诉webpack在哪里打包应用程序，就是如何处理归拢在一起的代码。
						
	```js
	const path = require('path');

	module.exports = {
		entry: './path/to/my/entry/file.js',
		output: {
			path: path.resolve(__dirname, 'dist'),
			filename: 'my-first-webpack.bundle.js'
		}
	};
	```
- loaders

鼎鼎大名的loaders登场了!

Loaders是webpack中最让人激动人心的功能之一了。通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种各样的格式的文件进行处理，比如说分析JSON文件并把它转换为JavaScript文件，或者说把下一代的JS文件（ES6，ES7)转换为现代浏览器可以识别的JS文件。或者说对React的开发而言，合适的Loaders可以把React的JSX文件转换为JS文件。

```js
	module.exports = {
		entry: './path/to/my/entry/file.js'
		output: {
			path: path.resolve(__dirname, 'dist'),
			filename: 'my-first-webpack.bundle.js'
		},
		module: {
			rules: [
				{
					test: /\.js$/,
					exclude: /node_modules/,
					loader: "babel-loader",
					options: {
						presets: [
							["es2015", {"modules": false}],
							"stage-0",
							"react"
						],
						plugins: [
							"transform-async-to-generator",
							"transform-decorators-legacy"
						]
					}
				},
			]
		}
	}
```

以上配置中，对一个单独的module对象定义了rules属性，里边的每一项基本都是loaders，里边包含两个必须属性：test 和 loader.

- test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
- loader: loaders的别名(新版)：loader的名称（必须）
- include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- options: 为loaders提供额外的设置选项（可选)

> “嘿，webpack compiler，当你碰到「在 require()/import 语句中被解析为 '.js' 或 '.jsx' 的路径」时，在你把它们添加并打包之前，要先使用 babel-loader 去转换”。

Webpack有一个不可不说的优点，它把所有的文件都可以当做模块处理，包括你的JavaScript代码，也包括CSS和fonts以及图片等等等，只有通过合适的loaders，它们都可以被当做模块被处理。

#### 推荐常用的loader

##### 文件

- file-loader 将文件发送到输出文件夹，并返回(相对)URL
- url-loader 同file-loader一样工作， 但如果文件小于限制，可以返回data URL

##### JSON

- json-loader 加载JSON文件

#### Transpiling 转换编译

- babel-loader 加载es6代码，使用babel转义为es5
- coffee-loader 像javascript一样加载CoffeeScript

#### Templating 模板

- html-loader 导出HTML为字符串，需要引用静态资源
- jade-loader 
- markdown-loader 
- react-markdown-loader

#### style 样式

- style-loader
- css-loader
- less-loader
- sass-loader
- postcss-loader
- stylus-loader

#### Linting && Testing (清理和测试)

- eslint-loader 
- jshint-loader
- mocah-loader

#### Frameworks 框架

- vue-loader


- plugins

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

Webpack有很多内置插件，同时也有很多第三方插件，可以让我们完成更加丰富的功能。

要使用某个插件，我们需要通过npm安装它，然后要做的就是在webpack配置中的plugins关键字部分添加该插件的一个实例（plugins是一个数组）继续看例子，我们添加了一个实现版权声明的插件。

```js
	const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
	const webpack = require('webpack'); //to access built-in plugins
	const path = require('path');

	const config = {
		entry: './path/to/my/entry/file.js',
		output: {
			path: path.resolve(__dirname, 'dist'),
			filename: 'my-first-webpack.bundle.js'
		},
		module: {
			rules: [
			{test: /\.(js|jsx)$/, use: 'babel-loader'}
			]
		},
		plugins: [
			new webpack.optimize.UglifyJsPlugin(),
			new HtmlWebpackPlugin({template: './src/index.html'})
		]
	};

	module.exports = config;
```

### 推荐常用的插件

- 优化插件
	1. OccurenceOrderPlugin :为组件分配ID，通过这个插件webpack可以分析和优先考虑使用最多的模块，并为它们分配最小的ID
	2. UglifyJsPlugin：压缩JS代码；
	3. ExtractTextPlugin：分离CSS和JS文件

```js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: ExtractTextPlugin.extract('style', 'css?modules!postcss')
      }
    ]
  },
  postcss: [
    require('autoprefixer')
  ],

  plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + "/app/index.tmpl.html"
    }),
    new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.optimize.UglifyJsPlugin(),
    new ExtractTextPlugin("style.css")
  ]
}
```

## 强大功能

### 调试

开发总是离不开调试，如果方便调试，可以提高开发效率，一般打包的代码不容易识别出错的位置，Source Maps就是来帮我们解决这个问题的。

在webpack中，配置devtool属性。来控制source maps

devtool的值有四种：

| devtool | 配置结果 |
| ------| ----- |
| source-map | 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包文件的构建速度； |
| cheap-module-source-map | 在一个单独的文件中生成一个不带列映射的map，不带列映射提高项目构建速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；|
| eval-source-map |  使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。不过在开发阶段这是一个非常好的选项，但是在生产阶段一定不要用这个选项；|
| cheap-module-eval-source-map | 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点； |

正如上表所述，上述选项由上到下打包速度越来越快，不过同时也具有越来越多的负面作用，较快的构建速度的后果就是对打包后的文件的的执行有一定影响。

> 开发环境推荐使用eval-source-map
> 生产环境使用：cheap-module-eval-source-map方法构建速度更快

### 构建本地服务器

想不想让你的浏览器监测你的代码的修改，并自动刷新修改后的结果，其实Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，可以实现你想要的这些功能，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖

```js
	npm install --save-dev webpack-dev-server
```

devserver作为webpack配置选项中的一项，具有以下配置选项

| devServer | 功能描述 |
| ------| ----- |
| contentBase |  默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到"public"目录）|
| port |  设置默认监听端口，如果省略，默认为"8080" |
| inline |  设置为true，当源文件改变时会自动刷新页面 |
| colors |  设置为true，使终端输出的文件为彩色的 |
| historyApiFallback |  在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html |

```js
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    colors: true,//终端中输出结果为彩色
    historyApiFallback: true,//不跳转
    inline: true,//实时刷新
	port: 3000
  } 
}
```

关于Webpack本文讲述得仍不完全，不过相信你看完后已经进入Webpack的大门，能够更好的探索其它的关于Webpack的知识了。










	


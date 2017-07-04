# 入职学习那些事儿

转眼间来公司已经一个月了，分享一下自己的所学所得...

刚入职的时候，拿到了一份前端开发概述和一份前端开发规范,瞬时觉得，搞得还蛮专业嘛,仔细一看也确实如此，规则很明确，描述很清晰

## 项目分析

经过一个月，对项目的了解和修改，有一些总结(frontend下的项目)

其实纵观我们的项目也是活生生的展现了一部web前端的发展史,从单纯的css,html,js的单纯书写，到amd规范的介入，到grunt构建工具的利器，到react,mobx机制的运用等，经历了很多，也丰富了很多。

1. 每个项目都有对应的文档说明，便于去分析项目结构。
2. 项目的目录结构很明确
3. 项目构建的时候，有代码规范检测
4. 对于用php渲染的项目，前端有mock模拟
5. 现在大部分项目或者说我们的重点项目，还处于php的渲染当中,这可能是由于时间维度或者php的渲染效率所决定的，我们可以更好的去维护和改进。
6. 有一些项目架构不同，运用的知识不同，但是不论用什么，他们都是工具而已，为了是实现我们需求。所以利于项目的就是好的，这样我们也可以多学习一些知识，归纳一些思路。

## 项目建议

纯属个人所建

1. 建议以后每个项目建立一个doc文件夹，放我们的需求文档，和设计文档。
	
	在维护项目的时候，迷茫的不是代码怎么改，而是想明白做的到底是什么事。

2. 建立自己的ui库

	在维护过程中，如商家管理后台，算是我们比较重要的项目了，还有pos后台等，我们发现他们就是传统意义上的后台管理，会经常用到一些表单，表格，分页等，当然了，我们这些组件有的已经独立出来了，但最后我们可以把他独立出项目，建立我们的ui库，对应后台应用来说，非常友好。

	比如叫westlife，托管到npm上，放入我们项目所用的，icon，button，tab等，也可以做一些日期，分页，轮播图等插件。
	方便我们开发使用。

3. 理想: 建立我们的项目架构。(理想)

	可以效仿express-generator, 直接创建项目目录结构，运行机制等，可以大大提高开发效率。


## 技术知识

在认识项目的过程中，学习了一些相关的知识.具体的api并不会详细列出，只是觉得先知道这些就可以开战了，在实际开发中自己google，才可以对相应的api印象深刻...

- [less](http://lesscss.org/)
- [mobx](https://mobx.js.org/)
- [grunt](https://gruntjs.com/)
- [eslint](http://eslint.cn/)
- [react](https://facebook.github.io/react/)
- [json-server](https://github.com/typicode/json-server)
- [react-router](https://github.com/ReactTraining/react-router)
- [webpack](https://doc.webpack-china.org/)
- [express](http://expressjs.com/zh-cn/)

以下做其中一些知识点的总结(简单粗暴一点)

- [less](http://lesscss.org/)
	预处理css的工具

	+ 使用方法
		```js
			npm install less --save //安装
			lessc style.less > style.css //编译

			//当然编译的时候可以加入一些参数，做优化
			lessc style.less > style.css -x //压缩
		```
	+ 语法

		1. 变量
		
			使用@+变量名来定义一个变量
			```html
				
				//less

				@color: #000;
				@border: 1px solid #000;

				.icon {
					border: @border;
					color: @color;
				};

				//css

				.icon {
					border: 1px solid #000;
					color: #000;
				}

			```

		2. 注释 
			
			分为单行和多行注释

			```js
				//单行注释，在编译过程中会被忽略掉

				/*!
				* 也可不用叹号，叹号的作用，不会在压缩的时候被去除，用于声明版权.
				*/

			```

		3. 嵌套语法
			
			我们可以在一个选择器中嵌套另一个选择器来实现继承，这样很大程度减少了代码量，并且代码看起来更加的清晰。
			```js
				// less
				#header {
					p {
						font-size: 12px;
						a {
							text-decoration: none;
							&: hover {
								border: 1px solid #000;
							}
						}
					}
				}

				//生成的css
				#header p {
					font-size: 12px;
				}
				#header p a {
					text-decoration: none;
				}
				#header p a:hover {
					border: 1px solid #000;
				}
				
			```

		4. Minin混合
			
			混合可以将一个定义好的class A轻松引入到另一个class B中，从而简单实现class B继承class A中所有属性。我们还可以带参数调用，就像使用函数一样。

			```js
				// less
				.icon (@width: 10px, @height: 10px, @backgroundColor: @fff;) {
					display: inline-block;
					width: @width;
					height: @height;
					background-color: @backgroundColor;
				}

				.icon1 {
					.icon(15px, 10px, #000);
				}
				//css
				.icon1 {
					display: inline-block;
					width: 15px;
					height: 10px;
					background-color: @000;
				}
			```

		5. import导入
			
			less支持导入其他的less或者css文件，用import
			```js
				@import url('a.less');
			```

		6. 函数 & 运算

			运算提供了加减乘除操作，我们可以做属性和颜色的运算，这样就可以实现属性值之间的复杂关系。

			```js
				//less
				@border: 1px;
				@color: #111;

				#header {
					color: @color * 3;
					border-left: @border * 2;
					border-right: @border * 3;
				}

				//css
				#header {
					color: #333;
					border-left: 2px;
					border-right: 3px;
				}


			```
			
		当然这些是基础的入门，但也够用了，当然可以深究，写的less比js可能还强大，可复用。
		

- [mobx](https://mobx.js.org/)

	基础的知识和api可以查看栾总的文章,里边有五篇文章来介绍mobx。

	（关注公众号,微生活前端开发)请大家掏出手机来搞事情...

	![公众号](https://mp.weixin.qq.com/mp/qrcode?scene=10000004&size=102&__biz=MzI0MDYzOTEyOA==&mid=2247483727&idx=1&sn=4961e7a7a0bd9922fe2559da7bcebe36&send_time=)

	当然了，我在这里重点说一下react中的使用，相比于redux，mobx太灵活了，灵活的总是想给他个壳子套用。

	+ `@` es6新增的装饰器语法decorators, babel以支持，需要安装 babel-plugin-transform-decorators-legacy
	+ `类的静态属性` es7新增语法，babel需要安装babel-preset-stage-2
	+ `@observer` 让React组件自动起了，它会自动更新，即便是一在一个很大的程序里也会工作的很好
	+ `@observable` 监听数据，当数据发生改变的时候，自动刷新视图
	+ `computed` 创建自动运算的表达式,一般用于计算
	+ `action` 改变@observable创建的数据，需要装饰action方法
	+ `autorun` 当observable创建的数据发生改变时自动执行。

	可以查看项目plguins_platform,简单的这么运用了一下。

	```js
		npm install mobx-react --save // 提供了Provider 

		ReactDom.render(
			<Provider {...stores}> //把store传入给每个组件
				<Router history={ browerHistory }>
					<Route path="/" component={ WebApp }>
						<IndexRoute component={ Home } />
						<Route path="home" component={ Home } />
						<Route path="explore" component={ Explore } />
						<Route path="detail/:id" component={ Detail} />
					</Route>	
				</Router>
			</Provider>,
			document.getElementById('webapp')
		);
	```

	```js
		//定义store
		import { observable, action, runInAction, useStrict } from 'mobx';
		import { GET } from 'utils/request';

		useStrict(true);

		class Store {
			@observable title;

			constructor() {
				this.title = "home";
				this.hotPlugins = [];
				this.newPlugins = {};
				this.banner = [];
			}

			@action setTitle(title) {
				this.title = title;
			}
		}

		const HomeStore = new Store();

		export default HomeStore;
	```

	```js

		//用mobx-react中提供的装饰器去动态更新render dom
		import React from 'react';
		import './style'
		import { inject, observer } from 'mobx-react';
		import { Banner, Card } from 'components';

		@inject("HomeStore")
		@observer
		export default class Home extends React.Component {
			constructor(...args) {
				super(...args);
			}

			componentWillMount() {
				this.props.HomeStore.getHomeData();
			}

			render() {
				...
			}
		}

	```

	其实mobx中的细节还挺多，尤其一些函数函数的运用可以提高我们很多效率，望深究。

- [grunt](https://gruntjs.com/)

	一句话：自动化。

	对于需要反复重复的任务，例如压缩，编译，单元测试，linting等，自动化工具可以减轻你的劳动，简化你的工作，grunt生态系统非常庞大，并且一直在增长，你可以利用grunt自动完成任何事情。

	+ 安装
		
		```js
			npm install grunt-cli --save-dev
		```

	+ Gruntfile

		放在根目录中，和package.json在同一目录层级。

		Gruntfile.js由一下几部分构成:

		- `wrapper` 函数
		- 项目与任务配置
		- 加入grunt插件和任务
		- 自定义任务

		```js
		module.exports = function(grunt) {

		  // Project configuration.
		  grunt.initConfig({
			pkg: grunt.file.readJSON('package.json'),
			uglify: {
			  options: {
				banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
			  },
			  build: {
				src: 'src/<%= pkg.name %>.js',
				dest: 'build/<%= pkg.name %>.min.js'
			  }
			}
		  });

		  // 加载包含 "uglify" 任务的插件。
		  grunt.loadNpmTasks('grunt-contrib-uglify');

		  // 默认被执行的任务列表。
		  grunt.registerTask('default', ['uglify']);

		};
		```

		1. `wrapper`函数

			每一份Gruntfile都遵循同样的格式，你所书写的grunt代码必须放在此函数内；

			```js
				module.exports = function(grunt) {
					//...
				}
			```

		2. 项目和任务配置

			大部分Grunt任务都依赖某些配置数据，这些数据被定义在一个object内，并传递给grunt.initConfig方法。

			```js
				grunt.initConfig({
				  pkg: grunt.file.readJSON('package.json'),
				  uglify: {
					options: {
					  banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
					},
					build: {
					  src: 'src/<%= pkg.name %>.js',
					  dest: 'build/<%= pkg.name %>.min.js'
					}
				  }
				});
			```

		3. 加载Grunt插件和任务

			```js
				//加载能够提供"uglify"任务插件
				grunt.loadNpmTasks('grunt-contrib-uglify');
			```

		4. 自定义任务

			```js
				grunt.registerTask('default', ['uglify']);
			```

		


- [eslint](http://eslint.cn/)

	ESlint的设计是完全可配置的，意味着你可以关闭每一个规则，只运行基本语法验证，或混合和匹配绑定的规则的自定义规则。

	+ 特点
	
		默认规则包含所有JSLint, JSHint中存在的规则，易迁移；
		规则可配性高: 可设置[警告]，[错误], 两个error等级，或者直接禁用；
		包含代码风格检测的规则
		支持插件扩展，自定义规则.

	+ 使用方式
		
		可以通过三种方式配置ESLint:
		
		- 使用.eslintrc文件(支持JSON 和 YAML两种语法);
		- 在package.json中添加eslintConfig配置块;
		- 直接在代码文件中定义(在javascript注释中写ESLint配置信息).

	+ 属性
		
		| 配置 | 意义 |
		| --- | --- |
		| parser | 指定不同的解析器 |
		| evn | 指定环境 |
		| globals | 指出你要使用的全局变量 |
		| plugins | 在配置文件里使用第三方插件，在使用之前，你必须npm安装它 |
		| rules | 指定代码规则，并且制定每一条规则的警告级别(error level) |
		| extends | 如需扩展一个特定的配置文件，配置extends项就可以了, 也可以用来引用一些在github上面共享的配置包，首先需要npm安装配置包文件 |

		demo

		```js
			{
				"env": {
					"browser": true,
					"node": true,
					"commonjs": true,
					"es6": true,
				},
				"parserOptions": {
					"ecmaVersion": 6,
					"sourceType": "module",
					"ecmaFeatures": {
						"globalReturn": true,
						"jsx": true,
						"modules": true,
						"experimentalObjectRestSpread": true
					},
					"rules": {
					}
				},
				"globals": {
					"document": false,
					"escape": false,
					"navigator": false,
					"unescape": false,
					"window": false,
					"describe": true,
					"before": true,
					"it": true,
					"expect": true,
					"sinon": true
				},
				"parser": "babel-eslint",
				"plugins": [
					"react",
					"babel"
				],
				"extends": [
					"eslint:recommended",
					'plugin:react/recommended'
				],
				"rules": {
					"indent": ["error", "tab", {
					  "SwitchCase": 1,
					  "VariableDeclarator": 1,
					  "MemberExpression": 1,
					  "ObjectExpression": 1
					}],
					"no-mixed-spaces-and-tabs": ["error", true],
					"no-spaced-func": "error",
					"no-with": "error",
					"block-scoped-var": "error",
					"brace-style": ["error", "1tbs"],
					"default-case": "error",
					"no-console": "off",
					"no-unused-vars": ["warn", {
					  "vars": "all",
					  "args": "after-used",
					  "varsIgnorePattern": "^(React)$",
					  "argsIgnorePattern": "^(e|err|reject|progressEvent)$",
					}],
					"react/prop-types": "off",
					"react/display-name": "off",
				}

		```

		每项的说明可以在官网查询到.


- [react](https://facebook.github.io/react/)

	无话可说了已经...
	一个图够了
	![react](http://upload-images.jianshu.io/upload_images/326507-281c610cec06a015.png?imageMogr2/auto-orient/strip%7CimageView2/2)


- [json-server](https://github.com/typicode/json-server)

	在开发过程中，前后端不论是否分离，接口多半是滞后于页面开发的。所以建立一个REST风格的API接口，给前端页面提供虚拟的数据，是非常有必要的。

	对比过多种mock工具后，我最终选择了使用 json server 作为工具，因为它足够简单，写少量数据，即可使用。

	也因为它足够强大，支持CORS和JSONP跨域请求，支持GET, POST, PUT, PATCH 和 DELETE 方法，更提供了一系列的查询方法，如limit，order等。下面我将详细介绍 json server 的使用。

	+ 安装
	
		```js
		 npm install json-server --save-dev
		```

	+ 创建数据文件(.json/js)

		```js
			//db.json
			{
				"posts": [
					{
						id: 1,
						title: 'json-server'
					},
					{
						id: 2,
						title: "mock"
					}
				],
				"comments": [
					{
						id: 1,
						content: "very good !",
						postId: 1
					}
				],
				"profile": {
					"name": "yulong"
				}
			}
		```
	+ 运行
		
		```js
			json-server --watch db.json
		```

		会启动默认端口为3000的服务

		运行参数配置如下:

		```js
			json-server [options] <source>

			Options:
			  --config, -c       指定 config 文件                  [默认: "json-server.json"]
			  --port, -p         设置端口号                                   [default: 3000]
			  --host, -H         设置主机                                   [默认: "0.0.0.0"]
			  --watch, -w        监控文件                                           [boolean]
			  --routes, -r       指定路由文件
			  --static, -s       设置静态文件
			  --read-only, --ro  只允许 GET 请求                                    [boolean]
			  --no-cors, --nc    禁止跨域资源共享                                   [boolean]
			  --no-gzip, --ng    禁止GZIP                                          [boolean]
			  --snapshots, -S    设置快照目录                                     [默认: "."]
			  --delay, -d        设置反馈延时 (ms)
			  --id, -i           设置数据的id属性 (e.g. _id)                     [默认: "id"]
			  --quiet, -q        不输出日志信息                                     [boolean]
			  --help, -h         显示帮助信息                                       [boolean]
			  --version, -v      显示版本号                                         [boolean]

		```

	+ 访问

		```html
			http://localhost:3000/posts
		```

		返回结果
		```json
			[
				{
					id: 1,
					title: 'json-server'
				},
				{
					id: 2,
					title: "mock"
				}
			]
		```
	
		

文档没有写那么全面，可能有些以偏概全，日后有机会一一分享给大家。

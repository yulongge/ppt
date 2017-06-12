## web幻灯片

### 准备工作

- [node](https://nodejs.org/en/)
- [yeoman](http://yeoman.io/)
- [generator-sildes](https://github.com/leftstick/generator-slides)
- [sero-cli](https://github.com/leftstick/Sero-cli)


### 安装

```js
npm install -g yo
npm install -g generator-slides
npm install -g sero-cli
```

### 创建

```js
yo slides
```

然后命令会提示各种问题等你回答

- Your slides name : 幻灯片名称
- Your slides description : 幻灯片描述
- Your name : 你的名字
- Your email : 你的邮箱
- Choose a theme of this sliders : 选择一个主题样式
- Choose plugins you'd like to add : 选择你需要的插件(一般选择markdown)

### 预览

进入创建的项目目录，用sero启动一个web服务

```js
	sero
```

![sero](/img/sero.png)

打开 http://localhost:8080

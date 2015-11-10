###前言
这是一个小白学习[“Modern WEB Developer ”](http://qd.haoduoshipin.com/)课程“前端开发技术”部分的一个感悟，也是作为一名执着的程序员的一次“填坑”复习。

首先要先感谢[HappyPeter](https://github.com/happypeter)老师精心出品的一个精品的课程，让我有一个合理的方法去学习主流的WEB前端技术以及工具。也非常感谢Peter老师耐心帮助我们这些学院，耐心帮我们填坑，在此再次拜谢！

###技术栈
* webpack
* gulp
* React.js
* Sass
###工作环境
Ubuntu14.04  64位操作系统

###开始
######第一阶段：
**安装nodejs**
推荐使用**NVM**的方式安装：
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash
```
查看有哪些版本可以被提供安装：
```
nvm ls-remote
```
可以看到当前最新版本是 v4.2.1 ，运行下面的命令来安装：
```
nvm install v4.2.1
```
这样就安装好了node.js，通过命令查node版本号来查看是否安装成功：
```
node -v
```
【提示】如果之前安装过nodejs，可能会有版本冲突，就是每次重新启动Shell，Node都还是老版本，解决这个问题可以通过nvm去设置默认版本：
```
$ nvm alias default 4.2.1
default -> 4.2.1 (-> v4.2.1)
```
【提示】可以让 npm 使用淘宝镜像（cnpm）：[淘宝镜像地址点我就送！](http://npm.taobao.org/)
cnpm的优势就是速度快，其他没什么区别，我在配置的时候**失败了**,所以一直用的npm，因为自己的电脑配置了代理，所以速度也不差。

回到正文，**Gulp**和**Webpack**都可以进行文件的压缩和打包，组合起来使用非常棒，gulp和webpack功能上有重叠，但是也很互补。
我就直接使用的Webpack这个工具，等以后做了大型项目，再结合起来使用。

**【备忘区】自己在此做个关于Gulp的备忘～**

全局安装gulp：
```
➜  ~  npm i -g gulp
/home/martin/.nvm/versions/node/v4.2.1/bin/gulp
```
可以看到 gulp 这个包安装到了 /Users/peter 之下。并且可执行文件也自动导出为系统命令了，可以通过运行下面的命令来确认：
```
➜  ~  which gulp
/home/martin/.nvm/versions/node/v4.2.1/bin/gulp
```

进入项目后，还要把 gulp 在项目内安装一次
```
➜  ~ npm init
➜  ~ npm i --save-dev gulp
```
要先用```npm init```去初始化工程，生成一个package.json文件，```npm i --save-dev gulp```可以用```npm i -D gulp```代替。
>这样做的目的是，保证项目内部的 gulpfile.js 中，使用 `require('gulp')` 的时候不会报错。

这是Peter老师告诉我们的，目前我还没遇到这个报错的情况，所以也不是很清楚，照做就好。

**安装 sass**

到项目文件夹内
```
npm i --save-dev gulp-sass
```

注意，gulp-sass 依赖于 libsass ，这是一个 C++ 的包，需要在本地编译，所以要确保本地 Mac 机器上是有 Xcode 的。 装好之后，gulpfile.js 中写下面的内容
```
var gulp = require('gulp'); 
var sass = require('gulp-sass'); 

gulp.task('sass', function(){ 
	gulp.src('main.scss') 
	.pipe(sass()) 
	.pipe(gulp.dest('css')); 
	}
);
```
这样每次运行 `gulp sass` 就可以把 main.scss 文件，编译输出为 main.css 了。

强大的 gulp 管道线
比如，还可以扩展添加 **gulp-autoprefixer**(自动添加厂商前缀 器) 进来
```
$ npm i gulp-autoprefixer -D
```
然后到 gulpfile.js 中添加
```
  var prefix = require('gulp-autoprefixer');
        .pipe(prefix())
```
这样，在输出的 main.css 中，就可以看到 vendor prefix 已经自动添加了。

**【备忘到此结束】**
#####webpack
安装
首先要进行全局安装：
```
npm i -g webpack
```
这样的目的是让我们拥有 webpack 这个系统命令。
项目内部也还是要再装一遍的，这样才能在项目代码中 require 到。
``` 
npm i --save-dev webpack
```
好，这样安装结束了，webpack 被安装到 qd/node_modules/ 文件夹里面，同时 package.json 文件中会有记录。

**项目中的使用**

webpack 的心脏就是 webpack.config.js 文件（需要自己在项目的根目录创建），到 [webpack 官方文档](https://webpack.github.io/docs/tutorials/getting-started/) 拷贝一份基本的配置内容粘贴到代码中：
```
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```
* entry参数定义了打包后的入口文件
* path: 打包文件存放的绝对路径
* filename: 打包后的文件名

建议可以先跟着[webpack 官方文档](https://webpack.github.io/docs/tutorials/getting-started/)里的新手教程走一边，感受一下webpack导入文件的方式，对使用webpack有很大帮助。
可以在[深入浅出React（二）：React开发神器Webpack](http://www.infoq.com/cn/articles/react-and-webpack?utm_campaign=rightbar_v2&utm_source=infoq&utm_medium=articles_link&utm_content=link_text)这篇文章里学习一下webpack，可以帮助你理解webpack的工作。

有了上面的代码，运行
```
webpack
```
就可以得到输出文件 bundle.js 了。

**项目中安装 react**

```
npm i -D react react-dom
```
**使用 loader**

loader的作用就是**翻译**，在这可以将React的jsx格式的文件翻译成纯js文件。
我们只需要在项目中安装就可以：
```
npm i -D babel-loader
```

安装好这个 loader 之后，就可以修改 webpack.config.js 文件，这样 webpack 就有能力把 jsx 翻译成纯 js 文件了
```
 module: {
         loaders: [
             //babel           
            { 
                 test: /\.js$/, 
                 loader: "babel" 
            }
         ]
     }
```
这样再运行：
```
webpack
```

使用gulp可以处理sass，使用webpack同样可以。
使用gulp是通过“管道线”，而使用webpack则是使用“loader”。

安装loader：
```
npm i -D style-loader css-loader autoprefixer-loader sass-loader
```
【提示】也许您那边不会出现下面的问题的，万一出现，解决方法如下：
是这样，上面的装包命令执行报错了：
```
npm ERR! peerinvalid The package node-sass does not satisfy its siblings' peerDependencies requirements!
npm ERR! peerinvalid Peer sass-loader@3.1.0 wants node-sass@^3.3.3
```
问题显然是我这里的 node-sass 这个包版本太低了，所以需要升级一下：
```
$ npm install -D node-sass
...
> node-sass@3.4.0 install /Users/peter/Desktop/modern-web-dev-stuff/qd/node_modules/node-sass
...
```
输出信息中看到安装的是 3.4.0 版本，满足了前面报错信息中要求的 ^3.3.3 （高于 3.3.3，对 package.json 各种格式的意义感兴趣？[参照这里](http://browsenpm.org/package.json)） 。所以在此安装各个 loader ，就能成功了。

现在需要修改下 webpack.config.js ，来添加 loader 的配置
```
    module: {
        loaders: [
          { 
              test: /\.scss$/, 
              loader: 'style!css!autoprefixer!sass' 
          }
        ]
    }
```
现在webpack就是有了babel-loader工具而且可以加载css解释sass并且可以自动添加厂商前缀的升级版。

导入 sass 文件。
到 entry.js 中把 main.scss 导入一下就可以了。
```
require('./styles/main.scss');
```
这里的main.scss是我自己的sass文件，你只需要导入自己的即可，注意文件路径。

打开index.html，添加```<script src="bundle.js"></script>```到body底部。
```
<html>
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<div id="test"></div>
		<div style="background:red"></div>
		<script src="bundle.js"></script>
	</body>
</html>
```
**【注意】**index.html此时的位置是在文件根目录下的，如果在其他位置，要修改```<script src="bundle.js"></script>```中bundle.js的路径位置。

然后再执行：
```
webpack
```
**【提示】**这里执行webpack的时候，可能会报错，这是由于```babel-loader 6.0```这个坑，所以回滚到低版本就可以解决。

操作是比较简单的，一条命令搞定，到项目文件夹内
```
npm i -D babel-loader@5.3.2
```
这样之后，效果是：
原来的 **babel-loader** 被删除
新安装的 **5.3.2 版本的 babel-loader **被安装到 node_modules 文件夹内
package.json 中也记录了这个 5.3.2 版本
```
-    "babel-loader": "^6.0.0",
+    "babel-loader": "^5.3.2",
```

#####代码热替换
使用：[React Hot Loader](http://gaearon.github.io/react-hot-loader/getstarted/),它的作用就是页面更新而不刷新。

需要安装两个包：
```
npm i -D react-hot-loader webpack-dev-server
```
其中 webpack-dev-server 是 webpack 在开发模式下的 server 。
具体配置教程可以在[点我就送Hot-Loader教程]（http://gaearon.github.io/react-hot-loader/getstarted/）获得。

#####以下内容来自Peter老师课程详解：
下面来解释一下几处比较重要的修改的意义：
```
devtool: 'eval',
```

根据官方文档 上的说明，上面一句的作用是可以提高编译的速度。

```  
entry: [
        'webpack-dev-server/client?http://localhost:3000',
        'webpack/hot/only-dev-server',
        './src/main.js'
    ],
```
上面这几行的作用可见 react-hot-loader 网站

 ```publicPath: '/static/'```

为何要加上面这句呢？因为在开发模式下，并不真正需要真的编译出 bundle.js 文件并把它保存到硬盘上。只需要把 bundle.js 的保存到内存中就可以了，但是总要有一个位置去指定它的，所以就有了上面这句，文件系统上我们是看不到这个文件夹的。对应的 index.html 中要写 
```
<script src="/static/bundle.js"></script>
``` 
来引用这个虚拟文件。

``` 
path: path.join(__dirname, 'dist'),
```

* publicPath: 网站运行时的访问路径

这个正好是与 publicPath 相对的。这个是 bundle.js 的编译输出位置，保存成一个实际硬盘上的文件，用来部署项目。
```
{ 
    test: /\.js$/, 
    loaders: ['react-hot', 'babel'], 
    include: path.join(__dirname, 'src')
}
```
上面的 include 选项指定了 react-hot-loader 要跟踪哪些文件。上面我们把这个位置设置为 src 文件夹，所以需要把一些文件移动进去，具体是哪些文件，参考上面 git commit 的代码。


这样源码中我们做任何更改，页面上都立刻会更新了。而且由于有 react-hot-loader 的支持，页面不会刷新，我们所谓的“浏览器内设计”的环境也就达成了。

####精华（填过的坑）在此

**Issue  N0.1**
[问题代码实例](https://github.com/Martin-zhz1994/webpack-problem-1)
现在hot-loader可以启动，但是无法获取entry.js文件。运行webpack命令，然后打开index.html文件，发现entry.js也没有被执行。但是用  entry: "./src/entry.js", 这个却可以。
所以我觉得可能出现问题的是两个地方，一个是bundle.js的文件生成出现了问题，另一个就是hot-loader的问题。
package.json检查过了，我也不是很清楚哪里还有冲突，babel已改为5.x版本。

如果用
```
 entry: [
        'webpack-dev-server/client?http://localhost:3000', 
        'webpack/hot/only-dev-server', 
        './src/entry.js' //这样页面无法加载entry.js
    ],
```
这个方式的话，打开localhost：3000 ，控制台会报错
```
Uncaught TypeError: Cannot read property 'WebSocket' of undefined
```
Peter老师解答：

```
Uncaught TypeError: Cannot read property 'WebSocket' of undefined
```

websocket 是为了页面在 dev server/hot-loader 的模式下自动刷新页面，而由 react-hot-loader 植入进代码的。所以在我们运行 webpack 命令编译输出 bundle.js 的时候，这时候其实是产品环境了，不再需要 websocket 。

所以，在编译生成 bundle.js 之前，应该把 webpack.config.js 中的 hot-loader 相关的设置都删除掉。然后在运行 `webpack` 命令进行编译。（也有人专门创建一个 webpack.production.js 文件，然后编译的时候就执行：`webpack --config  webpack.production.js`）


发现您已经找到了正确的解决方法。就是使用：https://github.com/Martin-zhz1994/webpack-problem-1/blob/master/webpack.config.js#L5

刚才试了一下，把 https://github.com/happypeter/qd 中的 package.json 和 webpack.config.js 文件拷贝到您的项目中，重新装一下包，然后 index.html 中在改为 `/static/bundle.js` 。这样您的项目是可以直接运行的。

所以建议是：

- webpack 还处于幼年，很多接口是几天一小变，一周一大变，没有必要花时间把语法结构搞清楚，得不偿失
- 直接用我项目中的配置文件就好了，如果要找问题，对比一下 `git diff` 总是会找到的，但是没有太大意义


Peter老师，entry.js貌似还是不起作用，在Chrome控制台中，看到我在test.js里添加的标签并没有出现，而entry.js里```ReactDOM.render(<test />, document.getElementById('test'));```这句话在index.html中出现的语句是
```
<test data-reactid=".0"></test>
```
但是
```
require('./styles/main.scss');
```
这句话可以工作了。


Peter老师解答：
#####有一点要主要，组件名和组件文件名的首字母都要大写。
大写这个问题，不是 webpack 的规定。是 React 自身的规定。因为组件被使用的时候，语法是一个 xml 标签 ( e.g <Hello /> ），首字母大写才能和普通的 html 标签进行很好的区分。


这句话解决了我搞了三天的问题。——。——！！！

**Issue NO.2**


Peter老师，还是不是很理解
```
path: path.join(__dirname, 'dist'),
+        filename: "bundle.js",
+        publicPath: '/static/'
```
这个地方，publicPath是网站运行时的访问路径，那既然static是存在与内存中的，我们调试结束了，我可以理解为它就被保存到硬盘上了吗？

path: path.join(__dirname, 'dist')
path：打包文件存放的绝对路径

>这个正好是与 publicPath 相对的。这个是 bundle.js 的编译输出位置，保存成一个实际硬盘上的文件，用来部署项目。

那path.join怎么理解？但是在结束server.js之后，我的项目目录里并没有看见bundle.js这个文件的生成？


在实际项目中，如果没有bundle.js,那webpack怎么解释打包组件？

Peter老师解答：
这里会涉及到两个很有用的概念，一个叫开发模式，一个叫产品模式。开发模式就是在 webpack dev server 在运行的时候，我们就处于这个模式。这时，是不会有 bundle.js 被保存到硬盘上的。调试结束了也不会。产品模式就是产品真正上线了。这个最关键的是，我们需要有一个 bundle.js 的硬盘文件，这样才能把他上传到服务器上进行部署。那么，如何才能得到这个文件呢？
运行
```
webpack
```

就可以了

**Issue NO.3**
在加载hot-loader出了问题。
```
➜  MakerLab git:(master) ✗ node server.js
Listening at localhost:3000
[5]    6666 bus error (core dumped)  node server.js
```
代码地址：https://github.com/Martin-zhz1994/MakerLab


- 首先在您的机器上，运行一下：http://qd.haoduoshipin.com/p/boilerplate 
  - 注意：有些老版本的 Mac 上，运行这个项目也会不成功，需要升级 Mac 才行

- 如果 boilerplate 可以运行成功，那么您的代码也应该是可以运行起来的，可以对照：
  - qd 项目，看一下有没有地方敲错了
  - 不行的话，对照看一下 您的 Package.json 文件中，是否是有包的版本和 boilerplate 项目中不同
    - 因为有些高版本的包，例如 babel-loader 6.x.x，和老版本的使用方法已经变了，所以也可能造成错误
    - 解决方式就是把这个包的版本降下来



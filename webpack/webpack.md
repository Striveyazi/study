### 1.what the fuck

​	webpack是在应用程序中构建JavaScript模块的工具,可以通过cli和api来使用。webpack通过快速构建应用程序的依赖图并按正确的顺序绑定它们来简化工作流。webpack可以自定义的配置来优化代码，拆分vendor的css 、 js代码进行生产，无需页面刷新就能进行热加载，运行在开发服务器上，等很多cool的功能。

### 2.installation

前提：

​	新版本的Node.js

本地安装：//标准和推荐的做法

​	npm install webpack --save-dev

​	npm install webpack@<version> --save-dev

全局安装：

​	npm install webpack -g

### 3.getstart

​	根据模块需要的依赖关系，webpack可以使用此信息来构建依赖关系图，然后使用图形生成优化的捆绑包，其中脚本将以正确的顺序执行。**此外，未使用的依赖关系不会包含在捆绑包中。**

a.创建一个文件夹：webpack-demo,并且初始化package.json

```
mkdir webpack-demo && cd webpack-demo //创建文件夹
npm init -y //初始化npm包
```

b.安装webpack 

```
npm install --save-dev webpack
```

c.新建 /app/index.js

​	安装lodash，让index.js来显示引用

```javascript
import _ from 'lodash'; //也可以不import，这样让index.js来隐式的引用lodash，因为index.js没有为lodash定义，只是假设'_'全局变量存在。这样就导致隐式的引用。这样会导致一些问题。当没有依赖项，或者依赖性顺序出错，会导致程序不能正常工作。还有如果包括一个依赖，但没有使用，那么浏览器必须下载很多不必要的代码。
function component(){
    var element = document.createElement('div');
    element.innerHTML = _.join(['hello','webpack'],'');
    return element;
}
document.body.appendChild(component);
```

d.新建webpack.config.js

```javascript
var path = require('path');

module.exports = {
  entry: './app/index.js',//入口文件
  output: {
    filename: 'bundle.js',//输出文件
    path: path.resolve(__dirname, 'dist')
  }
};
```

e.新建/index.html，引用输出文件

```html
<html>
  <head>
    <title>webpack 2 demo</title>
  </head>
  <body>
   <script src="dist/bundle.js"></script>
  </body>
</html>
```

f.在package.json中scripts中新增一项

```json
"build": "webpack"
```

```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^2.2.1"
  },
  "dependencies": {
    "lodash": "^4.17.4"
  }
}

```

g.运行webpack

```
npm run build
```

h.结果

```c
Administrator@LocalServer MINGW64 /f/webpack-demo
$ npm run build -- --color

> webpack-demo@1.0.0 build F:\webpack-demo
> webpack "--color"

Hash: 4e8f45e0420cfc419cb7
Version: webpack 2.2.1
Time: 697ms
    Asset    Size  Chunks                    Chunk Names
bundle.js  544 kB       0  [emitted]  [big]  main
   [0] ./~/lodash/lodash.js 540 kB {0} [built]
   [1] (webpack)/buildin/global.js 509 bytes {0} [built]
   [2] (webpack)/buildin/module.js 517 bytes {0} [built]
   [3] ./app/index.js 216 bytes {0} [built]
```

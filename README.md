# 利用webpack实现对html文件的热更新

&ensp;&ensp;webpack中webpack-dev-server是一个简单的web服务器，可以帮助我们实现代码的热更新，即在实际开发中只需保存修改完后的代码，不用手动刷新页面就可以看到效果。在使用webpack-dev-server时，会发现在对js、css文件中的代码修改时，可以很容易地实现页面热更新；修改html文件内容时，保存代码，页面并没有自动更新。（可以在html-hot-reload项目的中demo1分别修改html、js、css文件试一试）。

&ensp;&ensp;这是因为webpack在运行时它会根据webpack.config.js中入口文件（entry）来开始查询所有的依赖并根据不同的处理器（loader）、插件（plugin）来解析、打包，webpack-dev-server会实时监控webpack打包后的文件并实现热更新。js、css文件很容易实现热更新是因为js文件一般为入口文件或者js文件、css文件与入口文件存在依赖关系，在webpack打包后受到webpack-dev-server的实时监控。由于html与入口文件不存在依赖关系，所以实现不了热更新。可以通过以下两种方法实现对html文件热更新。

## 方法一：利用html-webpack-plugin插件

> html-webpack-plugin插件就是在webpack打包时重新生成一个html文件

> （可以在html-hot-reload项目的中demo2中分别修改html、js、css文件查看效果）

```
1、安装html-webpack-plugin插件

npm install --save-dev html-webpack-plugin

2、在webpack.config.js中配置html-webpack-plugin插件

......
const htmlWebpackPlugin = require('html-webpack')

module.exports = {
  ......
  plugins: [
    new htmlWebpackPlugin({
      template: './index.html'
    })
  ]
  ......
}
```

## 方法二：在入口文件中引入html文件，并使用raw-loader对html文件进行处理，实现html热更新

> 在入口文件中引入html文件是为了让html与入口文件产生依赖，这样在webpack运行时可以打包处理html文件。

> （可以在html-hot-reload项目的中demo3中分别修改html、js、css文件查看效果）

```
1、安装raw-loader

npm install --save-dev raw-loader

2、在webpack.config.js中配置raw-loader

module.exports = {
  ......
  module: {
    rules: [
      ......
      {
         test: /\.(htm|html)$/,
         use: [
           'raw-loader'
         ]
      }
      ......
    ]
  }
  ......
}

3、在入口文件index.js文件中引入index.html文件

import '../index.html'
```

&ensp;&ensp;综上所述，利用以上两种方法，我们可以在webpack搭建的环境中实现对html文件的热更新。可以参考html-hot-reload项目中的代码进行学习理解。

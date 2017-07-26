# webpack chap01-概述
本文的主要代码大多来自[webpack2.2中文文档](http://www.css88.com/doc/webpack2/)，对有需要了解更多的讯息可以前往阅读。
``` javascript
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
webpack的配置最基础的在于通过这四大基础核心就能实现一个简单的环境。
#### 入口（entry）
入口是作为文件的输入的起点，从引入的地址开始去检索整个项目过程中相互引用的模块，就如webpack自己所描述的，所有皆模块。
#### 出口（output）
将入口的起始文件开始，将所有模块打包后需要告诉webpack打包的文件存放点
#### 加载器（loader）
loader主要借由相对应的loader将文件中的所有资源整合成模块，然后存储在对应的依赖图表中
#### 插件（plugins）
插件的存在顾名思义就是为了在项目中引用造好的轮子，从而更方便高效地对项目进行构建与开发。通过在配置文档中require引入相对应的插件，在plugins配置里实例化即可使用。
在这里使用了'html-webpack-plugin'作为webpack打包时html页面同步引用打包的bundle文件，少去页面手动替换的操作。

#### module.export
在代码的末尾中也通过将配置文件暴露出去，可以使得当前的配置参数独立化，这样在更改替换配置的过程中可以根据项目需求进行。




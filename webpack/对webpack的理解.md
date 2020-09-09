### 概念： 
webpack是一个预编译模块方案，它会分析你的项目结构将其打包成适合浏览器加载的模块。

### 打包原理：
把所有依赖打包成一个bundle.js文件，通过代码分割成单元片段并按需加载。

### 核心思想：
1. 一切皆模块，一个js，或者一个css文件也都看成一个模块，
2. 按需加载，传统的模块打包工具（module bundlers）最终将所有的模块编译生成一个庞大的bundle.js文件。
   但是在真实的app里边，“bundle.js”文件可能有10M到15M之大可能会导致应用一直处于加载中状态。
   因此Webpack使用许多特性来分割代码然后生成多个“bundle”文件，而且异步加载部分代码以实现按需加载。

### 基础配置项：
1. ```entry:{} 入口，支持多入口```
2. ```output 出口```
3. ```resolve 处理依赖模块路径的解析```
4. ```module 处理多种文件格式的loader```
5. ```plugins 除了文件格式转化由loader来处理，其他大多数由plugin来处理```
6. ```devServer 配置 webpack-dev-server```
7. ```搭配package.json配置环境变量，以及脚本配置。```

```
"scripts": {
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development"
}

"scripts": {
    "build_": "NODE_ENV=production webpack",
    "start_": "NODE_ENV=development webpack-dev-server"
}
```
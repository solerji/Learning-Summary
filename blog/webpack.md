1.初始化包管理器 

```
npm init -y
```

2.安装webpack 

```
cnpm i webpack -D 
```

3.新建webpack配置文件（先行配置dev模式测试） 

```
const path = require('path') 

......

module.exports = { 
   mode: 'development', 
   entry: './index.js', 
   output: { 
       path: path.resolve(__dirname, './dist'), 
       filename: 'index.js' 
   } 
} 
```

4.安装处理css.html,图片资源的loader  

```
module: {         
  rules: [             
  {                
    test: /\.(png|jpg|jpeg|gif) $/,  
    use: {                    
        loader: 'url-loader' 
         }            
   }, 
   {                
     test: /\.css $/, 
     use: ['css-loader', 'style-loader', 'postcss-loader'] 
    }
  ]    
} 
```

style-loader: '使用<style>将css-loader内部样式注入到html界面。 

css-loader： 加载css文件。 

postcss-loader：用于把css解析成js可以操作的AST，并得到最终结果。这里需要使用一个叫 autoprefixer的插件对css编译，我们要创建一个**postcss.config.js**的文件。 

```
module.exports = { 
   plugins: [require('autoprefixer')] 
} 
```

url loader（ 内置file loader）：用于处理引入图片资源的url，可将文件处理为DataURL。 

sass-loader:配置scss语法，支持@import,if{}else{}语法等等 

```
 rules: [ 
           ...... 
           { 
               test: /\.scss $/, 
               use: ['css-loader', 'sass-loader', 'style-loader', 'postcss-loader'] 
           } 
       ] 
```

5.配置 HtmlWebpackPlugins 

安装 html-webpack-plugin，引入该插件，该插件的用途是解析html文件 

```
const HtmlWebpackPlugins = require('html-webpack-plugin') 

......
plugins: [ 
     new HtmlWebpackPlugins({ 
         template: './index.html', 
         title: 'react' 
     }) 
   ] 
```

6.配置es6语法适配 

安装babel（babel-loader）（共三个插件@babel/preset-env，@babel/polyfill，@babel/core） 

babel默认只转换新的javaseript句法，不转换新的API， 转译新的 API，需要借助polyfill方案去解决（ 使用@babel/polyfill或@babel/plugin-transform-runtime，二选一即可）； @babel/preset-env这个插件，简单理解字面意思就是"预设"，那到底是啥意思呢，大致意思就是官方帮我们准备的好的插件集合(包含@bable/preset-es2015、@bable/preset-es2016、@bable/preset-es2017)，@babel/core核心包用于转译。 

```
rules: [ 
             ......   
           { 
               test: /\.js $/, 
                // 排除不需要编译的目录提高编译效率 
               exclude: /node_modules/, 
               use: { 
                   loader: 'babel-loader' 
               } 
           }, 
       ] 
```

规则配置好之后要配置插件的一些属性值，创建**.babelrc**文件，配置该位置主要根据项目需要设置@babel/preset-env规则，具体可参考[官方文档配置](https://babeljs.io/docs/en/babel-preset-env)。

7.配置其他服务 

（1） CleanWebpackPlugin（打包之前删除上次的打包结果） 

```
const { CleanWebpackPlugin } = require('clean-webpack-plugin') 


plugins: [ 
     ...... 
      new CleanWebpackPlugin() 
   ] 
```

（2）热重启（代码更改后，进行保存之后可以立即显示修改之后的代码）依赖于webpack模块,需更改配置开关生效 

```
const webpack = require('webpack') 


plugins: [ 
......
new webpack.HotModuleReplacementPlugin() 
], 
devServer: { 
        contentBase: './dist', 
        open: true, 
        port: '8081', 
        hot: true, 
        hotOnly: true 
    }, 
```

（3）dev-tool模式 (选择的tool模式可以参考[这里](https://www.webpackjs.com/configuration/devtool/))

```
mode: 'development', 


devtool: 'cheap-module-eval-source-map’, 

———————————————————— 
mode: 'production', 


devtool: 'cheap-module-source-map' 
```

8.分开配置dev和production环境 

需安装组件webpack-dev-server， webpack-merge 

Dev环境下会有devserver的设置，这个在生产环境中不会用到。

```
//设置环境变量（commonConfig, prodConfig,devConfig分别是分开的dev,production以及公用设置） 
module.exports = env => { 

if (env && env.production) { 

  return merge(commonConfig, prodConfig) 


} 
if (env && env.development) { 

  return merge(commonConfig, devConfig) 

 } 
} 
```

完整可用配置可在[这里](https://github.com/solerji/vueConfigDemo)寻找。明天还会配置一个适合多页应用的打包环境试试看。 
## plugin
在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情
> #### 配置plugins

> ###### 生成创建html入口文件（html-webpack-plugin）

```
安装
npm i html-webpack-plugin -D
```
> ###### 清除文件（clean-webpack-plugin）

先把之前带hash值没用的文件删除掉清空，然后再自动产出文件
```
安装
npm i clean-webpack-plugin -D
```


```
//引入path路径包和html-webpack-plugin包
const path = require('path');
const HWP = require('html-webpack-plugin');
const cwp = require('clean-webpack-plugin');
module.exports = {
    mode:'development',//配置开发依赖（file本地的）
    //入口文件，目的是为了解析某些指定文件，最终编译成能够在浏览器运行的文件
    entry:{
        cc:'./c.js',
        ee:'./e.js'
    },
    //出口文件，取个名字放在build文件夹下面
    output:{
        path:path.resolve(__dirname,'build'),//最终生产出的自定义文件夹名build
        filename:'[name].js'//指定的文件夹名字
    },
    //plugins可以自动生产出html
    plugins:[
        new cwp(['build']), 
        new HWP({//new 一下HWP
           template:'./webpack.html',//本地的模板文件
           chunks:['cc','ee'],//在产出的HTML文件里引入哪些模块
           filename:'index.html',//编译后的html文件
           hash:true,//文件名称是否哈希 ?xxxx.js
           //给模板设置的变量名，html中调用 htmlWebpackPlugin.options.title来使用
           title:'base',
           minify:{
            removeAttributeQuotes:true,//去掉属性的引号
            collapseWhitespace:true //html压缩一行
           }
        })
    ]
}
```
> ###### 拷贝静态文件
将指定目录的文件赋值到指定目录下

```
安装
npm i copy-webpack-plugin -D
```

```
const cowp = require('copy-webpack-plugin'); 
plugins:[
    new cowp([{
        from:path.join(__dirname,'build'),//从哪里复制
        to:path.join(__dirname,'dist','build')//复制到哪里
    }])
]
```



> ###### 热更新应用(HotModuleReplacementPlugin)
`webpack自带插件，所以不用安装`

热更新是webpack的一个特性，通过无刷新实现代码更新

`热更新属于开发模式，上线后就不用了`
```
1、引入webpack包
    const wp = require('webpack');
2、devServer下设置hot 为true
3、在plugins下 执行new webpack.HotModuleReplacementPlugin()
4、在入口文件中写入
    if(module.hot){
        module.hot.accept()
    }
```

```

```

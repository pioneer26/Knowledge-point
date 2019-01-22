## webpack


webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)
```
英文指令比较多。学习webpack靠记忆。
多看英文报错，多尝试
```
`webpack可以看做是一个模块打包机，他做的事情是分析你的项目结构，找到js模块或其他浏览器不能直接运行的拓展语言，将其打包成适合的格式以供浏览器使用。`

特性：
```
1、代码转换：ts编译成js，把sass编译成css
2、文件优化：压缩js,css,html代码，压缩合并图片等
3、代码分隔：提取多页面的公共代码，提取首屏不需要执行部分的代码让其异步加载
4、模块合并：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件
5、自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器
6、代码校验：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过
7、自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统
```
> #### 核心概念
- Entry[因吹] 入口

```
Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入
```
- Module 模块

```
在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块
```
- Chunk代码块

```
一个 Chunk 由多个模块组合而成，用于代码合并与分割
```
- Loader 模块转换器

``` 扩展插件
用于把模块原内容按照需求转换成新内容
```
- Plugin 扩展插件

```
在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情
```
- Output 输出结果

```
在 Webpack 经过一系列处理并得出最终想要的代码后输出结果
```
> ###### 工作流程


```
1、Webpack 启动后会从Entry里配置的Module开始递归解析 Entry 依赖的所有 Module
2、每找到一个 Module， 就会根据配置的Loader去找出对应的转换规则
   对 Module 进行转换后，再解析出当前 Module 依赖的 Module
3、这些模块会以 Entry 为单位进行分组，一个 Entry 和其所有依赖的 Module 被分到一个组也就是一个 Chunk
4、最后 Webpack 会把所有 Chunk 转换成文件输出。 
   在整个流程中 Webpack 会在恰当的时机执行 Plugin 里定义的逻辑
```

> #### 配置webpack

1、初始化项目
```
npm init - y

在要进行打包的目录下初始化npm, 在控制台执行以上命令后会生成一个package.json的文件
```

```
package.json里面可以手动改webpack版本,改完之后npm i 重新生成下
```

```
在"build":"webpack -w"
        -w watch  监听
```

2、安装webpack（开发依赖）

```
全局安装：
    npm i webpack -g (由于版本更新问题，不建议全局安装)
建议本地安装：
    npm i webpack -D
    npm i webpack webpack-cli -D
    或npm install webpack --save-dev

开发依赖：有些配置开发时候使用，到了线上就不适用了
生产依赖：打包后上线需要的依赖。
        npm i xx --save
        -S --> save
        -D --> save-dev
```

```
** npm install webpack webpack-cli -D
从4.0开始，webpack拆分开两个包分别是webpack和webpack-cli
```

```
批量安装需要的插件、loader等命令

npm i webpack webpack-cli css-loader clean-webpack-plugin html-webpack-plugin mini-css-extract-plugin url-loader file-loader optimize-css-assets-webpack-plugin html-withimg-loader  -D

```

3、手动建立（配置）webpack.config.js文件

```
const path = require('path'); // 引包
const hwp = require('html-webpack-plugin');//自动产出html
const cwp = require('clean-webpack-plugin');//清除文件
const mcep = require('mini-css-extract-plugin');//css分离包
const ocawp = require('optimize-css-assets-webpack-plugin');//css压缩
const uwp = require('uglifyjs-webpack-plugin');//js压缩
module.exports = {
    mode:'development', // 4.0必须配mode =>development开发依赖 production生产依赖
    //入口文件的地址，解析某个指定的文件，
    entry: './c.js', //编译成浏览器可以使用的文件 可以是字符串、数组、对象
    //出口文件
    output: {
        path:path.resolve(__dirname, 'build'), // 当前目录创建文件夹 build是自己起的名字 dirname绝对路径
        filename: '[name].js', // 可以放固定名字，key值等于什么就写什么
        publicPath:'/'//输出解析文件的目录 比如解析图片url路径时候用
    },
    /*配置完入口出口文件，其实就可以打包了,只不过没有其他东西，比如插件什么的*/
    module:{
        rules:[//rules是个数组，里面包对象
            {
                 /*在根目录下找.css结尾的文件，把他们打包出来 */
                test:/\.css$/,//匹配所有以.css结尾的
                use:[
                    //使用css分离就用mini-css-extract-plugin
                    {
                        loader.mcep
                    },
                    'css-loader'//使用css-loader
                ]
            }，
            //处理css中图片
            {
                test:/\(jpg|png|gif|svg|ttf|eot|woff|bmp)$/,
                use:[
                    {
                        loader:'url-loader',
                        options:{
                            limit:4096,//图片大小
                            outputPath:'../images',//指定打包后的图片位置
                            publicPath:'/images'//原来的图片位置
                        }
                    }
                ]
            },
            //使用插件
            plugins:[
                new ocawp({}),//压缩css
                new cwp(['build']),//删除多余的js文件
                new hwp({
                    template:'./index.html',
                    filename:'../index.html',
                    minify:{
                        collapseWhitespace:true,//压缩成一行
                        removeAttributeQuotes:true,//去属性双引号
                    }
                })
            ]
        ]
    }
}
```


4、package.json文件script中定义打包后的文件夹名称

```
"script":{
    "build":"webpack"
}
```
5、npm run build 打包
```
最后输入npm run build命令进行打包

打包之后会产出一个build文件夹
```



> #### 配置开发服务器
只要在开发中，都要使用devserver
```
输入npm install webpack-dev-server -D
```
contentBase：配置开发服务运行时的文件根目录

host：开发服务器监听的主机地址

compress：开发服务器是否启动gzip等压缩

port：开发服务器监听的端口



配置参数

```
//放在plugins下面
devServer:{
    //配置开发服务运行时的文件根目录
    contentBase:path.resolve(__dirname,'build'),
    //开发服务器监听的主机地址
    host:'localhost',
    //开发服务器是否启动gzip等压缩
    compress:true,
    //开发服务器监听的端口号
    port:80
}
```
package.json文件里面配置启动参数

```
"scripts":{
    "dev":"webpack-dev-server"
}
```
运行服务器环境打包命令

```
npm run dev
```
卸载安装的插件

```
npm uninstall xxx
```
> ###### npm run build与npm run dev

```
npm run build：上线用的命令，生成build文件夹，打包上线
npm run dev：生产用的命令，生成临时build文件夹。运行完后删了在服务端运行
```
devDepencies

```
npm i -D
只用于开发环境，不用于生产环境
```
Depencies

```
npm i -S
用于生产环境
```
> ###### npm i 与npm install的区别

```
1、用npm i 安装的模块无法用npm uninstall卸载，需要用npm uninstall i命令
2、npm i 会帮助检测与当前node版本最匹配的npm包 版本号
   并匹配出来相互依赖的npm包应该提升的版本号
3、部分npm包在当前node版本下无法使用，必须使用建议版本
4、安装报错时intall肯定会出现npm-debug.log 文件，npm i不一定

```



```
***手写webpack4.0

const path = require('path');//引入路径包
const HWP = require('html-webpack-plugin');//引入自动产出html包
const CWP = require('clean-webpack-plugin');//引入清除文件包
const webpack = require('webpack');//引入webpack，做热更新用
//const etwp = require('extract-text-webpack-plugin');
const mcep = require('mini-css-extract-plugin');//引入分离css包
const copywp = require('copy-webpack-plugin');//引入拷贝插件

let obj = {
    /*
    webpack4.0需要配置依赖:
        开发依赖->mode:'development'
        生产依赖->mode:'production'
    */
    mode:'development',//配置开发依赖
    //入口文件
    entry:{
        './index.html'//目的为了解析某些指定文件，最终编译成能在浏览器运行的文件
    },
    //出口文件（取个名字放在build文件夹下面）
    output:{
        path:path.resolve(__dirname,'build'),//指定打包后的文件夹
        filename:'[name].[hash:6].js'//给指定的文件名字加上6位哈希
    },
    //利用loader模块转换器分离css
    module:{
        rules:[
           {
                 /*在根目录下找.css结尾的文件，把他们打包出来 */
                test:/\.css$/,//匹配所有以.css结尾的
                use:[
                    //使用css分离就用mini-css-extract-plugin
                    {
                        loader.mcep
                    },
                    'css-loader'//使用css-loader
                ]
            }，
            //处理css中图片
            {
                test:/\(jpg|png|gif|svg|ttf|eot|woff|bmp)$/,
                use:[
                    {
                        loader:'url-loader',
                        options:{
                            limit:4096,//图片大小
                            outputPath:'../images',//指定打包后的图片位置
                            publicPath:'/images'//原来的图片位置
                        }
                    }
                ]
            }
            
        ]
    }
    //引入扩展插件
    plugins:[
        //清除文件插件  
        new CWP({
            ['build']//清除打包后多余的js（必须放在html文件上面）
        }),
         //分离css插件  
        new mcep({
            filename:'1.css'
        }),
         //自动产出html插件  
        new HWP({
            template:'./index.html'//模板文件,
            
            //对文件进行压缩
            minify:{
                removeAttributeQuotes:true,//去掉属性双引号
                collapseWhitespace:true//将文件压缩成一行
            },
            /*
                设置文件的title
                用模板引擎配合使用,<%= htmlWebpackPlugin.options.title>
            */
            title:'欢迎来到webpack4.0',
            hash:true,//给产出的文件加上hash，避免缓存
            //提取代码中的公共模块，然后将其打包到独立的文件中
            chunks:['index','index1']
            
        }),
        //热更新插件
        new webpack.HotModuleReplacementPlugin()//热更新模块
    ],
    //配置开发服务器
    devServer:{
        //配置开发服务运行时文件根目录（该配置可以省略）
        contentBase:path.resolve(__dirname,'build'),
        host:'localhost', //服务器监听的主机地址
        compress:true, //服务器是否启动压缩
        port:80, //服务器监听的端口
        open:true, //自动打开浏览器 可不写
        hot:true//热更新，局部刷新
    }
    
}
module.exports = obj;//导出obj

*** extract-text-webpack-plugin默认安装的是3.0.2不支持webpack4.0
    所以使用mini-css-extract-plugin 支持webpack4.0
```

> #### npx
`node v5.2.0以上才能使用npx`


npx为了提升开发者使用包内提供的命令行工具的体验

```
npx webpack 直接运行webpack, 如果没有会直接下载一个 这是个临时包 关闭时自动删除
```


```
实现0配置，不用再json中配置script就能运行webpack
临时安装一个包,用完删除
```



特点：
```
1、临时安装可执行依赖包，不用全局安装，不用担心长期的污染
2、可以执行依赖包中的命令，安装完成自动运行
3、自动加载node_modules中依赖包，不用指定path路径
4、可以指定node版本、命令的版本，解决了不同项目使用不同版本命令的问题
```

```
打包上线 npm run build
开发模式 npx webpack
```



> #### 开发环境和生产环境
webpack4.0需要配置依赖：

- 开发环境

开发依赖->mode:'development'

```
development 
开发模式，打包默认不压缩代码，默认开启代码调试
```

```
安装包的时候使用npm i xx -D
安装完之后会在package.json中的devDependencies
使用的一些构建工具比如glup、webpack只是在开发中使用的包，
上线之后就和他们无关了，所以将它写入devDependencies
```

生产依赖->mode:'production'

```
production 
生产模式，上线时使用，打包压缩代码，不开启代码调试
```

```
安装包使用npm i xx -S
安装完之后会在package.json中的dependencies
比如我们写一个项目要依赖于jQuery，没有这个包的依赖运行就会报错，
这时候就把这个依赖写入dependencies
```


> ###### export 与 export default区别
`注意: 当一个文件既有export defalut 又有export 时,导入顺序有区别`
```
export引入时候要加花括号，并且名字必须与导出的一致，他们可以多个
```

```
export default下一个文件只能有一个（default），并且引入时候不用加{}，名字可以自定义
```

```
a.js  b.js
b.js里面加上 export default a;
b文件加了default之后，a文件引入 import后面名字可以自定义写

b.js里面多次导出export时候
```


es6中export导出的几种方式

```
1、export let a = 20
2、export {}
    1和2引入时候都要加{}，且名字必须和导出的一致，可以有多个export
3、export default
    default只能有一个
```

卸载命令

```
npm uninstall jquery
```


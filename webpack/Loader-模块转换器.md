## Loader
`模块转换器，用于把模块原内容按照需求转换成新内容`

通过使用不同的Loader，Webpack可以要把不同的文件都转成JS文件,比如CSS、ES6/7、JSX等

- test

```
匹配处理文件扩展的正则表达式
```
- use

```
loader名称
```
- include/exclude

```
手动指定必须处理的文件夹和屏蔽不需要处理的文件夹
```
- query

```
为loaders提供额外的设置选项
```
Loader的三种写法：

1. use
2. loader
3. use+loader

> ###### css-loader
安装

```
npm i style-loader css-loader -D
```
配置加载器

```
module:{
    rules:[
        {
            test:/\.css$/,     
            use:['style-loader','css-loader']//从右往左写，webpack特性
        }
        {
            test:/\.css$/,
            fallback:"style-loader",
            use:"css-loader",
            disable:true
        }
        
        /*
            注意先放style-loader再放css-loader
            上面的两个是执行了样式但css文件看不到
            下面的这个是可以看到css，并且有这个文件，将css分离了出来
        */
        {
            test:/\.css$/,
            use:[
                {loader:mcep.loader},
                "css-loader"
            ]
        }
    ]
}
```
> ###### 手动添加图片
命令

```
npm i file-loader url-loader -D
```
file-loader 解决CSS等文件中的引入图片路径问题

url-loader 当图片较小的时候会把图片BASE64编码，大于limit参数的时候还是使用file-loader 进行拷贝

```
let logo = require('./images/logo.png');
let img = new Iamge();
img.src= logo;
document.body.appendChild(imig);
{
    test:'/\.(jpg|png|gif|svg)/',
    use:'url-loader',
    include:path.join(__dirname,'./scr'),
    exclude:/node_modules/
}
```




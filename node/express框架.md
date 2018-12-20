`Express是一个简洁而灵活的 node.js Web应用框架。`

`Express是一个自身功能极简，完全是路由和中间件构成一个web开发框架`

Express 框架核心特性:

```
1、可以设置中间件来响应HTTP请求
2、定义了路由表用于执行不同的 HTTP 请求动作
3、可以通过向模板传递参数来动态渲染 HTML 页面
```
举个例子

node.js实现一个控制台打印“hello world”

```
let http = require('http');
let server = http.createServer((req,res)=>{
    console.log('hello world');
});
server.listen(3000);

当我们需要处理各种请求（主要指GET、POST）时，
我们需要将所有请求类型处理的代码写在createServer包裹的函数里
```
Express实现一个控制台打印“hello world”

```
let express = require('express');
let app = express();
http.createServer(app);
//处理用户请求
app.get('/',()=>{
    console.log('hello world');
})

express处理各种请求是通过Express执行函数去调用对应的方法，这样更方便和快捷
```

> ###### 安装express
全局安装

```
全局安装会自动将文件安装在 C:\Users\Administrator\AppData\Roaming\npm
（可以通过命令来修改）
```

```
1、cmd 输入npm install -g express-generator
（因为Express4.x的版本中，命令工具被分离出来，所以我们需要先安装一个命令工具否则输入第三步的命令时会报错）
2、npm install -g express
3、express --version 回车
如果出现了 Express 的版本号，说明 Express 安装成功
```

本地安装（即只安装在当前项目中）

```
一个项目安装一个 Express 框架对于新手来说比较好理解，推荐本地安装。
然后在程序中通过 require() 来使用
```

```
1、nmp init 项目初始化
    ** name属性名字不要取某些工具的名字
    初始化之后会出现一个package.json的文件
2、安装 Express 并将其保存到依赖列表中
    npm i express -save
    或者 npm i express -S
3、npm install body-parser cookie-parser express multer --save
需要与express一起安装的3个模块
    body-parser- node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据
    cookie-parser一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象
    multer- node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据
4、引入express模块
    const express = require('express');
    const app = express()
```
> ###### package.json文件介绍
package.json 是每个 Node.js 项目中必不可少的一个核心文件

`简单来说，pageage.json就是一个管理你通过npm安装到本地包的文件`

作用

```
1、显示了你的项目所依赖的各种包
2、显示你的项目的一些配置信息，如项目名称、版本号等
3、方便与其他开发者共享
```
内容

```
{
  "name": "ex",//名字-express
  "version": "1.0.0",//版本1.0.0
  "description": "",
  "main": "app.js",//
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "body-parser": "^1.18.3",
    "cookie-parser": "^1.4.3",
    "express": "^4.16.4",
    "multer": "^1.4.1"
  }
}
```

对于 package.json 文件的创建，注意以下几点

```
1、创建的时间：
    在项目刚开始时就应该创建
2、创建的位置
    项目的根目录
3、创建方法：
    使用命令行的方式创建
    （进入项目所在目录之后，输入 npm init 命令，接着使用默认值一路按回车
    在项目根目录下自动生成了一个名为 package.json 的文件）
    也可以手动创建，然后将相应的信息手动写入文件即可
```

```
如果想一步生成这个文件，可以输入命令 npm init -yes，然后回车即可
```
> ###### 理解中间件
express完全是路由和中间件构成一个web开发框架

从本质上来说，一个Express应用就是在调用各种中间件

那么什么是中间件？

```
大白话：

```


比如app.use（[path]，function）

```
path：是路由的url，默认参数‘/'，意义是路由到这个路径时使用这个中间件
function：中间件函数
这个中间件函数可以理解为就是function(request,response,next)
```


> ###### 静态文件

```
express提供了内置的中间件express.static来设置静态文件
比如css javascript等
```
通过use给app注入一个 管理静态文件的新功能

```
// 给回调函数传入一个next方法，通过下边的next()方法告知本函数执行完在执行下一个函数

app.use(express.static('www'),(req,res,next)=>{
    res.write(fs.readFileSycn('./www/404.html'));
    res.end();
    next();//执行下一个函数
})
```
整体流程栗子

```
const express = require('express');//引入express
const app = express();
const fs = require('fs');//引入fs文件系统模块
//引入body-Parser和multer中间件
const bodyParser = require('body-Parser');
const multer = require('multer');
app.use(express.static('www'));//静态资源管理
/*
    当bodyParser.urlencoded（解析文本格式）中 extended
    为false时，键值对中的值就为'String'或'Array'形式
    为true时候则可为任何数据类型
*/
app.use(bodyParser.urlencoded({extended:false}));
//指定配置项。这里指定文件保存于当前目录下的tmp子目录
app.use(multer({dest:'/uploads'}).array('myfile'));
app.post('/file_upload',(req,res)=>{
    //写入的路径和名字
    let des_file = _dirname +'/upload/' + req.files[0].originalname;
    try{
        let data = fs.readFileSync(req.files[0].path);
        fs.writeFileSycn(des_file,data);
        res.json({code:0,msg:'成功'});
    }catch(error){
        res.json({code:1,msg:'失败'});
    }
});

```



> ###### app.use和app.get区别

```
app.use(path,callback)中的callback既可以是router(路由)对象又可以是函数
app.get(path,callback)中的callback只能是函数
```
> ###### req获取参数的三种方法
1、req.params
```
取带冒号的参数

app.get('/user/:id', function(req, res){ 
res.send('user ' + req.params.id);
});
```
2、req.body

```
req.body一定是post请求，express依赖的中间件必须有bodyParser，不然req.body是没有的

var app = require('express')();
var bodyParser = require('body-parser');
var multer = require('multer'); 
app.use(bodyParser.json()); // for parsing application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded
app.use(multer()); // for parsing multipart/form-data
app.post('/', function (req, res) { 
  console.log(req.body); 
  res.json(req.body);
})
```
3、req.query

```
query是querystring，说明req.query不一定是get

// GET /search?q=tobi+ferret
req.query.q
// => "tobi ferret"// 
GET /shoes?order=desc&shoe[color]=blue&shoe[type]=conversereq.query.order
// => "desc"
req.query.shoe.color
// => "blue"
req.query.shoe.type
// => "converse"
```
变态的写法

```
// POST /search?q=tobi+ferret
{a:1,b:2}
req.query.q
// => "tobi ferret"
** post里看不的，用req.body取
```


> ###### Request 对象的一些常见属性
`request 对象表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性`

req.app

```
当callback为外部文件时，用req.app访问express的实例
```
req.baseUrl
```
获取路由当前安装的URL路径
```
req.path

```
获取请求路径
```
req.query

```
获取URL的查询参数串（==直接把查询信息转成对象==）
```
req.route

```
获取当前匹配的路由
```
req.accepts

```
检查可接受的请求的文档类型
```
req.get()

```
获取指定的HTTP请求头
```
req.is()

```
判断请求头Content-Type的MIME类型
```

> ###### Response 对象的一些常见属性
`response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据`

res.app（同req.app）

```
当callback为外部文件时，用res.app访问express的实例
```
res.append()

```
追加指定HTTP头
```
res.set()

```
在res.append()后将重置之前设置的头
```
res.cookie(name，value [，option])

```
设置Cookie
```
res.clearCookie()

```
清除Cookie
```
res.download()

```
传送指定路径的文件
```
res.get()

```
返回指定的HTTP头
```
res.json()

```
传送JSON响应
    之前写法：res.write(JSON.stringify(obj)) res.end()
```
res.jsonp()

```
传送JSONP响应
```
res.location()

```
只设置响应的Location HTTP头，不设置状态码或者close response
```
res.render(view,[locals],callback)

```
渲染一个view，同时向callback传递渲染后的字符串
```
res.send()
```
设置HTTP头，传入object可以一次设置多个头
```
res.status()

```
设置HTTP状态码
```
res.type()

```
设置Content-Type的MIME类型
```





## Koa
`Koa -- 基于 Node.js 平台的下一代 web 开发框架`

首先创建一个文件夹，项目初始化 

`会出现package.json文件`

```
npm init -y
```
安装koa
```
npm i koa --save
npm i koa -S
```

index.js文件
```JavaScript
const koa = require('koa');//引包
const app = new Koa();

app.use(async(ctx)=>{
    ctx.body = 'hello world';//把输出的返回给前端
})
app.listen(3000)//监听端口
```

```JavaScript
//然后输出 node index.js tab 补全回车
```

```
ctx.request//接收客户的发送的请求
ctx.request.query -> {key:val}//拿东西
ctx.request.querystring -> key=val&key=val//查询信息不包括?
ctx.body = {} //输出
```

---

#### 路由

1、安装router（才能写接口）

```
npm i koa-router -S
```
2、引包
```
const router = require('koa-router')()
//kow-static 静态文件
```
3、get方式（两种写法）

```
//第一种
router.get('/user',async(ctx)=>{
    //这里面放get请求的逻辑
})
/*
第二种，在一个单独的文件里边写get请求的逻辑,在这里需要require引一下
*/
router.get('/user',require('../xxx'))
```

4、use引用
```JavaScript
app.use(router.routes());
//app.use(static(path.join(_dirname,'www')))
```
##### 静态资源
1、安装 

```
npm i koa-static -S
```
2、引包

```
const static = require('koa-static');
const path = require('path');
```
3、使用static中间件，设置静态资源的目录


```
//路径指向 当前目录www
app.use(static(
    path.join(__dirname,'www')
)) 
```

##### post请求

```
post请求时候可以使用 bodyParser 中间件
```
安装koa-bodyparser中间件

```
npm i koa-bodyparser -S
```
引包

```
const bodyparser = require('koa-bodyparser');
```
use使用

```
app.use(bodyparser());
```

```
就可以在ctx.request.body下取数据

let req = ctx.request.body;
```
###### 事件

```JavaScript
let str = "";
//数据开始接收
cxt.req.on('data',(data)=>{
    str += data
})
//数据接收完触发
ctx.req.addListener('end',()=>{
    console.log(str);
})
```

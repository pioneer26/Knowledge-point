> ##### 概念
`可以说node是运行在服务器端V8引擎上的JavaScript`

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎。

V8引擎执行Javascript的速度非常快，性能非常好

（node不需要Apache,Naginx,IIS等Web服务器）


```
node -v 查看当前版本
```

特征：
1. 单线程

```
一个进程中只有一个线程。程序顺利执行
前面执行完才会执行后面的程序
优势：为了提高服务器性能，单线程减少内存开销
```

2. 事件驱动

```
某个条件完成之后执行
不管是新用户的请求，还是老用户的I/O完成，都将以事件方式加入事件环，等待调度
```

3. 非阻塞I/O

```
I/O->input/output，代表输入输出
单线程执行任务时，不堵塞通道（无须排队），这个线程是一直在工作的
```

```
阻塞代码

const fs = require("fs");
let data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("程序执行结束");

非阻塞代码

const fs = require("fs");
fs.readFile('input.txt',(error,data)=>{
    if(error) return console.log(error);
   console.log(data.toString()); 
});
console.log("程序执行结束");
```

node与php、jsp等其他服务器端对比

```
1、node 无法直接渲染静态页面，提供静态服务
2、node 也没有根目录的概念
3、node 必须通过路由程序指定文件才能渲染文件
4、node 比其他服务端性能更好，速度更快
```
node的应用场景

```
用户表单收集
考试系统
聊天室
web论坛
图文直播
应用程序需要处理大量并发的I/O项目
```

```
总而言之，NodeJS适合运用在高并发、I/O密集、少量业务逻辑的场景。
```

> ###### 创建第一个应用
nodejs提供了http模块，自身就可以用来构建服务器

引入http模块（就是到默认文件夹找一个叫 http.js 的文件）

```
http模块中封装了一个HTPP服务器和一个简易的HTTP客户端
比如打开浏览器，输入localhost:80我们就可以看到 hello node 
```

```
let http = require('http');
```

```
http没有路径，是系统默认自带的。 如果自定义的js文件就需要写路径
```

require 引入

```
require（指定的文件）其中js文件可以不写.js
比如直接require('http');
```
module.exports 导出

```
module.exports = {
    a:10,
    fn:function(){
        console.log(1);
    },
    arr:[1,2,3,4]
};
```
> ###### 创建服务器

```
 使用 http.createServer 方法创建服务器
```
`request 接收客户端（浏览器）发送的信息`

`response 给客户端（浏览器）发送信息`

```
发送给前端

response.write()
response.end()

*** 写了write就要写end
```

```
listen(端口名)
监听端口
```

```
    let http = require('http');
    http.createServer((request,response)=>{
        // console.log(request);
        let name = request.url.split('=')[1];
        switch(name){
            case 'xyz':
                response.write('{code:0,msg:chy}');//服务端提示信息
            break;
            case 'hongdan':
                response.write('{code:0,msg:cmy}');
            break;
            default:
                response.write('{code:1,msg:cyy}');
            break;
        }
        response.end();//写了write()就要写end()
    }).listen(80);//监听端口 （默认80可以省略）
```
writeHead()方法

向请求的客户端发送响应头

```
该格式可以识别HTML结构，编码格式是UTF-8
*** 该函数在一个请求内最多只能调用一次，如果不调用，则会自动生成一个响应头
```

```
res.writeHead(200,{'Content-type':'text/html;charset=utf-8'});
```

> #### fs文件系统模块
所有与文件操作都是通过fs核心模块实现的。包括文件的创建、删除、查询以及读写和写入。

`在 fs 模块中，所有的方法都分为同步和异步两种实现`

```
具有 sync 后缀的方法为同步方法，不具有 sync 后缀的方法为异步方法
```
> ###### 文件操作-文件读取
同步文件读取readFileSync

```
第一个参数为读取文件的路径或文件描述符；
第二个参数为 options，默认值为 null
```

```
const fs = require('fs');
fs.readFile('./www/1.txt',(error,data)=>{
   if(error){
    console.log('404');
  }else{
    console.log(data.toString());
  }
});
//readFileSycn只有成功状态，找不到就报错
try {
    let data = fs.readFileSync('./www/1.txt');
    console.log(data.toString());
} catch (error) {
    
}
console.log(11111);
```

异步文件读取readFile

```
第一个参数为读取文件的路径或文件描述符；
第二个参数为 options，默认值为 null
第三个参数为callback回调函数
    //函数内有两个参数 err（错误）和 data（数据）
    //该方法没有返回值，回调函数在读取文件成功后执行
```

```
const fs = require('fs');
fs.readFile('./www/1.txt',(error,data)=>{
    if(error){
        console.log('404');
    }else{
    }
});
```

> ###### 文件操作-文件写入
同步文件写入writeFileSync

```
第一个参数为写入文件的路径或文件描述符
第二个参数为写入的数据，类型为 String 或 Buffer
第三个参数为 options，默认值为 null
```

```
const fs = require("fs");
fs.writeFileSync("2.txt", "Hello world");
let data = fs.readFileSync("2.txt", "utf8");
console.log(data);
```

异步文件写入writeFile

```
第一个参数为读取文件的路径或文件描述符；
第二个参数为 options，默认值为 null
```

```
let fs = require('fs');

fs.writeFile('./www/2.txt','dsnadkjsjdksa',(error)=>{
    if(error){
         console.log('失败');
    }else{
         console.log('成功');
    }
 })
```
> ###### 文件操作-文件删除
同步文件删除unlinkSync

```
const fs = require("fs");
fs.unlinkSync("a/inde.js");
```

异步文件删除unlink

```
const fs = require("fs");
fs.unlink("a/index.js", err => {
    if (!err) console.log("删除成功");
});

```
> #### querystring模块
`用于解析与格式化 URL 的查询字符串`
比如 字符串转对象
```
a=b&c=d -> {a:b,c:d}
```
querystring模块提供了四种方法
1. querystring.parse(str,separator,eq,options)

```
parse这个方法是将一个字符串反序列化为一个对象
```

```
参数：str指需要反序列化的字符串;
　　　separator（可省）指用于分割str这个字符串的字符或字符串，默认值为"&";
　　　eq（可省）指用于划分键和值的字符或字符串，默认值为"=";
　　　options（可省）该参数是一个对象，里面可设置maxKeys和decodeURIComponent这两个属性：
　　　　　　maxKeys：传入一个number类型，指定解析键值对的最大值，默认值为1000，
　　　　　　如果设置为0时，则取消解析的数量限制
```

```
querystring.parse("name=whitemu&sex=man&sex=women");
 /*
 return:
 { name: 'whitemu', sex: [ 'man', 'women' ] }
 */
 querystring.parse("name=whitemu#sex=man#sex=women","#",null,{maxKeys:2});
 /*
 return:
 { name: 'whitemu', sex: 'man' }
 */
```
2.querystring.stringify(obj,separator,eq,options)

```
stringify 这个方法是将一个对象序列化成一个字符串，与querystring.parse相对
```

```
querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] });
/*
return:
'name=whitemu&sex=man&sex=women'
*/
querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] },"*","$");
/*
return:
'name$whitemu*sex$man*sex$women'
*/
```
3. querystring.escape(str)

```
escape可使传入的字符串进行编码
```

4. querystring.unescape(str)

```
unescape方法可将含有%的字符串进行解码
```


判断URL是接口还是请求静态资源（页面）栗子

```
思路：
如果url里面包含？就说明请求的是接口
否则是静态文件。需要拼地址
```

```
let http = require('http');//引入http模块用来构建服务器
let fs = require('fs');//需要读写删等操作时候就引入fs文件系统模块
http.createServer((req,res)=>{
    //判断是请求接口还是请求静态页面
    let url = req.url;
    if(url.includes('?')){//如果url包含？代表请求的是接口
        //向客户端发送头信息（防止中文乱码）
        res.writeHead(200,{'Content-Type':'text/html;charset=utf-8'});
        res.write('每天进步一点点');
        res.end();
    }else{//否则请求的是静态页面
    // 斜杠代表 localhost
     //默认出现index文件
        if(url === '/') {url = '/inex.html';}
            //读入文件
            fs.readFile('./www'+url,(err,data)=>{
              if(err){
                  //如果没有文件，就读取404页面
                  let data = fs.readFileSync('./www./404.html');
                  res.write(data);
              }else{
                  //如果有文件就把文件发给前台
                  res.write(data);
              }
              res.end();//写入结束
            });
    }
}).listen(80);//监听端口
```
try catch解决删除unlinkSync栗子

```
const http = require('http');
const fs = require('fs');
/*
    querystring可以把字符串转成对象
    比如a=b&c=d -> {a:b,c:d}
*/
const qs = require('querystring');
http.createServer((req,res)=>{//创建服务器
    res.writeHead(200,{'Content-type':'text/html;charset=utf-8'});
    let url = req.url;
    if(url.includes('?')){
        let obj = qs.parse(url.split('?')[1]);//用 ? 把url分隔开
        if(obj.rm){
            try{
                fs.unlinkSync(ojb.rm);
                res.write('{"code":0,"msg":"删除成功"}');
            }catch(err){
                //找不到某个文件就会进入catch。里面放报错的信息
               res.write('{"code":1,"msg":"删除失败"}');
            }
            red.end();
        }
    }else{
        if(url === '/'){
            url = '/index.html';
        }    
        fs.readFile('./www'+url,(error,data)=>{
            if(error){
                res.write(fs.readFileSycn('./www/404.html'));
            }else{
                res.write(data);
            }
            res.end();
        })
    }
}).listen(80);
```
node — post请求方式栗子

```
html部分

    用户名:  <input type="text" id="user">
    密码:<input type="password" id="pw">
    <button id="btn">登录</button>
    <div id="tan">不存在用户</div>
```

```
btn.onclick = function(){
    let uv = user.value.trim();
    let pv = pw.value.trim();
    //判断如果输入了用户名和密码再进行下面的操作
    if(uv && pv){
        fetch('/login',{
            body:new URLSearchParams({
                user:uv,
                pw:pv
            }).toString();
            method:'post',
            headers:{
                'Content-type':'application/x-www-form-urlencoded'
            }
        })
        .then(e=>e.json())
        .then(data =>{
            if(data.code == 1){
                tanFn(data.msg);
            }
            if(data.code === 0){
                tanFn(data.msg,()=>{
                    window.location.href = 'http://www.baidu.com';
                });
            }
            if(data.code === 2){
                tanFn(data.msg);
            }
        })
    }
}

 function tanFn(txt,cb){
        tan.innerHTML = txt;
        tan.style.top = '30%';
        setTimeout(()=>{
            tan.style.top = '-40px';
            setTimeout(function(){
                cb && cb();
            },500);
        },2000);
    }
```

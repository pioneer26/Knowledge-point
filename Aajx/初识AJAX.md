> ## AJAX
全称：ajax：Asynchronous  javascript  and  xml
- 异步的js和XML
- 前后端的数据交互
- 前端向后端请求数据的一种手段
- 局部刷新

`难点：1、如何拿数据（掌握字段，服务器要什么客户的就传什么）
2、获取数据后如果操作数据`


```
访问新的数据
    传统的：刷新页面对客户体验不好，增加服务器压力
    ajax：  做到页面无刷新
```

优点：
1. 无刷新更新数据
（在不刷新整个页面的情况下维持与服务器通信）
2. 异步与服务器通信
（使用异步的方式与服务器通信，不打断用户的操作）
3. 前端与后端负载均衡（将一些后端的工作移到前端，减少服务器与带宽的负担） 
4. 界面与应用分离（Ajax使得界面与应用分离，也就是数据与呈现分离）

缺点；
1. 会破坏Back与History后退按钮的正常行为等浏览器机制。
2. 安全问题（ajax暴露了与服务器交互的细节）
3. 对搜索引擎支持比较弱
4. 破坏程序的异常处理机制


> ###### 传统模式的交互方式
IE下有ActiveXObject插件，new ActiveXObject('Mscrosoft.XMLHttp');
```
get方式

<form action="/get" method="GET">
        用户名：<input type="text" name="user">
        密码：<input type="password">
        <input type="submit" value="提交">
    </form>
```

```
post方式

 <form action="/post" method="POST" enctype="application/x-www-form-urlencoded" >
        用户名:<input type="text" name="user"/>
        密 码:<input type="password" name="password" />
        <input type="submit" value="提交">
    </form>
```

> #### JQ版ajax
- url - 请求地址（后台提供）
- data - 数据的拼接 （data为object,直接使用的数据）
- dataType - JSON.parse() 把json转成对象
- code为0 代表失败

```
*** data传的值是重点和难点

$.ajax({
    url：请求地址, //后台给地址
    data:{
        user:$('#txt').val()//
        // key值：value值
        //其中key是前后端一块定义的字段
            //value是前端传的数据
    },
    success:function(json){
        console.log(json);
    }
})
```
未来会用到的版本

```
        fetch('/get?ren='+txt.value)
         .then((e)=>e.json())
         .then(data=>{
             console.log(data);
         });

```

```
    fetch('/post',{
        method: 'post',
        headers: {
            'Content-Type': 'application/json'
        },
        body:JSON.stringify({'user':txt.value})
        }).then(e=>e.json())
        .then(e=>{
            console.log(e);
        });
        
```

> #### AJAX的交互流程

```
btn.onclick = function(){
    let xhr = new XMLHttpRequest; //创建ajax对象
    xhr.open('get','get',true);//填写请求地址
    xhr.onload = function(){ //等待服务器响应
        console.log(xhr.responseText);//接收信息
    }
    xhr.send();    //发送
}
```
1. 创建ajax对象

```
xhr = new XMLHttpRequest;
// XMLHttpRequest是ajax的核心
```

```
var xhttp;
if (window.XMLHttpRequest) {
//现代主流浏览器
xhttp = new XMLHttpRequest();
} else {
// 针对浏览器，比如IE5或IE6
xhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```

2. 规定请求地址
- xhr.open(method,url,async) -> 规定请求的类型，URL以及是否异步处理
```
method：请求的方式（get、post）
url：url地址+具体的参数
async(是否异步)：true(默认)代表异步，false代表同步
```

3. （监听）等待服务器响应
- xhr.onload

```
xhr.onload = function(){}
```

接收信息
- 在xhr.onload中接收到数据 xhr.responseText
- responseText代表获得字符串形式的响应数据，所以要转一下

`let data = JSON.parse(xhr.responseText);`

4. 向服务器发送请求
- send() -> 由open规定好的请求方式，发送到服务器

```
xhr.send();
```

总体流程


```
html部分

    用户名:<input type="text"  id="txt"/>
    <input type="button" value="提交" id="btn">
```

```
*** get方式

let btn = document.getElementById('btn');
btn.onclick = function(){
    let xhr = new XMLHttpRequest;//1、创建ajax对象
    xhr.open('get','/get?user='+txt.value,true);//2、规定请求地址
    xhr.onload = function(){//3、等待服务器响应
        let data = JSON.parse(xhr.responseText);
        //接收的信息(如果code等于0说明失败，等于1说明成功)
        if(data.code == 0){
            txt.className = 'error';
        }else if(data.code == 1){
            txt.className = 'ok';
        }
    }
     xhr.send();//4、发送请求
}
```
另一版本（使用onreadystatechange）

```
        var oBtn = document.querySelector('input');
        oBtn.onclick = function () {
            var xhr = new XMLHttpRequest();
            // data/2 -- 不存在
            xhr.open('get', '1.txt', true);
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4) {
                    if (xhr.status == 200) {
                        alert(xhr.responseText) // 如果加载完成，并且请求内容存在时
                    } else {
                        document.body.innerHTML = xhr.status + xhr.readyState;  // 如果加载失败,将错误的Http代码显示出来
                    }
                }
            }
        }
```

```
*** post方式

btn.onclick = function(){
    let xhr = new XMLHttpRequest;
    xhr.open('post','/post',true);
    //设置头信息
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
    xhr.onload = function(){
        console.log(JSON.parse(xhr.responseText));
    }
    xhr.send();
    
}
```





> #### onreadystatechange()事件
onload高版本，onreadystatechange所有版本
- 用于监听ajax的工作状态
- 当readyState改变时就会触发onreadystatechange （须在open前监听）

readyState属性
```
用来存放XMLHttpRequest的状态
从0-4发生不同的变化

0：请求未初始化（此时还没有调用open）
1：服务器连接已建立，已经发送请求开始监听
2：请求已接收，已经收到服务器返回的内容
3：请求处理中，解析服务器响应内容
4：请求已完成，且响应就绪 
*** 主要最后一条是核心

执行1-4步时候，每完成一步就会调用onreadystatechange，直到第4步为止
```

```
同步情况下，低版本IE按照正常的同步编程顺序解析1-4
同步情况下，高版本下直接走4，没有1 2 3
```

status属性

```
200表示请求成功
404表示请求失败
当 readyState 等于 4 且状态为 200 时，表示响应已就绪
```


由于有时请求的内容并不一定存在，所以要做容错处理

```
        var oBtn = document.querySelector('input');
        oBtn.onclick = function () {
            var xhr = new XMLHttpRequest();
            xhr.open('get', '1.txt', true);
            xhr.send();
            console.log(xhr);
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4) {
                    if (xhr.status == 200) {
                        alert(xhr.responseText) 
                        //如果加载完成，并且请求内容存在时
                    } else {
                        document.body.innerHTML = xhr.status + xhr.readyState;  
                        //如果加载失败,将错误的Http代码显示出来；
                    }
                }
            }
        }
```

兼容问题

```
IE低版本：面试考虑（send放下面）
1、创建ajax对象
2、填写地址
3、等待onreadystatechange
4、send发送    

 //低版本send在最后可以多监听一次
 //低版本send在事件上面，同步请求时候第四步监听不到
 
高低版本：
1、创建ajax对象
2、填写地址
3、send发送
4、等待onload
5、onload里面拿到返回信息
```


> #### get与post
get


```
从服务器端获取数据（给的少，拿的多）
get更简单快速，效率高

对于请求数据和静态资源（HTML页面和图片）在低版本IE下都会缓存
高版本浏览器只缓存静态资源，不缓存数据

什么情况下才会被浏览器缓存？
第二次地址和第一次地址一样（则会被浏览器缓存）第二次找到的资源为第一次的资源
解决方案：让第二次地址和第一次地址不一样即可
```

```
请求过程：
（1）浏览器请求tcp连接（第一次握手） 
（2）服务器答应进行tcp连接（第二次握手） 
（3）浏览器确认，并发送get请求头和数据（第三次握手，这个报文比较小，所以http会在此时进行第一次数据发送） 
（4）服务器返回200 OK响应 
```
post

```
从服务器端推送数据（给的多，拿得少）
```

```
请求过程：
（1）浏览器请求tcp连接（第一次握手） 
（2）服务器答应进行tcp连接（第二次握手） 
（3）浏览器确认，并发送post请求头（第三次握手，这个报文比较小，所以http会在此时进行第一次数据发送） 
（4）服务器返回100 Continue响应 
（5）浏览器发送数据 
（6）服务器返回200 OK响应 
```
get请求方式

```
btn.onclick = function(){
    let xhr = new XMLHttpRequest;
    xhr.open('get','/get?ren='+encodeURI(txt.value),true);//
    // 其中url里面get是后端给的，?后面user是前后端定义的（或者后端提供）
    xhr.onload = function(){
        console.log(xhr.responseText);
    }
     xhr.send();
}
```
post请求方式

```
btn.onclick = function(){
    let xhr = new XMLHttpRequest;
    xhr.open('post','/post',true);
    //设置头信息
    xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
    xhr.onload = function(){
        console.log(JSON.parse(xhr.responseText));
    }
     xhr.send('user'+txt.value);
}
```

两者区别
- get比post速度块
- get相对post安全性低
- get有缓存，post没有
- get的url参数课件，post不可见
- get请求参数会保留历史记录，post中参数不会保留
- get请求数据放在url，post数据在http包体(requrest body)内
- get只接受ASCII字符的参数数据类型，post没有限制
- get会被浏览器主动catch，post不会，需要手动设置
- get在浏览器回退时无害，post会再次提交请求
- get体积小（url字节长度每个浏览器不一样），post可以无限大（根据php.ini 配置文件设定）


```
传递给服务器信息方式不一样
get基于url地址（问号）传参方式
post基于请求主题传递给服务器

安全性方面，post相对get安全些
get基于问号传参把信息给服务器容易被URL劫持
```

应用场景
```
get常用于查找、展示轻量的数据
post用于修改和写入数据（比较大的数据传输）、操作用户信息
```

http状态码：

```
HTTP状态码（HTTP Status Code）是用以表示网页服务器HTTP响应状态的3位数字代码

比如在A网站写了location.href = B 网站
就可以说A网站做了个301跳转（没条件的转发）
301：比如输入http://www.xiaomi.com/直接跳转到http://www.mi.com
302一般是后台做的转发，需要后台判断逻辑，如果请求地址满足后台的话也可以不跳转，比如登录

100-199 信息；用于指定客户端应相应的某些动作。 
200-299 成功；用于表示请求成功。 
300-399 重定向；用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
400-499 客户端错误；用于指出客户端的错误。 
500-599 服务器端错误；用于支持服务器错误
```

```
200-307都算成功，其中5**就是后台服务端问题
*101：切换协议(websocket协议)
 * 200[OK]： 客户端成功接收到服务端的返回的数据
* 301： 永久重定向，当前域名地址已经永久跳转到另外一个域名地址中
 * 302： 临时重定向，一般情况下当服务器超过它的加载负荷范围之后，会重新跳转到一个新的服务器，一般是用来解决负载均衡，或者我们一般的项目的图片、文件、其他资料都放在其他的服务器当中
 * 304： 加载缓存，一般我们请求一个页面，浏览器会默认将这个页面的全部信息缓存的本地，当我们再次请求这个页面的时候，浏览器检测到输入的地址参数一样的话，会默认走缓存【加载更快】ctrl+f5实现强制刷新 也可以清除缓存，或者服务端可以设置响应头的时间进行变更也可以达到清除缓存的效果，
 * 我们如果想要每次请求都不走的缓存的话，可以这样操作
 * xhr.open('get','3.TCP协议.html?_='+Math.random(),true);
 * xhr.open('get','3.TCP协议.html?_='+new Date().getTime(),true);
 * 307: 临时重定向，针对的是http传输协议，一开始http=>https
 * 400：访问参数错误
* 401：没有访问权限（要求用户的身份验证）
 * 404：请求地址【资源】不存在
 * 413：客户端请求的文件超过服务端的最大承载的容量
 * 500：内部服务端错误
* 503：服务器超过最大的负荷
* 注意：只要是5xx 都是服务端的问题，只要页面是4xx 都是前端的问题 2xx和3xx 都可以表示请求的成功的意思

```


> #### 同步和异步
- 同步
```
程序运行从上而下，只要上面代码没运行完成，会阻塞下面代码；
浏览器必须把这个任务执行完毕，才能继续执行下一个任务
```
- 异步

```
程序运行从上而下，上面代码没运行完成，不会阻塞下面代码；
浏览器任务没有执行完，但是可以继续执行下一行代码；
```

```
事件和定时器都是异步的
```
浏览器如何规划同步异步机制？

```
1、浏览器是多线程的，Js是单线程（浏览器只给js执行分配一个线程）
2、js单线程中实现的异步机制，主要依赖于浏览器的任务队列完成。
```

复习

```
response - 所有
responseseXML - xml
responseText  字符串格式（除了处理json还可以处理xml）
一般常用responseText这个
```

```
onabort：中断ajax执行的操作
timeout：设置ajax请求的超时时间
ontimeout：超时之后执行的方法
settime：1000  等待时间
```

```
1、FormData()序列化表单数据

	var data = new FormData();
	data.append("hello","world");
	xhr.send(data);
	或者
	xhr.send(document.forms[0])
	这样可以快速传入post参数，而且无需设置content-type。

2、timeout超时

	xhr.timeout = 1000; //设置超时时间
	xhr.ontimeout = function(){}; //处理超时情况
	xhr.send(null);

3、overrideMimeType

重写MIME，MIME决定了你的responseXML或者responseText的值是否存在，MIME由content-type设置
```

## Cookie

Cookie是一种能够让网站Web服务器把少量数据储存到客户端的硬盘或内存里，或是从客户端的硬盘里读取数据的一种技术
- cookie是一种后端技术，前端也可以设置
- 作用为了保存信息
- 所有浏览器都识别，并且会缓存到浏览器中
- 使用cookie必须在服务器端运行
- cookie是以键值对的形式保存（key=value）
- 各个cookie之间一般是以“;”分隔
- 每个Cookie的大小一般不超过4KB
- cookie的默认生命周期是当浏览器关闭以后生命就结束

Cookie文件则是指在浏览某个网站时，由Web服务器的CGI脚本创建的存储在浏览器客户端计算机上的一个小文本文件，其格式为：用户名@网站地址 ［数字］.txt

---

设置cookie

```
document.cookie = key = value

比如 document.cookie="name="+username;

设置cookie都会调用tostring
可以通过JSON.parse转一下来解决
```

```
对于前端来说，种cookie就是给document设置了一个叫做cookie
自定义的属性，这个属性能被所有浏览器识别并缓存在浏览器中
```

读取cookie

```
document.cookie
返回结果会把域中所有cookie都显示出来
key=val; 每个之间以分号 空格来分隔
```
默认生命周期

```
当浏览器关闭之后cookie就被销毁（生命结束）了
```
通过expires可以设置生命周期

```
let d = new Date();
d.setDate(d.getDate()+1);
d.setMinutes(d.getMinutes() + 5);
document.cookie = 'name = xh;' +'expires='+d;
```
应用场景

```
猜你喜欢、身份验证、每日推荐、免登录等等
```

以下都是window对象的属性

---

- window.location 
```
对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面
```

- window.location.host

```
返回主机名或者当前URL端口号
```
- window.location.hostname

```
返回当前域名
```
- window.location.pathname

```
返回当前页面的路径和文件名
```

- window.location.hash

```
返回锚点部分hash值
```
- window.location.href

```
整个URl字符串(在浏览器中就是完整的地址栏)
不能新打开页面，只能在当前页面跳转
```
- window.open 【window对象的方法】

```
打开一个新的浏览器窗口或查找一个已命名的窗口

人为主动触发时候才不会被拦截
如果不是人为触发事件，高版本浏览器会拦截
```

- window.location.search

```
返回查询(参数)部分
```
下面是window.location对象的一些方法

---
- location.reload()

```
重新加载当前页面
默认不传参如果存在缓存会从浏览器缓存中加载；
如果传入Boolean类型的true，则会强制从服务器加载
```
- location.assign()

```
在浏览器的历史记录中增加一条新纪录
```
- location.replace()

```
使用新URL覆盖浏览器的当前历史记录
```

```
location.assign('http://www.baidu.com');
location.reload()   // 可能从浏览器缓存加载
location.reload(true)   // 强制从服务器端加载
PS：每次修改location的属性（除hash外），页面都会以新URL重新加载；
虽然修改location.hash页面不会重新加载，但是会在浏览器中生成一条新的历史记录
```






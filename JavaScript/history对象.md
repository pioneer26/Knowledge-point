## history对象
`window.history表示window对象的历史记录`
###### history 对象属性
history.length
```
查看历史记录中的个数（据说最高是50个）
```
###### history 对象方法
window.history.forward();

```
前进一个
```
window.history.back();

```
后退一个
```
window.history.go();

```
指定跳转到某 URL
```

```
windiw.history.go(-1);//相当于back()后退
window.history.go(1);//相当于forwar()前进
window.history.go(0);//相当于刷新
```
#### H5API扩展的window.history

`HTML5 History API包括2个方法、1个事件`

- history.pushState(state, title, url)

`添加历史记录`
```
在history内添加一个历史记录
（会增加后退效果，无刷新改变浏览器地址）
```
三个参数
```
state：状态对象，记录历史记录点的额外对象，可为null
title：页面标题，目前所以浏览器都不支持，需要的话可以用document.title来设置
url：新的网址，必须同域，浏览器地址栏会显示这个网址
```
`url参数如果是以'?'开头，则url的值会代替seach的值`

`如果是以'#'开头，则url的值会代替hash的值`

`如果是以'/'开头，则url的值会代替/后的值`

比如

```
window.history.pushState(null, '', 'a.html');
//页面不刷新，只是改变history对象，地址栏会改变
window.history.pushState(null, '', '#aaa');
//url参数带了hash值并不会触发hashchange事件
```
- history.replaceState(state, title, url)

`修改历史记录`
```
在history内修改一个历史记录
（不会增加后退效果，无刷新改变浏览器地址）
```

```
与pushState的区别：

它是修改浏览历史中的当前记录，而并非创建一个新的
```
- hashchange

`监听历史记录`
```
当前url的hash改变的时候会触发该事件
```

###### history.state属性

```
返回当前页面的state对象
改变此对象可以给pushState和replaceState的第一个参数传参
```
###### popstate事件
`每当同一个文档的浏览历史(即history对象)出现变化时，就会触发popstate事件`

onpopstate事件

`window.onpopstate是popstate事件在window对象上的事件处理程序`
```
当触发了浏览器的前进或后退按钮就会触发这个事件
在这个事件中，ev下有state属性。
可以接收到pushState时候的数据
```

`调用history.pushState()或者history.replaceState()不会触发popstate事件。popstate事件只会在浏览器某些行为下触发, 比如点击后退、前进按钮`


```
当网页加载时,各浏览器对popstate事件是否触发有不同的表现。
Chrome 和 Safari会触发popstate事件, 而Firefox不会
```

```javascript
window.onpopstate = function(event) {
  alert("location: " + document.location + ", state: " + JSON.stringify(event.state));
};
//绑定事件处理函数. 
history.pushState({page: 1}, "title 1", "?page=1");    //添加并激活一个历史记录条目 http://example.com/example.html?page=1,条目索引为1
history.pushState({page: 2}, "title 2", "?page=2");    //添加并激活一个历史记录条目 http://example.com/example.html?page=2,条目索引为2
history.replaceState({page: 3}, "title 3", "?page=3"); //修改当前激活的历史记录条目 http://ex..?page=2 变为 http://ex..?page=3,条目索引为3
history.back(); // 弹出 "location: http://example.com/example.html?page=1, state: {"page":1}"
history.back(); // 弹出 "location: http://example.com/example.html, state: null
history.go(2);  // 弹出 "location: http://example.com/example.html?page=3, state: {"page":3}
```


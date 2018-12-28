## DOM基础
- 内置属性默认（没有）是空字符串和number，自定义属性没有是undefined
#### DOM获取元素的方法

---
> Js中获取到的元素是对象数据类型的
- ###### 通过ID获取元素

```
<div id="box"></div>

document.getElementById('box');
//getElementById这个方法上下文只能是document，不可以是其他元素
```
- ###### 通过类获取元素


```
<div class="box"></div>

document.getElementsByClassName('box');
//document.getElementsByClassName获取到的是一个类数组 的集合
```
- ###### 通过标签名获取元素


```
<div class="box"></div>

document.getElementsByTagName('div'); //IE8不兼容
//getElementsByTagName上下文可以是元素
```
- ###### 通过name属性获取元素 


```
<input type="text" name="name1">

document.getElementsByName('name1');
//一般常用于表单元素
```

- ###### 万能选择器（常用于移动端）


```
<div id="box1"></div>
<div class="box2"></div>
//获取单个元素
document.querySelector('#box1'); //可以是id
document.querySelector('.box2');//可以是class
document.querySelector('div');//可以是标签名
//获取所有元素
document.querySelectorAll('#box1');
document.querySelectorAll('.box2');
document.querySelectorAll('div');
```
- ###### 获取HTML元素


```
document.documentElement 
```
- ###### 获取body元素


```
document.getElementsByTagName("body")[0]
```

---
#### DOM由节点组成
###### DOM的节点类型：{ 都是对象数据类型 }

```
1. 元素节点
2. 属性节点
3. 文本节点     
4. 注释节点 //其中空格和换行都是文本节点
5. document
```
> 三个键值对（nodeType、nodeName、nodeValue）

```
                      nodeType     nodeName      nodeValue
    元素节点           1         大写的标签名         null
    属性节点           2         大写属性名       属性的内容
    文本节点           3          #text           文本的内容
    注释节点           8          #comment        注释的内容
    document           9          #document       null
```


```
console.log(childNodes); //获取当前元素的所有子节点

console.log(children); //获取当前元素所有的子元素节点

console.log(parentNode); //获取当前元素的父亲节点
//document的父亲节点是null

console.log(nextSibling); //获取下一个弟弟节点
console.log(nextElementSibling); //获取下一个弟弟的元素节点 （IE8不兼容）

console.log(previousSibling); //获取上一个哥哥节点
console.log(previousElementSibling) //获取上一个哥哥元素节点（IE8不兼容）

console.log(firstChild); //第一个子节点
console.log(firstElementChild);//第一个子元素节点（IE8不兼容）

console.log(lastChild);//最后一个子节点
console.log(lastElementChild);//最后一个子元素节点

```

---
#### 动态操作DOM
> 创建一个元素

```
createElement()
```
> 向元素末尾添加一个子节点

```
appendChild()
```
> 删除一个子节点

```
removeChild() //接收一个节点类型的；参数是要删除的这个元素；
```
> 替换子节点

```
replaceChild(new,old); //用新的元素替换原有的元素
```
> 将新的元素插入到指定元素的前面

```
insertBefore(new,old);
```
> 克隆元素

```
cloneChild()
//接收一个布尔类型的参数 true,false
//如果不传参数，默认是false；
console.log(box.cloneNode(true)); //浅克隆
a.appendChild(box.cloneNode(true))//深克隆
```

> #### DOM映射机制
- DOM映射

```
通过DOM中方法获取到的元素，与页面一一对应的这种关系
（操作元素的时候不是重新创建一个元素，而是把对应的元素拿来操作）
```
- DOM回流/重排

```
当页面元素(宽高，位置，创建元素)发生改变，回导致页面重排，浏览器会根据新位置进行重新渲染
（重排是非常消耗性能，尽量减少操作）
```
- DOM重绘

```
当一个元素样式(颜色、背景、字体、边框等)发生变化，浏览器重新绘制的过程
（重绘相对于回流性能消耗较低）
```

`解决方法`

```
通过文档碎片(createDocumentFragment)或模板字符串，将操作的元素进行字符串拼接，最后一块打包返还给页面。
```
`注意`

```
querySelectorAll获取到的元素集合没有DOM映射
必须是元素的内置属性发生变化，浏览器才能监听得到
```
> #### DOM盒子模型
- 获取行间样式
```
style.width/style.height     //获取行间样式高度
盒子.currentStyle.height   //计算后的样式高度 IE低版本
getComputedStyle().height  //获取计算后的样式高度（不支持padding） 谷歌高版本（只可读不可改）
```
- 设置行间属性

```
设置自定义属性
xx.setAttribute('index','i'); //属性名，属性值

获取自定义属性
xx.getAttribute('index');//得到属性值

删除自定义属性
xx.removeAttribute('index');
```

- client系列（可视区域）

```
clientWidth/clientHeight //获取可视区域的宽高（支持padding 不支持border）
clientLeft //获取边框的左边框宽度
clientTop //获取边框的上边框的宽度
```
- offset系列（偏移量）
`使用时候要有定位，而且设置初始值；

```
offsetWidth/offsetHeight //获取容器宽高(在client的基础上，支持border)
offsetLeft //获取当前元素的左外边框到定位的父级左内边框的距离
//如果没有定位父级或者祖先级，那么就应该跟html走
offsetTop //指元素的边框的外边缘距离与已定位的父容器（offsetparent）的上边距离（不包括元素的边框和父容器的边框）
offsetaParent //定位父级
//（在没有任何定位的情况下它走的是body（在chrome中实测是走html））
```
- scroll系列（滚动大小）

```
scrollWidth/scrollHeight //获取元素本身的宽高 (被内容撑开的尺寸,不管加不加固定尺寸，都会获取出来)
scrolltop //获取或设置一个元素的内容垂直滚动的像素数
scrollLeft //获取或设置一个元素的内容水平滚动的像素数
```
> ###### getBoundingClientRect() 获取元素位置
- 获得页面中某个元素的左，上，右，下分别相对浏览器视窗的位置
- 指DOM元素到浏览器可视范围的距离（不包含文档卷起的部分）
- 返回一个object对象
- 有left,top.right,bottom,width,height六个值
```
right是指元素右边界距窗口最左边的距离
bottom是指元素下边界距窗口最上面的距离
width,height (只有高版本才有)
x轴和y轴(只有高版本才有)
```

```
var box=document.getElementById('box');         // 获取元素

alert(box.getBoundingClientRect().top);         // 元素上边距离页面上边的距离

alert(box.getBoundingClientRect().right);       // 元素右边距离页面左边的距离

alert(box.getBoundingClientRect().bottom);      // 元素下边距离页面上边的距离

alert(box.getBoundingClientRect().left);        // 元素左边距离页面左边的距离
```
`正常使用他们要做到以下几点`

```
清除默认样式
设置默认字体大小
触发haslayout
```

> ###### 总结
- 容器尺寸
```
clientWidth/clientHeight,offsetWidth/offsetHeight,scrollWidth/scrollHeight
```
- 边框

```
clientLeft/clientTop
```
- 浏览器可视区域的尺寸

```
document.documentElement.clientWidth(低版本IE和有滚动条时候使用，不会计算滚动条)
document.body.clientWidth chrome浏览器
window.innerWidth/window.innerHeight 高版本用//不计算（包括）滚动条的宽高

document.documentElement.clientHeight //计算（不包括）滚动条的宽高

document.documentElement == HTML
window.innerWidth || document.documentElement.clientWidth
```
> ###### 滚动的距离

```
window.scrollTo(x,y) 滚动の距离
document.documentElement.scrollTop //既可读又可写
window.pageXoffset/window.pageYoffset //只可读不可写
window.pageYoffset || document.documentElement.scrollTop
```

```
window.pageXOffset/pageYOffset
加上window.scrollTo(x,y)就可以写了
```

> ###### getAttribute()获取指定属性名的属性值

```
 <input type="text" name="ipt" id="ipt">
 
 console.log(ipt.getAttribute('name')); //ipt
```










 

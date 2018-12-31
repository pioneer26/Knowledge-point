> ## vue
vue.js是一套用于构建用户界面的渐进式框架

vue.js目标是实现响应的数据绑定和组合的视图组件

vus.js的核心是一个响应的数据绑定系统



`Vue.js的作者为Evan You（尤雨溪），曾任职于Google Creative Lab`
```
学习方式：
背指令、学思想、写逻辑（数据与视图）关系、路由、状态管理
```

`vue是一款MVVM框架`

`双向绑定数据（数据影响视图，视图影响数据）`

特点：

```
1、简洁
2、轻量
3、快速
4、数据驱动
5、模块友好
6、组件化
```

> ###### MVVM框架
M
```
model（数据或者模型）
指的是后端传递的数据
```
V

```
view（视图）
指的是看到的页面
```
VM

`通过视图操作数据，也能通过数据操作视图（双向绑定）`

```
view-model（视图模型）
MVVM模式的核心,连接view和model的桥梁
VM两个方面：
一是将数据转成视图，即将后端传递的数据转化成所看到的页面。通过数据绑定实现
二是将视图转成数据，即将所看到的页面转化成后端的数据。通过DOM事件监听实现

这两个方向都实现的，我们称之为数据的双向绑定
```

```
总结：
在MVVM的框架下视图和模型是不能直接通信的，他是通过view-model来通信的
```


代表框架

```
angular(ng-xx)、vue(v-xx)、emberjs、avalon
```

```
react与传统的MVVN框架不同的是：
react默认数据绑定方式是单向绑定，而vue及angular都是双向绑定
react使用虚拟DOM配合JSX，而vue及angular直接将数据通过属性绑定在真实DOM上的
```


> ###### MVC框架
MVC是Model-View- Controller的简写。即模型-视图-控制器

`使用MVC的目的就是为了将M和V相分离`
```
思想：相分离

model（数据） view（视图） control（控制器）

数据库
```
> ###### 渐进式（弱主张）
大白话：没有多做职责之外的事
```
一般的框架粘性很强。比如angular
只能开始用ng，那么项目就一直用它，不能和其他框架耦合
```

```
vue是弱主张，没有强主张。
在原有大系统的上面，把一两个组件改用它实现
也可以整个用它全家桶开发，当Angular用
还可以用它的视图，搭配你自己设计的整个下层用
```
vue全家桶

```
vuejs + vue-router + vuex + vue-cli
```
项目全家桶（推荐）

```
框架全家桶 + node + 数据库
```
简单实现步骤

```
1、先引入vue.js //跟jq一样引入vue库文件
2、body中创建一个元素，给他id或class，比如#app
3、实例化Vue
    {
            el:id或class名字
            data:{
                数据:''||{}||[]//可以是字符串、数组、对象
            }
    }
4、显示数据
{{数据名字}}//必须是两个花括号
```
> #### VUE指令
v-if
```
v-if="数据或者三目判断"
只要条件成立就显示if中的元素，否则不会渲染元素
```
v-else-if

```
v-else-if充当v-if的else-if块，可以链式的使用多次
也可以使用switch语句
```
v-else

```
搭配v-if来使用，它必须紧跟在v-if或者v-else-if后面，否则不起作用
如果if条件不成立显示当前的元素
```

v-show

`根据表达式之真假值，切换元素的 display CSS 属性`
```
条件成立就显示，不成立就display:none隐藏
（性能比v-if略好）
```

```
如果要频繁的切换使用v-show
如果运行条件不怎么改变使用v-if

元素少的时候用v-if
元素多的时候用v-show
```

v-for

```
一个参数：<div v-for="val in obj">{{val}}</div>
两个参数：<div v-for="(val,key)" in obj>{{val}}</div>
三个参数：<div v-for="(val,key,index)" in obj>{{val}}</div>
**花括号里面只能写一个变量
 {{}} -> {{index}}:{{val}}
```

```
val:数组中每个值，对象的每个值
key：数据的下标 对象的key值
index:下标 0 1 3
```

```
还可以使用for in形式

<div v-for="item of obj"></div>
```

```
注意：
当v-for 和 v-if同处于一个节点时候，v-for比v-if优先级高
意味着v-if将运行在每个v-for中
```

v-text

```
v-text => 原生的innerText 文本
```

```
<span v-text="msg"></span>
等价于
<span>{{msg}}</span>
//双花括号方式解析出来的数据是纯文本
//为了输出真正的html，可以使用v-html
```

v-html

```
v-text => 原生的innerHTML 内容（包括元素）
```
v-on

`v-on可以简写为@`
```
用来监听dom事件
v-on:事件名称 = "fn"
事件函数写在methods对象下
methods在根实例下，值为一个对象

methods:{
    函数名(){
        this指向实例
        this的数据修改，直接写this.数据名
     }
}
//改了数据，内容也会随之改变（只要操作数据即可）
```

```
//app就相当于根，放在app外面，是不识别的

 <div id="app">
      <button v-on:click ="fn">{{str}}</button>
 </div>
 
 简写@形式的
 
 <div id="app">
      <button @click ="fn">{{str}}</button>
 </div>
```

```
 let app = new Vue({
        el:'#app',
        //放数据
        data:{
            str:`按钮`,//实例下str为按钮
        },
        //放函数
        methods:{
            //值为一个对象
            fn(){
              this.str = '按钮来了';
            }
        }
```
v-bind

`v-bind可以缩写成:`
```
动态绑定一个或多个属性

 v-bind:属性名
 一般是用来操作属性。如class、style、src、href
```

```
:class="c" //可以写数据
:class="{red:true}" //red为类名 true是布尔值 只有为true时候才显示该class
:class="[c1,c2]"//c1和c2为对象 
```

```
//进行类切换的例子
<div id="app">
    <!--当data里面定义的isActive等于true时，is-active这个类才会被添加起作用-->
    <!--当data里面定义的hasError等于true时，text-danger这个类才会被添加起作用-->
    <div :class="{'is-active':isActive, 'text-danger':hasError}"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,  
            hasError: false
        }
    })
</script>

```

```
渲染结果
<!--因为hasError: false，所以text-danger不被渲染-->
<div class = "is-active"></div>
```
v-model

```
用表单控件或者组件上创建双向绑定
```

```
所谓的双向绑定，就是你在视图层里面改变了值，vue里面对应的值也会改变。只能给具备value属性的元素进行双向数据绑定。
```

> ###### vue实现选项卡应用

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="js/vue.min.js"></script>
    <style>
    .active{
        background:yellow;
    }
    </style>
</head>
<body>
    
    <div id="app">
        <!--循环数据，当点击时候把当前key传到changs方法中,然后把num赋给active-->
        <button
            v-for="(val,key) in box"
            @click="changs(key)"
            :class="{active:key == num}"
        >
        按钮{{key+1}}</button>
        <!--循环数组，只要key等于num就显示出来-->
        <div 
        v-for="(val,key) in box"
        v-show="key==num"
        >{{val}}</div>
    </div>
    <script>
        new Vue({
            el:'#app',
            data:{//放数据
                box['小黄车','小蓝车','摩拜单车'],
                num = 0
            },
            methods:{//放函数
               changs(key){
                   this.num = key;//把key赋给当前点击的值
               }
            }
        })
    </script>
</body>
</html>
```


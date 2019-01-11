## 路由

> ###### 路由历史
路由是根据不同的 url 地址展示不同的内容或页面

- 后端路由

```
1、切换页面是跳转全局刷新页面（用户体验差）
2、A页面的静态资源和B页面静态资源会重复请求
```

```
SSR->服务器渲染
网络爬虫在爬取资源时候会及时找到重要资源有利于SEO优化，
但对服务器压力较大。因此一般首页服务器渲染，其他页面使用ajax
```
优缺点：

```
优点：服务器渲染有利于SEO优化
缺点：1、静态资源重复请求，对服务器压力较大
      2、用户体验不好
```

- 前端路由


通过不同的路由 切换不同的页面
```
比如 根 / 到-> index.html
/2.html 到-> 2.html
```
`前端路由的主要模式是hash和history模式`

```
hash路由 -> #/ #/2.html
history路由 / /2.html
```

优势：

```
优点：
1、单页面应用，用户所有的操作都在一个页面完成
2、用户体验好，共享资源只需要请求一次即可
```
#### Vue 路由
`Vue Router 是 Vue.js 官方的路由管理器`

Vue.js + Vue Router 创建单页应用

> ###### vue-router实现原理

SPA(single page application)单一页面应用程序

```
只有一个完整的页面；
它在加载页面时，不会加载整个页面，
而是只更新某个指定的容器中内容
```
`单页面应用(SPA)核心：更新视图而不重新请求页面`

vue-router实现单页面前端路由，提供了两种方式
```
hash 模式 和 history 模式
//根据mode参数来决定采用哪一种方式
```

> ###### 使用vue-router
自动全局安装：

```
vue create 项目名称
```

手动配置：

1、安装

在app项目文件夹里面

```
npm i vue-router
```
2、在min.js下引入包

```
import Vue from 'vue'
import VueRouter from 'vue-router';
```
3、使用use来引用（使用）

```
Vue.use(VueRouter);
```
4、需要配置router -> route.js

```
引入组件
配置路由表
```
配置路由表（当前是哪个路径需要执行哪个组件）

`配置什么路径就显示什么组件`
```
比如
'/' ->app  
'/ppa' ->ppa
```
```
const routes = [//一个数组
//配置什么路径就走哪个组件
  {
      path:'/', //根路径
      component:app //app就是根
  },
  { 
    path: '/foo',//路径
    component: Foo//组件
  },
  { 
    path: '/bar',
    component: Bar
  }
];
export default new VueRouter({
    router:routes
    //等同于
    //router
})
```
`在new vue-router时候，通过routes来引配置数组`

`new Vue时候是通过router来引路由`

5、放到跟下

```
 new Vue({
    el:'#app',
    render:()=>h,
    router
 })
```

6、渲染，在页面中定义router-view 标签

```
<router-link></router-link>//为了跳转
//有children时候，父组件必须有router-view
<router-view></router-view>
//路由匹配到的组件将渲染在这里
```

```
<router-link>默认会渲染成一个a标签
<router-link tag="button">
这样可以加一个按钮的样式
```

```
栗子：
<ul id="main">
	<li><router-link  to="/food" >商品</router-link></li>
	<li><router-link  to="/rating">评价</router-link></li>
	<li><router-link  to="/seller">商家</router-link></li>
</ul>
```

#### <router-link>
> ###### to
表示目标路由的链接

（一般的to放一个简单字符串）

```
通过 to 属性指定目标地址
默认渲染成带有正确链接的 <a> 标签
```

```
to可以放路径，也可以是 v-bind:to

/*字符串*/
<router-link to="home">home</router-link>

/*v-bind 的 JS 表达式 */
如果是:to，路径要写成字符串

比如<router-link :to="'home'">Home</router-link>

:to 可以是字符串也可以是对象

/*命名的路由 params*/
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

/*带查询参数，下面的结果为 /register?plan=private*/
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
```

`使用params时候要用name去引，不使用path`

```
:to="{path:'a'}"
:to="{params:{a:5}}"  

 注意:如果使用params，
 那么就不能定义path，
 改为使用name
 
 :to="{query:{a:5}}"
```
> ###### tag
通过配置 tag 属性生成别的标签

```
比如生成一个按钮
<router-link tag ="button"></router-link>
```
> ###### active-class
默认值 "router-link-active"
```
设置 链接激活时使用的 CSS 类名
//默认值可以通过路由 linkActiveClass 来全局配置
```
> ###### exact
默认值 false

```
"是否激活" 默认类名
```

```
<!-- 这个链接只会在地址为 / 的时候被激活 -->
<router-link to="/" exact>
```


> ###### exact-active-class
默认值 router-link-exact-active
```
配置当链接被精确匹配的时候应该激活的 class
```

> ###### 激活class 应用在外层元素
`让激活 class 应用在外层元素，而不是 <a> 标签本身,可以用 <router-link> 渲染外层元素，包裹着内层的原生 <a> 标签`

```
<router-link tag="li" to="/foo">
  <a>/foo</a>
</router-link>
```
#### 静态路由

```
一般常用的普通路由

const routes = [
    {
        path:'/a',  //路径
        component:a //组件
    }
]
```

#### 动态路由
`我们需要把某种模式匹配到的所有路由，全都映射到同个组件。我们可以使用 vue-router 的动态路径`

一个“路径参数”使用冒号 : 标记
```
<template>
    <ul>
        <li v-for="(val,key) in obj[$route.params.num]">{{val}}</li>
    </ul>
</template>
```

```
const router = new VueRouter({
    routes:[
        //动态路径参数以冒号开头
        {
            path:'/about/:num',
            component:about
        }
    ]
})

export default{
    data(){
        return{
            obj:{
                a:[1,2,3],
                b:[4,5,6],
                c:[7,8,9]
            }
        }
    }
}

输出结果
//localhost:8080/about/a            1 2 3
//localhost:8080/about/b            4 5 6
//localhost:8080/about/c            7 8 9
```

`当匹配到一个路由时，参数值会被设置到 this.$route.params，可以在每个组件内使用`

```
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

```
除了 $route.params 外
还提供了$route.query 和 $route.hash
```

#### 嵌套路由（children）

`children`

如果有子级路由，那么记得在父级身上加

```
<router-view></router-view>
```

此时组件就会在父级组件显示

`不管嵌套多少层都遵循此规则`

`多级嵌套的话可以使用一个模板`

```
const routes = [
    {
        path:'/',
        component:APP
    },
    {
        path:'/child',
        component:Child,
        children:[
            {
                path:'one',
                component:One
            },
            {
                path:'two',
                component:Two
            },
            {
                path:'three',
                component:Three
            }
           /*
            1、组件名字一般首字母大写
            2、子路由路径不用加/
           */
        ]
    }
]
```
解决激活状态切换时，加不了事件的问题，使用.native

```
比如：
@click.native = ""
```
栗子
```
children:[
    {
        path:'tamen/:name',
        name:'tamen',
        component:Tamen
    }
]

//wo.vue文件里面
//(切换时候会把params带过去)
<template>
    <div>
        <h2>关于我</h2>
        <router-link :to="{
            name:'tamen',
            params:{
                name:'铁松'
            }
        }">
        </router-link>
        /*只要是父组件一定要写*/
        <router-view></router-view>
    </div>
</template>
export default {
    name:'tamen'
}
```

#### 路由对象
`在组件内即this.$route,存着一些与路由相关的信息`

一个路由对象表示当前激活的路由的状态信息


> ###### 常用的路由对象属性

`如果要在刷新页面时候通过路由的信息来操作数据，
可以在created下使用this.$route 这个属性`
- name

```
//当前路由的名称，如果有的话
//如果没有使用具名路径，则名字为空
```

- path

```
 //字符串，等于当前路由对象的路径，
 //会被解析为绝对路径
 如 "/home/news" 
```
- fullPath

```
//完成解析后的URL路径
//包括查询信息和hash的完整路径
```
- query
```
//查询信息 ?
比如
路径 /about?user=1
$route.query.user == 1 来查询
```
- hash

```
// 哈希信息 #
*** hash
```
- parmas

```
//预设的变量（设了之后可以取)
//切换时候，通过parmas带过去某个id的值
*** 如果使用params,就不能定义path,改为name来引用
```
- redirectedFrom
```
//重定向
//如果存在重定向，即为重定向来源的路由名字
```
- meta

```
//meta 埋一个字段，在路由表里面
```
-  matched

```
//一个数组，
//包含当前匹配的路径中所包含的所有片段所对应的配置参数对象
```


```
export default {
    data(){
        return {
            n:0 //表示为根目录
        }
    },
    created(){//当数据渲染完之后
        this.$route
    }
}
```
#### 编程式路由

`$route -> 使用它的属性`

`$router-> 使用它的方法`

可以使用this.$router的方法去切换路由(路由跳转)

`想要导航到不同URL，则使用$router的方法push、go、replace`

常用的路由方法：

- push()

```
放入跳转的路由/路径
比如：this.$router.push('/c')
//想要导航到不同URL，则使用 router.push 方法
```

```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

- go(num)

```
在 history 记录中向前或者后退多少步

//类似于：window.history.go(n)

跳到第几个
比如：
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)
//后退一步记录，等同于 history.back()
router.go(-1)
```
- replace(location)

```
把当前路径替换成xxx
比如：['/','/a','/c']
```

```
replact 与 push不同的是：
它不会向 history 添加新记录
而是替换掉当前的 history 记录
（使用replac跳转的 后腿按钮不能点击 ）
```

`设置to(link跳转）时候要加上/斜杠+路径
不然点击多次link时候会累加 重复`

#### 路由元信息（meta）
定义路由时候可以配置meta字段

```javascript
const router = new VueRouter({
    routes:[
        {
            path:'/foo',
            component:'Foo',
            children:[
                {
                    path:'sub',
                    component:Sub,
                    meta:{
                        requiresAuth: true 
                    }
                }
            ]
        }
    ]
})
```
我们称呼routes配置中每个路由对象为路由记录

`一个路由匹配到的所有路由记录会暴露为 $route 对象（还有在导航守卫中的路由对象）的 $route.matched 数组。我们需要遍历 $route.matched 来检查路由记录中的 meta 字段`



路由元信息说白了就是通过meta对象中的一些属性来判断当前路由是否需要进一步处理，如果需要处理，按照自己想要的效果进行处理即可


```
//下面例子展示在全局导航守卫中检查元字段

router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

应用场景

```
1、权限认证，限制路由的访问
2、传递信息，可以用作路由记录
3、实现不同路由展现不同页面
（带不同的meta信息，展示不同页面布局）
```


#### 重定向（redirect）
`当访问某个路径时候，跳转到另一个路径`
```
{
   path:'/',
    redirect:'/xx'
}
```
```
//栗子-从 /a 重定向到 /b

const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```
重定向可以通过name指定一个组件

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```
重定向可以是一个方法，动态返回重定向目标

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})

//return一个字符串或者一个对象
```
重定向中的 to

```
//to 可以获取从哪里来的一个对象,记录了一些上一次的路由信息

常用的有：parmas,query,hash,path
```
#### 过渡动效
`<router-view> 是基本的动态组件，所以我们可以用 <transition> 组件给它添加一些过渡效果`

```
<transition>
  <router-view></router-view>
</transition>
```


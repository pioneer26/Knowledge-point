
## 组件化开发

组件 (Component) 是 Vue.js 最强大的功能之一

组件可以扩展 HTML 元素，封装可重用的代码

`把一个大的功能拆分成若干个小的功能，解决高耦合的问题，方便开发人员维护`

```
所有的 Vue 组件同时也都是 Vue 的实例
可接受相同的选项对象 (除了一些根级特有的选项) 并提供相同的生命周期钩子
```
- 语法

```
vue.component(组件名称,对象) 必须放在跟实例 new Vue()的上面
```

- 命名规则：

尽量不使用驼峰命名

小技巧：

```
1、组件名字和引入组件名字尽量一样
2、如果要写驼峰命名，引入组件名字时候，大写变小写，中间带-
    烤串命名法：比如创建Vue.component('pp-x',{})，引入<pp-x></pp-x>

结构中自定义属性名字不要与子组件数据（data中）名字一致
```

- 步骤

```
1、vue.component(组件名称,对象){}
2、new Vue()
3、引入组件名字，比如<ppa></ppa>
```
栗子

```
<div id="app">
    <!--引入组件，名字和创建组件的一致-->
    <h1>父组件</h1>
    <my-component></my-component>
</div>
```
在template中，顶层只能有一个元素
```
Vue.component('my-component',{
/*template里面必须有一个顶级的div盒子*/
    template:`<div id="box">子级</div>`,//需要放在父级的盒子里面 
});
new Vue({
    el:'#app'  
})
```
> #### 子组件
在子组件里面的data必须是函数形式，函数中返回一个对象。对象下挂数据
```
Vue.component('my-component',{
    template:`
        <div>   //template中的顶级盒子
            <h4>子级</h4>
            <button @click="fn">按钮的{{val}}</button>
        </div>
    `,
    data(){//子组件里面必须是函数形式，函数中返回一个对象
        retrun{
            val:'子组件'
        }
    },
    methods:{
        fn(){
            alert(1);
        }
    }
});
new Vue({
    el:'#app'
});
```
> #### 组件传递
`props`

```
props接收的可以是数组也可以是对象
数组时候传字符串，
使用对象是为了启用高级配置（传入数据类型的检测、自定义校验和设置默认值）
```

```
*** 在传入数据时候，类型检测会帮助我们更快的锁定问题（故意报错）

// 简单语法
Vue.component('props-demo-simple', {
  props: ['size', 'myMessage']
})

// 对象语法，提供校验
Vue.component('props-demo-advanced', {
  props: {
    // 检测类型
    height: Number,//number是一个类，不是数字
    // 检测类型 + 其他验证
    age: {
      type: Number,//数据类型
      default: 0,//如果子组件没有这个属性，默认走default
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})
```


1、在父组件上面挂一个自定义属性
```
<div id="app">
     <ppa c="子组件"></ppa>
</div>
```
2、子组件上设置一个props接收（可以是数组也可以为对象）

```
数组里面写自定义属性的名字
 比如 {
        props:['c'],
        template:`
            <div>{{c}}</div>
        `
    }
```


```
    <div id="app">
       <!--引入组件，名字和创建组件的一致-->
        <ppa c="子组件"></ppa>
   </div>
   <script>
    Vue.component('ppa',{
       props:['c'],
       template:`
        <div>{{c}}</div>
       `,
       data(){
           return {
               val:'123'
           }
       }
    }     
   )
   new Vue({
       el:'#app'
   });
   </script>

```
> ###### 父级数据传递子级
1、通过在子组件身上挂一个v-bind:自定义属性 = 父级数据
```
<ppa d="传进来的数据"></ppa>
```
2、子组件通过props接收

```
props:['data']
```
3、使用

```
{{data}}
```

```
 <div id="app">
        <h1>{{arr}}</h1>
        <hr>
        <ppa :data="arr"></ppa>
</div>
```

```
let obj = {
    props:['data'], //子组件通过props接收
    template:`
        <ul>
            <li v-for="(val,key) in data">{{val}}</li>
        </ul>
    `
}
Vue.component('ppa',obj);
new Vue({
    el:'#app',
    data:{
        arr:[1,2,3,4]
    }
});
```
> ###### 子级数据传递给父级

`$emit`

单项数据流动（父级数据和子级数据的流动）

`子级不能修改父级数据（通过告知父级改哪个数据，父级修改完之后再传给子级）`
```
数据从父级流向子级，数据本身还是父级的
如果操作子级要改变父级的数据，只能通过子级告知父级要操作哪个数据
然后让父级去修改自己的数据，修改完毕再传给子级
```
简单类型，直接修改会报错
复合类型，用的时候可以深度拷贝
```
子传父：
    1、在子组件上绑定一个自定义的事件，并且传入父级的“事件”处理函数
    比如 <ppa @changebool="changeC"></ppa>
         @changebool是子组件定义的
         changeC是父组件定义的
    2、子组件内部监听这个自定义的事件，(满足自定义函数的条件就调用执行)
    比如 change(){
        this.$emit(changebool,id)//自定义事件名称，参数
        
    }
    *** 不能直接在子级修改父级的数据
```
自定义事件

document.addEventListener('点击',fn)
```
鼠标点击了页面，点击行为已经产生，这个时候只要找到对应的事件名下数组，循环数组依次调用
```
单项数据流动 另一种解决方案

```
如果要让子级有功能（操作父级数据的能力），可以把父级传进来的数据变成自己的。
子级改变自己的数据是不会影响父级
```

```
1、传数据
    :data = "arr"
2、通过pops:['data']接收
3、把父级传过来的数据变成自己的
    data(){
        return {
            cd:this.data
        }
    }
4、使用自己的数据
`<div>{{cd}}</div>`

注意:
如果父级传进来的数据是复合类型，那么变成自己的数据时候要深拷贝一下
不然改变子级时候会影响父级
```

---
木偶组件

```
只是为了接收和渲染数据，基本没什么逻辑
项目越往下越木偶
```
功能组件
```
更多是为了控制数据，有大量的逻辑
越往顶层越是功能组件
```
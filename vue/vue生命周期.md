> #### 生命周期钩子
Vue的生命周期就是一个vue组件或实例从出生到死亡的过程

`所有生命周期钩子的this上下文指向他的vue实例`


Vue每一个组件或者实例都会经历一个完整的生命周期

总共分为三个阶段：初始化、运行中、销毁

```
1、实例、组件通过new Vue() 创建出来之后会初始化事件和生命周期，然后就会执行beforeCreate钩子函数，这个时候，数据还没有挂载呢，只是一个空壳，无法访问到数据和真实的dom，一般不做操作

2、挂载数据，绑定事件等等，然后执行created函数，这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数，在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取

3、接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染，然后执行beforeMount钩子函数，在这个函数中虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated，在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取

4、接下来开始render，渲染出真实dom，然后执行mounted钩子函数，此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了，可以在这里操作真实dom等事情...

5、当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿

6、当更新完成后，执行updated，数据已经更改完成，dom也重新render完成，可以操作更新后的虚拟dom

7、当经过某种途径调用$destroy方法后，立即执行beforeDestroy，一般在这里做一些善后工作，例如清除计时器、清除非指令绑定的事件等等

8、组件的数据绑定、监听...去掉后只剩下dom空壳，这个时候，执行destroyed，在这里做善后工作也可以
```

```
所有的Vue应用都是从这里开始的，
当实例化出Vue对象时就已经进入了Vue的生命周期。

进入的生命周期第一个钩子函数就是beforeCreate。
在这之前组件还没有真正的初始化。
在beforeCreate之后，Vue对data对象作了getter/setter处理，
并且将对象放在一个Observe里面以便于监控对象，
另外还有使用initEvents绑定事件
在组件初始化完成后，遇到第二个钩子函数：created。
在这个阶段我们可以访问到了data的属性以及绑定的事件
通过了created阶段后组件的生命周期到了beforemount，
在这个阶段DOM结构还没有生成，
但是已经创建了el，组件挂载的根节点。
在beforemount执行完成后开始渲染DOM，
执行_render方法，_mount方法，
然后会有new出一个watcher对象,
形成VNode节点，然后会更新DOM
渲染完毕后组件就会到了下一个生命周期mounted，
一般的异步请求都会写在这，
这个阶段DOM已经渲染出来了。
至此一个组件已经完整的出现在眼前了
，但是生命周期却还没有停止。
当组件需要更新的时候生命周期会先到达beforeUpdate,
在这个阶段显示数据并没有更新，
但是DOM中的数据已经改变了，
这是因为双向绑定的关系
走过beforeUpdate组件完成了更新，
生命周期走到updated
完成更新后的组件应该被销毁了，
beforeDestroy，这个阶段组件还没有被销毁
destroy这个是真正的销毁
```

```
生命周期什么时候触发 ？
new Vue时候就触发了
```
> ###### vue中$mount和el的区别
两者在使用效果中没有区别，都是将实例化后的vue挂载到指定的dom元素中
```
如果在实例化vue的时候指定el，则该vue将会渲染在此el对应的dom中
反之，若没有指定el，则vue实例会处于一种“未挂载”的状态，此时可以通过$mount来手动执行挂载
```

```
el:'#app'//自己写个el
或者
vm.$mount('#app')//实例身上绑定个mount
```

`看看有没有el属性，没有再看看实例下有无$mount(el)`

`template属性会覆盖app根元素`


```
模板文件的三种写法

1.template:'<div></div>'
2.
<template id="demo1">
    <div>demo1</div>
</template>
template:'#demo1'
3.
<script type="x-template" id="demo2">
    <h2 style="color:red">我是script标签模板</h2>
</script>
template:'#demo2'
```

`有模板走模板，没模板走根元素的innerHTML`

1、beforeCreate 在组件初始化之前

这个生命周期组件的数据、方法、事件还都没有

简单来说,new Vue之后 第一句话就调用beforeCreate
```
new Vue({
    beforeCreate()
});
```

```
function Fn(age){
    beforeCreate()
    this.age = age;
}
```

```
应用：可以使用loading
```
2、created 当数据初始化之后调用

```
当数据、方法、事件初始化之后调用
简单点来说，当数据有初始值之后调用
```

```
function Fn(age){
    beforeCreate()
    this.age = age;
    created();//这时候拿到数据就可以修改数据了
}
```

```
应用：一般都是请求数据的时候用，还可以关闭loading
```

```
1和2的应用loading栗子

new Vue({
    el:'#app',
    data:{
        val:''
    },
    created(){
        setTimeout(()=>{
            this.val = '数据请求成功';
            document.getElementById('loading').style.display = 'none';
        },2000)
    },
    beforeCreate(){
    //debugger阻塞一下
        document.getElementById('loading').style.display = 'block';
    }
});
```
3、beforeMount 渲染之前

```
beforeMount之前el上还是undefined
```

4、mounted 渲染之后

```
Dom渲染之后，可以获取页面的（主要是数据生成出来的）元素

nextTick:当DOM更新完成时触发，只要操作DOM就使用nextTick(不用依依映射)
```


5、beforeUpdate 数据更新之前

```
当组件或实例的数据更改之后，会立即执行beforeUpdate
（一般不做什么事儿）
```


6、update：数据更新之后

```
（一般使用computed居多）
可以直接使用computed即可
```

7、beforeDestroy 生命周期销毁之前

```
应用场景：关掉定时器、状态初始化
```

8、destroyed 生命周期销毁之后之后

```
Vue实例销毁后，数据就不能添加
```

```
（没有路由时候，人为手动触发）

（有路由，切换路由时候，上一个组件触发）
```


常用的生命周期

```
beforecreate : 举个栗子：可以在这加个loading事件 

created ：在这结束loading，还做一些初始化，实现函数自执行 

mounted ： 在这发起后端请求，拿回数据，配合路由钩子做一些事情

beforeDestroy： 你确认删除XX吗？ destroyed ：当前组件已被删除，清空相关内容
```

生命周期钩子函数的执行
```
 var app = new Vue({
      el: '#app',
      data: {
          message : "xuxiao is boy" 
      },
       beforeCreate: function () {
                console.group('beforeCreate 创建前状态===============》');
               console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
               console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
               console.log("%c%s", "color:red","message: " + this.message)  
        },
        created: function () {
            console.group('created 创建完毕状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el); //undefined
               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
               console.log("%c%s", "color:red","message: " + this.message); //已被初始化
        },
        beforeMount: function () {
            console.group('beforeMount 挂载前状态===============》');
            console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
            console.log(this.$el);
               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
               console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
        },
        mounted: function () {
            console.group('mounted 挂载结束状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
            console.log(this.$el);    
               console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
               console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
        },
        beforeUpdate: function () {
            console.group('beforeUpdate 更新前状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);   
               console.log("%c%s", "color:red","data   : " + this.$data); 
               console.log("%c%s", "color:red","message: " + this.message); 
        },
        updated: function () {
            console.group('updated 更新完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el); 
               console.log("%c%s", "color:red","data   : " + this.$data); 
               console.log("%c%s", "color:red","message: " + this.message); 
        },
        beforeDestroy: function () {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);    
               console.log("%c%s", "color:red","data   : " + this.$data); 
               console.log("%c%s", "color:red","message: " + this.message); 
        },
        destroyed: function () {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);  
               console.log("%c%s", "color:red","data   : " + this.$data); 
               console.log("%c%s", "color:red","message: " + this.message)
        }
    })
```
面试——阐述vue生命周期

`一般来说用的最多的是created和mounted`

`在new 了vue之后 一开始进行beforecreate，这时数据还没有初始化。可以去做一个loading动画，然后进入created 这时数据已经初始化（data的方法已经有了），此时可以请求数据。请求数据一般用created，在执行中看有没有el属性，有的话就走el，可以把根节点挂载上。没有的话就直接看vm身上有没有$mount，如果两个都没有，（缺少挂载点），生命就结束了。如果有挂载点就看看有没有template属性，有的话就把挂载点的值替换掉，没有的话找根节点的innerHTML。接着进入beforeMount，这时字符串，模板都已经编译好还没有进入页面，此时进入mounted状态，Dom就有了。当更改数据时候，数据是更新了，但试图不一定能一一映射，这时候就使用nextTick直接拿到DOM，不用依依映射。当数据变化就到了beforeUpdate和updated，这两个一个是更新前一个是更新后，一般用computed居多。然后就到了beforeDestroy和destroy，像路由切换就用destroyed，没有路由时候，人为手动触发，有路由，切换路由时候，上一个组件触发`
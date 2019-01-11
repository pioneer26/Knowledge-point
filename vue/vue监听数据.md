> #### computed 计算属性

`优点：指定看哪个数据就只看哪个数据`

概念

能监听vue中数据的变化，当数据发生变化时候会触发
```
页面上来就会执行一次，只要数据变化就会继续触发
//你在操作数据的时候，派生出另一个事情
//比如在用户输入输入框改变数据的时候过滤数字
```
用法
```
在html模板中{{函数名}} 
    其中函数名字不要和data中数据 名字重复
```
```
1、函数形式
computed:{
    listenArr(){
    /*
    使用data中数据，自动帮你监听数据的变化
    返回的结果就是通过改变数据 做的另一件事情（当数据变化时候需要做的事情）
    */
    }
}
应用：（1）、通过数据变化来进行累计 
      （2）、通过单选按钮的变化，来进行判断是否全选

```

```
2、对象形式
computed:{
    listenArr:{
        get(){
            //获取时候
        },
        set(newVal){
            //修改时候
        }
    }
}
//当使用get set时候，computed中定义的属性为一个对象
//不适用get set时候，computed中定义的属性可以是一个函数
```
栗子

```
<div id="app">
     全选：<input type="checkbox" v-model="all">
     <hr>
     <input type="checkbox" v-model="val.checked" v-for="(val,key) in arr">{{arr}}
  </div>
```

```
new Vue({
       el:'#app',
       data:{
           arr:[
               {
                checked:false
               },
               {
                 checked:false
               },
               {
                checked:false
               },
               {
                checked:false
               }
           ]
       },
       computed:{
           all:{
               get(){
                   //获取时候执行的操作
                   return this.arr.every(e=>e.checked);
               },
               set(newValue){
                //修改时候执行的操作
                    return this.arr.forEach(e=>e.checked = newValue)
               }
           }
        }
   });
```
> #### watch 监听vue数据的变化
当指定数据发生变化时候触发

`优点：可以通过watch把数据存储到本地`

```
页面一开始不会触发，只有指定的数据每变化一次就触发一次
```

```
应用：种localStorage的时候，深度添加
```

第一种写法

```
watch:{
    要监听的数据名（新的数据，更新之前的数据）{
        只能监听一层，深层就监听不到
        []->一层 [{}] ->多层
    }
}
```
第二种写法

```
watch:{
    //要监听的数据名:{
        handler(新的数据，更新之前的数据){
            
        },
        deep:true;//深度监听
    }
}
```

```
<div id="app">
      <input type="text" v-model="val">
  </div>
```

```
 new Vue({
    el:'#app',
    data:{
        val:'沙河'
    },
    watch:{
        //取值必须与上面的名字一致
        val(v,ov){//v代表改变之后的数据，ov代表改变之前的数据
            //改变数据时候看看触没触发
            console.log(v,ov);
            localStorage.setItem('station',v)
                }
            }
    });
    function getStorage(station,init){
        //有的话走station,没有的话走init
        return localStorage.getItem(station)||init
    }
```

watch与computed的区别

```
watch一开始不会触发，只有数据变化时候才会触发
computed一开始会执行一次，只要数据变化就会继续触发
```

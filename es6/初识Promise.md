## Promise

Promise是es6中新增加的类（new Promise）,目的为了管理JS中异步编程，也叫“Promise”设计模式


`Promise本身是同步的，只是用来管理异步编程的一种模式`

promise的三种状态

```
pending（准备状态：初始化成功，开始执行异步的任务）

fulfilled（成功状态，已经成功）

rejected（失败状态，已经失败）

最终只有两个状态，1、准备 2、成功或者失败
```


基本语法
- ES6 规定，Promise对象是一个构造函数，用来生成Promise实例
- Promise两个参数：resolve和reject


```
* resolve：当异步操作执行成功，执行的方法

* reject：当异步操作执行失败，执行的方法
```
-  Promise实例生成后，用then方法指定resolved状态和rejected状态的回调函数
- 其中then方法可以写多个


```
new Promise((resolve,reject)=>{
    // 执行一个异步任务
    //（new Promise时候 创建一个Promise实例，会立即执行这个函数）
    setTimeout(()=>{
        resolve(100);
    },1000)
}).then(()=>{//继续做什么事情
    //第一个传递的函数resolve
    console.log('ok');
},()=>{
    //第二个传递的函数reject
     console.log('error');
});

/*
1、异步操作放在Promise传的函数里面
2、Promise的参数与 then的参数相对应
*/
```
Promise来管理异步操作（其中then可以写多个）

```
let pro = new Promise((resolve,reject) =>{
    //执行一个异步操作
    let xhr = new XMLHttpRequset();
    xhr.open('get','1.js',true);
    xhr.onreadyStatechange() = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            val = xhr.responseText;
            resolve(val);//成功
        }
        if(xhr.status!==200){
            reject();//失败
        }
    }
    xhr.send();
})
pro.then((resolve)=>{
    console.log('ok');
    //数据绑定
},(reject)=>{
    console.log('error');
}).then(()=>{
    //当第一个then中的函数执行完，会执行第二个
}).then(()=>{
     //当第二个then中的函数执行完，会执行第三个
})

```
> ##### 回调地狱
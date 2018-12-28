 Generator[ˈdʒɛnəˌretɚ] 摘呢瑞德
 
 先说下同步和异步
 
```
同步：连续的执行即为同步
```

 
> #### 概念
`Generator函数是es6提供的一种异步编程解决方案`

Generator函数，也是一种函数，不过这种函数在它的内部，封装了多个状态机

并且这个函数的执行结果，返回的是一个遍历器对象。


***  Generator函数有两个特征
```
1、function关键字与函数名之间有一个星号 *

2、函数体内部使用 yield 表达式 （定义不同的内部状态）
```


这个遍历器对象可以通过其next()方法，得到这个Generator函数的每一种状态

每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止

`Generator函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行`
```
   function* next(){
       yield 1+1;
       yield 1+2;
       yield 1+3;
       return 5;
   }
   let v = next();
   console.log(v.next());   //{value: 2, done: false}
   console.log(v.next())    //{value: 3, done: false}
   console.log(v.next())    //{value: 4, done: false}
   console.log(v.next());   //{value: 5, done: true}
   console.log(v.next());   //{value: undefined, done: true}
```


```
函数在调用时候执行next方法，碰到yield或return就停
（停到了yield或return代码这一段上）

next可以传入数据，为了操作下一步运算
```



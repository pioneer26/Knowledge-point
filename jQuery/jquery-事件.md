> #### jq事件

jQ的所有的事件都是on()这个方法，扩展出来的(基于on二次封装的)

```
 click()
 mouseover()
 ....
```

- trigger()

```
触发被选元素的指定事件类型
```

```

    $('#btn').on('click',function(){
        console.log('第一次');
    });
    $('#btn').on('click.2',function(){
        console.log('第二次');
    });

    $('#btn2').on('click',function(){
        $('#btn').trigger('click.2');
    });
```

- off()

注意：在事件套事件的时候，容易出问题(重复绑定的问题)
```
在事件套事件时候，容易出现事件重绑的问题。使用off可以把上次的事件清除掉
off可以放精确的范围,比如具体的事件名称，事件名的class
```

```
$("#btn").click(function(){
    $("#box").click(function(){
        console.log(1);
    })
})

//触发第一次btn的click事件的时候，就会在box上加了一个click事件，之后，无需btn点击，box也会有个click事件
//解决方法：使用off()，每次加事件之前先把上次的事件清除，如下：

$("#btn").click(function(){
    $("#box").off().click(function(){
        console.log(1);
    })
})
```
off(可以放精确的范围) 比如:具体的事件名、事件名的class

```
$('#box').click(function(){
    alert(1);
});

$('#box').on('click.就是你',function(){
    alert(2);
})

$('#btn').click(function(){
    $('#box').off('click.就是你');
})
```


on绑定事件的几种方式

```
on('不带on的事件名',fn) ->  on('click',function(){})
on('不带on的事件名',target,fn) -> 事件委托
on('不带on的事件名',{key:val},fn) -> 事件名后面是对象
```

```
jq中的event对象是二次封装的对象，增加了很多东西。ev.data 就是绑定的数据
```

```
如果在 jq 中没有找到原生的对象属性，还可以去对象的 originalEvent（原生的事件对象）找
```

```
let arr = ['1111','2222','3333'];
arr.forEach(e=>{
        let $btn = $('<button>按钮</button>');
        $btn.on('click',{e},function(ev){
            // console.log(ev.data.e);
            console.log(ev);
        })
        $('#box').append($btn);
    });
```
jq版拖拽

```
 $('#box').mousedown(function(ev){
        let disX = ev.pageX - $('#box').position().left;
        let disY = ev.pageY - this.offsetTop;
        // console.log(this)
        $(document).mousemove(function(ev){
            $('#box').css({
                left:ev.pageX - disX,
                top:ev.pageY - disY
            });
        });
        $(document).mouseup(function(ev){
            $(document).off();
        });
    });
```
> ###### 自定义事件
1、收集器（订阅模式） add
```
目的为了收集事件名和事件函数

** 其中订阅模式就是为了绑定事件和事件函数
```
2、触发器（发布模式） trigger

```
当某个条件成立之后，执行对应时间的事件函数
```
3、接触器 off

```
销毁对应事件的事件函数
```

```
/*订阅模式：就是为了绑定事件和事件函数*/
function add(ele,evName,fn){
        //给对象绑定一个diy的属性，值为对象，如果之前绑定过了，就用之前的，如果从来没有绑定过，就为{}
        ele.diy = ele.diy || {};  //box.diy = {};
        //看看之前这个对象里有没有同样的属性，如果有，就用之前的，没有设置为[]
        ele.diy[evName] = ele.diy[evName] || [];
        //往当前这个事件对应的数组中push本次的事件函数（因为是事件绑定，所以每次绑定的事件不会覆盖，而是都被保留）
        ele.diy[evName].push(fn);
}
//解析：
    在元素ele下加一个属性diy，里面有各种事件
    diy:{
        click:[fn1,fn2],   //事件下有绑定的多个事件函数
        mouseover:[],
        '点击':[fn1,fn2]
    }

    ele.diy = {
        click:[]
    }
```

```
/*发布模式：当某个条件成立之后,执行对应事件的事件函数*/
 function trigger(ele,evName){
    //如果元素没有绑定过这个事件，那么就返回null
    if(!ele.diy[evName])return null;
    //如果绑定了事件，此时，说明这里事件名对应的值，是存放事件函数的数组
        ele.diy[evName].forEach(fn=>{
        //调用（执行）这个事件函数
        fn.call(ele);
    });
}
```

```
/*接触器：销毁对应事件的事件函数*/
function off(ele,evName,fnName){
    if(!ele.diy[evName])return null;
    /*
        循环对象中对应的事件数组
        找到数组中和第三个参数中的函数名一致的函数删除
    */
    for(let i=0;i<ele.diy[evName].length;i++){
        if(ele.diy[evName][i].name === fnName.name){
            ele.diy[evName].splice(i,1);
            i--;
        }
    }
}
```

高版本自定义事件 - dispatchEvent

```
  document.addEventListener('上滚',function(ev){
        console.log(ev);
        // alert('正在山gun');
    });
    
     //创建event的对象实例。
    var event = document.createEvent('HTMLEvents');
       
    // 3个参数：事件类型，是否冒泡，是否阻止浏览器的默认行为
    event.initEvent("上滚", true, true);

    //给event对象绑定数据
    event.name = '小胖';
    event.num = 0;

    
    document.onmousewheel = function(ev){
        if(ev.wheelDelta > 0){
            event.num = ++ event.num;
            document.dispatchEvent(event);
        }
    }

```


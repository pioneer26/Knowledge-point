js的类库（方法库），把一堆常用的方法进行了封装，能够更好的兼容各种浏览器

`jquery在浏览器中全局window，在node下，全局是this`

`Write less and do more`
- Js的方法库（类库）
- 把一堆常用的方法进行封装
- 能够更好的兼容浏览器
- 方便操作DOM
> ###### jquery语法

```
//简写
$(function(){
    
})

=>等价于

//标准写法
$(document).ready(function(){
    
})
```
DOM文档加载的步骤

```
(1) 解析HTML结构
(2) 加载外部脚本和样式表文件
(3) 解析并执行脚本代码
(4) 构造HTML DOM模型
(5) 加载图片等外部文件
(6) 页面加载完毕
```


原生对象转jq对象

```
$()包起来
```
jq对象转原生，加下标

```
$('btn')[0].innerHTML = '点击';
```
> ###### jquery选择器
- 基本选择器

```
$('#box')
$('.box')
$('li')
```
- 属性选择器

```
 $('ele[属性key=属性val]') -> $('li[class="li1"]')
                             $('input[type="text"]')
```
- 层级选择器

```
parent child -> $('form input')
parent > child -> $('form > input')
prev + next  -> $('label + input')
prev ~ siblings  -> $('form ~ input')
```
- 伪类选择器

```
:first 匹配第一个元素
:last  匹配最后一个元素
:even  匹配所有索引值为偶数的元素，从 0 开始计数（现实中从1计数的奇数）
:odd   匹配所有索引值为奇数的元素
:lt(num) 小于某个索引
:gt(num) 大于某个索引 
:checked 匹配所有选中的被选中元素(复选框、单选框等，不包括select中的option)
```
> ###### jq常用方法
val()

```
$(ele).val()//获取
$(ele).val('xx')//设置

原生写法：ele.value
```
css()

```
$(ele).css('width');   //获取
$(ele).css('width','200px');   //设置
$(ele).css({ width:'200px',height:'200px' });   //批量设置

原生写法：ele.style.css
```
attr()

```
$(ele).attr('id');//获取属性
$(ele).attr('id',3);//设置属性值
$(ele).attr({//批量设置属性值
    id:3,
    num:10
});
```
prop()

```
专门操作表单元素
$('input').prop('checked',false)
```
html()

```
$(ele).html();   //获取
$(ele).html('123');   //设置

原生写法：ele.innerHTML
```
创建元素

```
let li = $('<li></li>');
```
添加元素

```
addClass()
```
删除元素

```
removeClass()
```

eq(index)

```
找到一组元素的某个，并且还是 jq 对象，非原生
```
index()

```
1、默认为父级为基准，找到子级的索引
2、小技巧：在使用index的时候，把精确匹配条件加上
    index('button')   //以一堆button为依据，当前点击的是谁
```
siblings()

```
1. 以当前节点为基准的所有兄弟节点
2. 小技巧: 因为兄弟节点包含的元素很多，使用时把精确匹配条件加上
     siblings('div')   //找到所有兄弟节点为div的元素
```
each()

```
用于循环遍历jq 对象
```
hover()

```
hover等于onmouseleave 和 onmouseenter

$('#box').hover(function(){
    $(this).css('background','red');
},function(){
    $(this).css('background','#000');
});
```
> ###### jq对DOM的增删改
增加

```
append() - 在被选元素的结尾插入内容
prepend() - 在被选元素的开头插入内容
after() - 在被选元素之后插入内容
before() - 在被选元素之前插入内容
```
删除

```
remove() - 删除被选元素（及其子元素）
empty() - 从被选元素中删除子元素
```
修改

```
replaceWith() - 用指定的 HTML 内容或元素替换被选元素
replaceAll() - 使用给定的参数 replacement 替换字符串所有匹配给定的正则表达式的子字符串

//两者差异在于语法：内容和选择器的位置，以及 replaceAll() 无法使用函数进行替换。
```
查找

```
jQuery.parent(expr)           //找父元素

jQuery.parents(expr)          //找到所有祖先元素，不限于父元素

jQuery.children(expr)        //查找所有子元素，只会找到直接的孩子节点，不会返回所有子孙

jQuery.contents()            //查找下面的所有内容，包括节点和文本。

jQuery.prev()                //查找上一个兄弟节点，不是所有的兄弟节点

jQuery.prevAll()             //查找所有之前的兄弟节点

jQuery.next()                //查找下一个兄弟节点，不是所有的兄弟节点

jQuery.nextAll()             //查找所有之后的兄弟节点

jQuery.siblings()            //查找兄弟节点，不分前后

jQuery.find(expr)            //跟jQuery.filter(expr)完全不一样，jQuery.filter(expr)是从初始的

jQuery对象集合中筛选出一部分，而jQuery.find()的返回结果，不会有初始集中筛选出一部分，
而jQuery.find()的返回结果，不会有初始集合中的内容，
比如：$("p").find("span")是从元素开始找，等于$("p span")
```

> ###### attr()与prop() 属性操作
- attr操作自定义属性
- prop操作表单元素
```
.attr('key','val')
// 一个参数是字符串为获取，若是对象为批量设置
//传两个参数是设置
```
> ###### 全选与反选栗子

```
        <button id="btn1">全选</button>
        <button id="btn2" >不选</button>
        <button id="btn3" >反选</button>
        <input type="checkbox">
        <input type="checkbox">
        <input type="checkbox">
        <input type="checkbox">
        <input type="checkbox">
        <input type="checkbox">
        <input type="checkbox">
```

```
    $('#btn1').click(function(){
       $(':checkbox').prop('checked',true);
   })
   //全不选
   $('#btn2').click(function(){
       $(':checkbox').prop('checked',false);
   })
   //反选
   $('#btn3').click(function(){
       let ck = $(':checkbox');
       for(let i=0;i<ck.length;i++){
       //如果里面的按钮是选中状态就让checked为false，否则为true
           if($(ck[i]).prop('checked')){
            $(ck[i]).prop('checked',false);
           }else{
            $(ck[i]).prop('checked',true);
           }
       }
   });
```
> ###### jq无new初识化操作
`在构造函数内不能new自己，否则会递归。无new化主要是为了减少用户的操作`

那么既要实现不让用户new，又要实现不递归，还要保证构造函数的所有方法都能使用
此时，可以通过另一个函数与构造函数的属性、方法一样，然后去new该函数
这样就完成了无new化操作
```
如果直接在jq中使用new,会造成递归
解决方法：找一个拥有jq所有方法的替身，然后new该方法
```

```
    (function(global,factory){
        factory(global);
       
    })(typeof window !== 'undefined'?window:this,function(global,noGlobal){
        function JQuery(seletcor){
            return new Ts(seletcor);
        }
        /*
            一个参数为字符串的时候是获取，是对象就批量设置
        */
        JQuery.fn = JQuery.prototype = {
            constructor:JQuery,
            css:function(){
                // console.log(123);
                if(arguments.length === 1){
                    if(typeof arguments[0] === 'string'){
                        return getComputedStyle(this.ele[0])[arguments[0]];
                    }
                }
            }
        }

        var Ts = JQuery.fn.ts = function(seletcor){
            if(typeof seletcor === 'string'){
                seletcor = document.querySelectorAll(seletcor);
                this.ele = seletcor;
            }else if(typeof seletcor === 'function'){
                document.addEventListener('DOMContentLoaded',function(){
                    seletcor();
                });
            }
        }

       Ts.prototype = JQuery.fn;
        

        window.$ = window.JQuery = JQuery;
    });
```
> ###### noConflict
`noConflict方法为了解决重命名`

在jQuery库中，几乎所有的插件都被限制在它的命名空间里

通常全局对象都被很好的储存在jQuery的命名空间里

当jQuery与其它的JavaSscript库使用时不会引起冲突

```
一般情况下jQuery 中用“”号作为自身的快捷方式，但我们也可以把这个控制权转交给其它库
```
在其它javascript库引入完成后，再引入jQuery库，我们可以利用jQuery库中自带的jQery.noConflict()函数把变量“$”的控制权交给其它js库

移交快捷方式jQuery.noConflict()
```
jQuery.noConflict();//将变量$交给其他库
jQuery(function(){//使用jQuery
    jQuery('div').click(function(){//这里不能在使用$方法
        alert(123);
    })
})
```
自定义快捷方式jQuery.noConflict()

```
$jq = jQuery.noConflict();//自定义快捷方式&方法
$jq(function(){
    $jq("div").click(function(){
        alert();//操作代码
    })
});
```




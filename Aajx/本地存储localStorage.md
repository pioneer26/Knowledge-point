> ### window.localStorage
- 体积一般5M，未来可能更大
- 生命周期只要不清除，一直会存在

设置本地存储

```
localStorage.setItem('user','小明');
只要
```
获取本地存储

```
localStorage.getItem('user');
```
删除本地存储

```
localStorage.removeItem('user');
```
清除本地存储

```
localStorage.clear();
```

> #### onstorage

```
当本地缓存修改时候，自己不触发，所有兄弟页面都会触发
```

```
 document.onclick = function(){
        localStorage.setItem('name','aoe');
    }
    
另外一个页面

 onstorage = () => {
        console.log('隔山大牛');
        window.location.href  = 'http://www.baidu.com';
    }
```

本地缓存

```
let style1 = localStorage.getItem('style1');
    if(!style1){
        head.innerHTML += '<link rel="stylesheet" href="./1.css" id="s1">';
        fetch('1.css')
        .then(e=>e.text())
        .then(e=>{
            localStorage.setItem('style1',e);
        });
    }else{
        head.innerHTML += '<style>'+ localStorage.getItem('style1') +'</style>';
    }
```

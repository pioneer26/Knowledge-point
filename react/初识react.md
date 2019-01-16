## react

- React 是Facebook推出的一个用于构建用户界面的 Javascript 库
- 核心思想->通过数据操作试图（目的实现组件化开发）

`react算是MVC中的V层框架`

关于react
```
1、不是一个MVC框架
2、不使用模板
3、响应式更新简单
4、html5 仅仅是开始
```
掌握知识点

```
1、jsx语法
2、state
3、props
4、数据通信
```

### 优势
声明式

```
通过声明一个类或函数来创建UI的
class Person{
    //声明了一个类
}
```

###### 组件化

```
将复杂的页面分割成若干个独立组件，
每个组件包含自己的逻辑和样式，
再将这些组件组合成一个复杂页面。
```

```
1、可组合
//一个组件可以和其他组件一起使用 或 直接嵌套在另一组件内部

2、可重用
//每个组件都有独立的功能，可以被使用多个场景

3、可维护
//每个小组件仅包含自身逻辑，容易理解和维护
```
组件化优势：

```
即减少了逻辑复杂度又实现了代码重用
```

---

react全家桶

```
react + react-router-dom + redux（mobx）
```

```
react-native 开发原生APP
```

#### create-react-app

`react官方推荐的脚手架`

安装create-react-app

```
npx create-react-app my-app
cd my-app
npm start
```
###### ReactDOM.render()

```
React的最基本方法
用于将模板转为html，并插入指定DOM节点
```

```
三个参数：（前两个常用）
参数1、模板或组件
参数2、挂载的根节点
参数3、挂载完成后触发的回调函数
```

###### myapp项目

public文件夹

```
放静态资源
//其中index.html是根文件
```
src文件夹

```
index.js //相当于vue中的min.js
app.js
```

```javascript
index.js

import React from 'react';//引入react库
/*把jsx转成能够浏览器识别的工具*/
import ReactDOM from 'react-dom'//引入react-dom

ReactDOM.render();//根节点的挂载组件
```
###### 使用

1、引包

```
import React from 'react';//引入react库
import ReactDOM from 'react-dom'//引入react-dom
```
2、挂载组件

```
ReactDOM.render(
<div>你好</div>,//顶层只能有一个标签（类似于vue的template）
document.getElementById('root'),
//其中root对应于public下index.html中的#root
()=>{
    console.log('挂载成功');
}
);
```


```javascript
import React from 'react';//引入react库
import ReactDOM from 'react-dom'//引入react-dom

/*
ReactDOM.render();  根节点的挂载组件
    用于将模板转为html，并插入指定DOM节点
三个参数：
    参数1->模板或组件
    参数2->挂载的根节点
    参数3->挂载完成后触发的回调函数
*/
if(module.hot){
    module.hot.accept();//热更新
}

ReactDOM.render(
    <div>hellow,react</div>,//模板或组件
    document.getElementById('root'),//挂载的根节点
    ()=>{
        console.log('挂载成功');//挂载后的回调函数
    }
);
```
#### jsx语法

`用来创建虚拟DOM和声明组件`
```
JavaScript + xml语法
//一种js和自定义写法的混合模式
```

```
为了更好的读、写模板或组件
jsx 语法浏览器不识别
通过babel 来进行转换成浏览器识别的代码
```

```
react中写<div></div>
就等于React.createElement('div,等等');
```
jsx 优势

```
1、jsx执行更快
2、安全，编译时能及时发现错误
3、JSX 编写模板更加简单快速
```

###### jsx规则
1、jsx语法中只能有一个顶级标签（元素）

```javascript
/*
下面写法是错误的
<div>你好</div>
<p>世界</p>
*/
```
2、使用组件时，首字母必须大写

```
import Banner from ./Banner

ReactDOM.render(
    <div>
        <Banner />
    </div>,
    document.getElementById('root')
 )
```

3、遇到样式中 class, 就变成 className

```
<div className="App"></div>
```


4、所有元素标签必须闭合（尤其单标签）

```
<input />
```

5、花括号{} 

`在JSX中通常通过{} 的方式插入值，但设置style属性需要{{ }}`

```
1) 可以放任意js代码
//与vue区别，vue{{}} react {}
2) {}会默认展开数组
比如[1,2,3,4] 打印出的 是 1234
3) 注释 {/**/} 在花括号里面注释
4) {}还可以声明函数，但不能直接调用
5) 输出数据时候，赋值给某个元素属性
//比如 value={a}
6） 设置style也用{}，里面可放对象
//比如style={{width:'200px','height:100px'}}
```

```
[<Li />,<Li /]
如果数组值为组件（列表），
循环时 一定要给列表中每一项加一个key（数组中唯一），为了优化
```

6、表单设置value默认值 要使用两种方式解决

```
/*
报错原因：react会认为input是一个受限组件，
动态会操作数据，value就会变化
变化就要有事件（onChange）
*/
1、给表单元素加事件onChange（受控组件）
2、定义默认值使用 defaultValue（非受控组件）
//把value变成非动态
```

```
比如<input value="1">

第一种写法
<input 
    value="1" 
    onChange={(ev)=>{}}
/>

第二种写法
<input value="1" defaultValue="1">
```

```
表单元素如果设置一个默认值（基于state中状态）

```

7、jsx表达式不能使用if else

```
可以使用三元运算符
```

---

#### 定义组件的方式
通过class 定义的 两种方式

第一种方式
```
import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component{}
```
第二种方式（常用）
```javascript
先在import引入时解构一下Component

import React,{Component} from 'react';
import ReactDOM from 'react-dom';

class App extends Component{
    render(){
       //return上面可以写逻辑、解构，下面是UI
        return (//放入UI
            <div><h1>你好</h1></div>
        )
    }
}
ReactDOM.render(
    <div>
        <App />
    </div>
);
```
#### 数据之间的通信（单向数据流）
传递数据->在父组件身上使用属性的绑定

```
比如 
 <App aaa="你好" bbb="world" ccc={[1,2,3]}{...{
            aaa:1,
            bbb:2,
            ccc:[1,2,3]
        }}/>
```
子组件接收->props

```
this.props
```

类的首字母一定要大写

`组件挂载时候会先执行constructor，只会执行一次，经过几个钩子函数 再执行render
只要数据发生变化，就再次执行render,而constructor不会改变`

```javascript
class App extends React.Component{//App这个类继承了react的组件
    //constructor只执行一次
    constructor(props){//props是父级传过来的
        super(props)
       /*
       状态初始化必须放在constructor里面
       没有super就拿不到this
       只要更新state就会更新视图
       */
        this.state = {//自己的数据
            num:0
        }
    }
    click = () =>{
        let {num} = this.state;
        this.setState({num:++num});
    }
    //数据发生变化就会执行 render
    render(){//render方法，需要return
        let {aaa,bbb} =  this.props;//通过this.props接收
        return (
            <div>
                <h2>组件</h2>
                <div>{aaa}{bbb}</div>
            </div>
        )
    }
}
```
```
ReactDOM.render(
    <div>
        <App aaa="你好" bbb="world" ccc={[1,2,3]}{...{
            aaa:1,
            bbb:2,
            ccc:[1,2,3]
        }}/>
    </div>,//模板或组件
    document.getElementById('root')
);

```
```
父级传递子级，把数据挂在子组件属性上
子组件接收父组件的数据通过this.props
```

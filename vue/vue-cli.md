## vue-cli脚手架
官方配置的关于使用vue的各种功能，这些功能称之为脚手架

`vue不支持IE8，因为vue使用了IE8无法模拟的 ECMAScript 5特性`

```
比如想编译一下es6的语法，
就使用 babel loader 和 babel core
（可以转换es6代码）
webpack中安装babel loader babel core
npm install --save-dev babel-loader babel-core
```

> ###### 利用npm安装
1、安装vue-cli只需要安装一次即可

```
npm i -g @vue/cli
或者使用yarn
yarn global add @vue/cli
```
2、创建项目

```
vue create 项目名称
```
3、使用第二个自定义Manually select features，不使用默认形式default配置（不然会有esLint，有一个空格就会报错）

```
使用上下键来切换配置
```

4、选择需要的配置项

```
比如：
Babel
Router
Vuex

选择和取消选择星星使用空格或者n键
```
出现

In dedicated config files

In package.json （选择这一个）

5、最后出现 cd 项目名称和运行服务命令 （说明成功）

```
比如
cd ppx
npm run serve

输出 npm run serve命令 运行服务
```
6、打开localhost:8080 访问默认主页

其他：

```
文件目录在src文件夹 min.js文件
App.vue做模板引入文件
组件放在component文件夹
```
一个默认的vue文件里面包含三项

```
一套东西：

template模板
    <div id="app"></div> 根
script
    export default{} 导出对象出去
            
style
```

引入组件
```
1、import 引入
2、components:{}
3、把组件放入template中
```

在component文件夹里面新建个list.vue组件文件

```
<template>
    <div>
        <ul>
            <li v-for="(val,key) in arr"></li>
        </ul>
    </div>
</template>
```

```
<script>
export default {
    data(){
        return{
            arr:[1,2,3,4]
        }
    }
}
</script>
```
App.vue文件

通过export default把App这个组件导出去，给min.js使用

`组件名字 类 一般首字母大写`

`react里面必须首字母大写`
```
<template>
    <div id="app">
        <span>你好</span>
        <hr>
        <!-- 
        list子组件标签可以是单标签
        单标签必须有结束符 /
        -->
        <List />
    </div>
</template>
```

```
<script>
//导入组件文件
import List from './component/list';

export default {
    component:{
        List
    }
}
</script
</script>
```
举个例子

比如在component文件夹 建个text.vue组件

```
<template>
    <div><input type="text"></div>
</template>
```

```
<script>
//导出去
export default{
    
}
</script>
```

然后在App.vue里引入一下

```
<template>
    <div id="app">
        <!-- 
        list子组件标签可以是单标签
        单标签必须有结束符 /
        -->
        <Text />
    </div>
</template>
```

```
<script>
import Texts from './component/texts';
export default{
    components:{
        Texts
    }
}
</script>
```


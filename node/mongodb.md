## mongodb

`mongodb是一个基于分布式文件存储的数据库`

`MongoDB是非关系型数据库`
###### 关系型数据库

```
Sql Server、mysql、Oracle
```

网址：https://www.mongodb.com/

`可以配合node环境使用`

安装步骤

```JavaScript
1、找到所安装的目录，
比如E:\BJ\web\zhufeng\bosh\DB
输入mongod 按tab补全
然后输入 --dbpath = 数据库存放的位置
--port=27017
//27017是mongodb默认
```

```
2、重新打开窗口使用npm i 进行安装
```

```
3、输入npm run dev 运行服务
```

```
4、安装成功，输入localhost:80进行访问
```

mongod

```
//mongod  没有b
开启服务
```

```
数组如何放进数据库？  需要mongodb
```

---
#### mongoose
官网 https://mongoosejs.com/

先启动数据库

```
找到mongodb目录
bin/运行环境
启动mongod.exe
mongod --dbpath=数据库的目录 --port=27017
```


安装Mongoose

```
npm install mongoose -S
```
引包

```
const mongoose = require('mongoose');
```
连接

```JavaScript
mongoose.connect('mongodb://localhost:27017')
```
监听连接状态（是否成功）

```
mongoose.connection.on('connected',()=>{
    if(error){
        //连接失败
    }else{
         //连接成功
         //一般在连接成功之后写后台逻辑
    }
    
})
```

栗子

```
const Koa = require('koa');//引入koa依赖
const app = new Koa();//实例化Kow
const static = require('koa-static');//引入静态资源
const path = require('path');//引入路径
const mongoose = require('mongoose');//引入mongoose

//连接数据库
mongoose.connect('mongoose://localhost:27017')
//监听是否成功
mongoose.connection.on('connect',(error)=>{
    if(error){
        console.log('连接失败');
    }else{
        console.log('连接成功');
        app.use((ctx)=>{
            //在连接成功里面写后台逻辑
            
        })
    }
});
app.use(static(path.join(__dirname,'www')));
app.listen(3000);//3000端口
```

###### Schema

`mongoose中 一切都始于Schema，为建模`
```
schema -> mongoose对表结构的定义
还可以定义实例方法，静态模型
//相当于做月饼的 mo  zi
```

```
mongoose.Schema({key:val,key2:val})
//里面放对象，定义值的类型
//key 数据名字 val 数据类型
```

```JavaScript
const studentsSechma = new mongooseSchema({
    name:String,
    age:Nuber
})
```
需要model 去接收 这个 Schema

```
mongoose.model('Kate',Schema)
//参数第一个是名字，第二个是建模
```


```
新建个文件夹 schema
创建一个animal.js  动物的 模
```

```
const mongoose = require('mongoose');
//建模
const Animal = mongoose.Schema({
    name:'大猫',
    type:'猫科'
});

module.exports = new Animal;//导出
```

```
再建一个model文件夹
animal.js文件
```

```
const mongoose = require('mongoose');
const Animal = require('./schema/animal');

module.exports = mongoose.model('animal',Animal);

```


```JavaScript
let pig = new Animal({
    type:'pig',
    name:'佩奇'
});
pig.save();

app.use((ctx)=>{
    Animal.find(
    {name:'佩奇'},
    (err,res)=>{//回调函数
        if(err){
            ctx.body = {code:1,msg:'找不到'}
        }else{
            //console.log(red)
            ctx.body = {
                code:0,
                data:res,//把数据返回
                msg:'成功'
            }
        }
    })
})
```

```
实例化类
const Animal = mongoose.model('名字','类型')
//通过对象方式创建他
let pig = new Animal({type:,age:12})

写入数据库
pig.save();

查询操作
find 查询所有
//koa中使用async和await
Animal.find(查询条件,()=>{})

输入给前台
ctx.body = {} 
```
查询条件

```

```
添加

```
save
```

更新

```
user.update({
    {'user':'pig'},
    {'pwd':'123'}
})
```
删除

```

```



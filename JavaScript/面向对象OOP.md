## 面向对象(OOP)

> #### 单例模式（Singleton pattern）
- 单一并且独立的实例模式
- 缺点：单一、不好维护，容易暴露数据，数据容易被修改

```
作用：把描述同一种事物的属性和特征归纳在一起，以避免全局变量冲突
```

```
   let person1 = {
       name:'小明',
       age:20,
       home:'北京'
   }
   
   //person1 和 person2都是独立的命名空间，两者互不干扰
   
   let person2 = {
       name:'小红',
       age:18,
       home:'杭州'
   }
   let aa = person2;
   aa.home = '上海';
   console.log(person2.home); //上海
```

```
把描述同一件事情的属性或者方法存放在某一命名空间，多个命名空间中的属性和方法是互不干扰的
```
> #### 高级单例模式
- （模块化开发）每一个模块都是独立的，不会出现全局污染
- 匿名函数自执行 + return 对象，对象中可以开放对外接口
- 更容易维护，功能更加丰富
- 高内聚低耦合(功能完整独立,减少冗余代码，耦合度低，方便拆解和组合)

```
let obj = (function(){ //匿名函数自执行
    fn..
    let ..
    return{
        ..
        ..
    }
})
```
```
低耦合高内聚：
模块与模块之间不冲突，很少有联系。功能与功能之前是独立的
可以减少冗余代码
```

```
let police = (function(){
    
})()
```

```
 let police = (function(){
    let name = '警察';
    function kongfu(){
        console.log('擒拿手');
    }

    function fadan(){
        console.log('开罚单');
    }

    function 狙击手(){
        console.log('开枪');
    }

    function 便衣(){
        kongfu();
        狙击手();
    }
    return {
        便衣,
        狙击手
    }
})();

console.log(police); //{便衣: ƒ, 狙击手: ƒ}

```

> #### 工厂模式
- 把相同代码封装起来，实现批量生产目的的一种模式（返回对象）
- 工厂方式，就是面向对象的封装函数


```
function person(name,age,sex){ //工厂模式 (外界也有人说这叫构造函数)
        //引进原材料，一张白纸
        let obj = {};
        //材料加工
        obj.name = name;
        obj.age = age;
        obj.sex = sex; 
        
        //最后出厂，把obj对象返回出去
        return obj;
    }
    let p1 = person('马国耀',18,'男');
    let p2 = person('李冰洲',22,'男');

    console.log(p1.name)
```
> #### 构造函数（模式）
- 类，配合new运算符来使用

```
普通函数的机制

1、形成一个私有作用域
2、形参赋值
3、变量提升
4、代码从上而下执行
5、栈内存释放问题（是否释放）

构造函数的机制:

1、形成一个私有作用域，接着形参赋值和变量提升
2、代码执行之前在私有作用域创建一个实例，让this指向这个实例
3、代码自上而下执行
4、return（不写return也会返回实例对象）
return 是简单类型值，返回值是类的实例
       是复合类型值，返回值是复合类型值（把原来值覆盖）
```

```
function Person(name,age,sex){ //构造函数
        //引进原材料
        let obj = {};
        //材料加工
        obj.name = name;
        obj.age = age;
        obj.sex = sex; 
        
        //出厂
        return obj;
    }
    let p1 = new Person('马国耀',18,'男');
    let p2 = new Person('李冰洲',22,'男');

    console.log(p1);
```


> #### New 关键字
- 一元运算符，专门运算函数
- new之后this就是实例化对象

```
1、加了new之后，等同于函数运行。可以不加括号 （加括号是为了传参）
2、new之前，this是window。 new之后，this是实例化对象 Fn{}，默认空白对象
3、new之后默认返回值由undefined变为实例化对象
4、new之后，如果return后面是简单类型，返回值就是this实例化对象
            如果return后面是复合类型，返回值还是复合类型值
```
> #### constructor
- 对象的原型链下的一个属性（构造函数原型下的属性）
- 默认指向实例的构造函数（只是具有参考性的指针）,容易被修改

```
只要改变构造函数原型下的constructor，实例的constructor就 为改变的值

正常的开发，就算constructor被修改 了，也应该及时修正，这样方便维护
手动在构造函数原型下加一个constructor属性，并修改他的指向
```

```
 function Fn(name){
       this.name = name;
   }
   Fn.prototype = {
       constructor:Fn, //默认指向实例化构造函数
       a:10,
       b:18,
       fn:function(){
           console.log(this.name); //索明珠
       }
   }
   let f = new Fn('索明珠');
   f.fn();
   console.log(f.constructor);
```
> ###### instanceof

```
检测某一个实例是否属于这个类 //返回布尔值

function fn(){
       console.log(this);
    }
    fn();
    let f = new fn();
console.log(f instanceof fn);       //true
console.log(f instanceof Array);    //false
console.log(f instanceof Object);   //true

```


> #### Js中内置对象
- 类分为内置类和自定义类

```
String：字符串类
Number：数字类 （NaN是他的一个实例）
Boolean：布尔类 （true/false是它的实例）
Null/Undefined：浏览器屏蔽了我们操作Null或者Undefined这个类
Function：函数类
Object：每一个对象数据类型都是它的实例
        Array 数组对象
        Math  math对象
        Date 日期对象
        RegExp 正则对象
```
> #### 函数的三种角色
- 函数

```
1、执行函数体代码
2、return 返回值

function fn(){
    函数体      //整理功能的一个容器，主要为了复用
}
```
- 类（构造函数）

```
function Person(name){
    this.name = name;
    创建对象
}
```
- 实例化对象

```
函数都是由大写Function构造的
function (){} ->  new Function()
```


> #### 原型 Prototype
- 创建一个函数时候，函数自身会有一些属性和方法（包括属性prototype）
- 原型主要解决性能问题，复用性高
- 一个函数既有原型也有原型链

`原型可以比作为 css中的class
普通方法可以比作为css的style`


```
原型的方法可以实例用f1.say(),this就是实例

可以直接用 fn.prototype.say(),this 是Fn的prototype
```

```
*** 当前构造函数的原型，只有实例化对象可以使用

（实例化对象没有，会去构造函数的原型下找）
```

```
    function Person(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    Person.prototype.say = function(){
        alert(this.name);
    }

    let p1 = new Person('王苗苗',18,'女');
    let p2 = new Person('杨雅婷',18,'女');

    console.log(p1);//Person{name:'王苗苗',age:18,sex:'女'}
```


> #### 原型链  

- js中所有的对象实例都有_proto_ 这个属性
- 实例化对象只有原型链没有原型（函数除外）

`两层意思：_proto_一个指的是属性，一个是指的链这种关系`
```
函数：
函数不但是函数还是Function实例化对象，所以他身上既有原型也有原型链
函数的原型只给他的实例化对象用
```
- 什么时候会走原型链

```
当前使用这个对象的属性或方法，自身没有会走原型链。通过原型链找到构造函数的原型
```
- 原型与原型链之间的关系


```
*** 构造函数的原型 === 实例化对象的原型链_proto_

先看当前对象下没有，如果没有就通过对象原型链 去构造函数的原型找，
如果还找不到就从构造函数的原型链上查找就等于Object.prototype
```
查找规律

```
1、先找自身数据
obj.xx = '数据';
//fn是构造函数，并且return是当前这个实例
function fn(){
    this.xx = 数据
}
2、如果找不到数据就找对象的原型链（等同于构造函数的原型）
3、如果还找不到数据就会找构造函数原型的原型链（等同于Object.prototype）
4、如果还没有就真没有了（报错或undefined）,因为Object.prototype没有原型链
```
> #### 面向对象
1. 抽象 --- 抓取核心问题；（将具有相同的进行，统一管理）；
2. 封装 -- 只能通过对象的方式使用，例如：push()方法只能由数组使用；
3. 继承 -- 从已有的对象上继承处新的对象，并且新的对象不会影响原有对象；
4. 多态 -- 多对象的不同形态，利用一个接口支持不同的设备；

`特点：类的原型下挂方法、进行归类`
```
把具有相同特征的代码提取出来归为一类，把描述这个类的细节、功能挂在原型上的一种开发模式
```

```
在面向对象中，了解this指向十分的重要，首先最简单的情况，如果this所在函数前面没有'.',那么就是指向window,(new 是个例外)
```

> #### 面向对象的继承

```
继承：子类继承父类的属性或方法，自己也有自己的一套属性或方法
```
- 面向对象的写法：属性挂在继承里，方法挂在原型下
- 属性使用类式继承，方法使用原型继承
> ###### 类式（call）继承
- 一般类式继承是继承（私有）属性
- 调用父类，通过call改变子类this的指向
```
 //类式继承
 
    function Person(name,age){
        this.name = name;
        this.age = age;
    }
   function Coder(name,age,job){
       Person.call(this,name,age);  //调用父类 Person,通过call改变this指向
       this.job = job;
       console.log(this); //Coder
   }
   let p = new Person('小明',20);
   let c = new Coder('小红',18);
   Coder(); //window
```
> ###### 拷贝继承 （性能不好）
- 把一个对象中的属性或者方法直接复制到另一个对象中
- 使用for(let attr in 父类.prototype)
- 遍历父类身上的方法，只要是自身的就赋值给子类的原型

```
拷贝继承：子类通过for in继承父类的方法（父类有啥都继承过来）
然后子类新加的方法，父类身上不会有
```

```
function Person(name,age){
        this.name =name;
        this.age = age;
    }
    Person.prototype.say = function(){
        console.log('我叫'+this.name);
    }
    Person.prototype.runing = function(){
        console.log('跑得快');//父级还是跑得快，不会改变
    }
    function Coder(name,age,job){
        Person.call(this,name,age);
        this.job = job;
    }
    for(let attr in Person.prototype){ //for in拷贝父级的原型
        if(Person.prototype.hasOwnProperty(attr)){ //如果父级有这些方法
            Coder.prototype[attr] = Person.prototype[attr];//就让父类复制一份给子类
        }
    }
    Coder.prototype.runing = function(){
        console.log('跑得飞快'); //子级跑得飞快
    }
    let p = new Person('小黄',22);
    let c = new Coder('小蓝',20);
  
    c.runing();
    p.runing();
    console.log(Coder.prototype);//{say: ƒ, runing: ƒ, constructor: ƒ} 
    //父级的say runing方法 都继承过来了
```
> ###### 原型继承

- 原型继承主要是 继承父类原型上的 属性或者方法

```
1、创建一个空的构造函数
2、把空构造函数的原型等于父类的原型
3、把子类的原型等于new空函数

这样就到达了 继承属性的目的，（需要手动修改constructor指向）
```

```
     function Person(say,name,age){
        this.say = say;
        this.name = name;
        this.age = age;
        console.log(this);
    }
    Person.prototype.runing = function(){
        alert(this.name+'会跑');
    }

    function paohui(){}
    paohui.prototype = Person.prototype;
    Coder.prototype = new paohui;

    function Coder(say,name,age){
        Person.call(this,say,name,age);
    }
    Coder.prototype.constructor = Coder; //手动修正构造函数指针

    Coder.prototype.runing = function(){
        alert(this.name+'脚踏七彩云');
    }
    let p1 = new Person('大渣好','渣渣辉',40);
    let c1 = new Coder('我是程序猿','abc',12);
    console.log(c1 instanceof Person); //继承成功了
```

```
1、只有赋址之后，两个对象地址为同一个，修改一个才会影响另一个
2、实例化对象没有会去他的构造函数的原型在找
3、实例化对象不等于构造函数的原型（因为不是一个地址，但是又能通过第二句的关系找到）
```


> ###### 寄生式组合继承
- 使用object.create()
-  把父类的原型丢进去

```
object.create：内置Object类天生自带的方法
1、创建一个空对象
2、让新创建的空对象的_proto_指向第一个传递进来的对象

Object.assign():
将多个对象的可枚举性属性拷贝到目标对象上，并且返回赋值后的目标对象

// Object.assign({},{key:val})
//Object.assign(Coder.prototype,Person.prototype)

Object.defineProperties()
给对象定义属性，如果存在该属性，
则用新定义的属性更新已存在的属性，
如果不存在该属性，则添加该属性。
```

```
子类的原型 = Object.create(父类的原型);
在子类原型的原型链上加了父类的原型
Coder.prototype = Object.create(Person.prototype);
```


> ###### ES6继承
- ES6给我们提供了class的语法糖，可以通过class来创建类
- 通过extends来继承
- class 子类 extends 父类

```
具体语法

     class 类名 {
         constructor(形参){  
             //构造方法
        }
        方法名1(){
            
        }
        //中间不用加逗号，加了会报错
        方法名2(){
        }
    }
```

```
        定义方法:
            动态方法:
                这样的写法就等同于把方法挂在类的原型上了
                方法名(){

                }

                实例使用的方法
            
            静态方法:
                static 方法名(){

                }

                类使用的方法
```

```
class Person {
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        static say(){
            console.log('静态方法');
        }
        say(){
            console.log(this.name);
        }
        runing(){
            console.log('会跑');   
        }
    }

    let p = new Person('小明',20);
    p.say();
    // p.runing();

    Person.say();

```
`es6继承`

```
在继承里写了constructor就必须写super
super下方才能使用this
super上方有暂存死区（super上面不能使用this，用了就报错）
super(...arg);//等同于调用父类，把多余的参数(和父类一样的属性)放到super中，达到继承父类属性的目的
```

```
class Person(){
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    say(){
        console.log('姓名：'+this.name,'年龄：'+this.age);
    }
}
class Coder extends Person(){ //子类 extends 父类实现继承
    constructor(job,...arg){//job放在前面，父类放在后面就可以直接用...arg
        super(...arg);//等同于调用父类，把多余参数放在super中达到继承父类属性的作用
        this.job = job;
    }
    codeing(){
        console.log('多敲代码');
    }
    say(){
        console.log('恩恩');
    }
}
let p = new Persno('大人',40);
let c = new Coder('学生','小孩',20); "学生"}
c.codeing();//多敲代码
c.say();    //恩恩
console.log(p);//{name: "大人", age: 32}
console.log(c);//{name: "小孩", age: 18, job:
```
```
在子类constructor中添加属性的小技巧：
专属于子类的属性写在参数的前面，父类的属性放在参数后面
这样就可以直接使用...arg
比如 constructor(job,...arg)
```


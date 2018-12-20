> #### npm
- Node Package Manager
- JavaScript的包管理工具
- Node.js 平台的默认包（模块依赖）管理工具

> #### nrm
- 一个npm的源管理器（管理工具）
- 允许快速的在 npm 源间切换

> ###### 两者关系
```
npm是Js的包管理器，通过npm安装你所需要的项目包。
但是有时候会下载不了，所以就需要用nrm修改下npm的下载源

```
顾名思义

```
npm是管理包，nrm用来管理npm
```
> ###### 命令
`带 * 的是当前使用的源，上面的输出表明当前源是官方源`

```
安装nrm
npm install nrm -g
```


```
增加源：nrm add
```
```
删除源：nrm del
```

```
测试速度：nrm test （测试源的相应时间）
```
```
查看可选的源：nrm ls
```

```
将npm切换指定的源：nrm use [name]
    比如taobao：nrm use taobao
    //Registry has been set to: http://registry.npm.taobao.org/
```

```
跳转到指定源的官网：npm home [name]
```

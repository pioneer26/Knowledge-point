### 版本控制工具

---
> ###### Git (Linux开发)

- git相对于svn最大的优势就是不需要网络即可进行版本控制
- 记录当前产品代码的所有版本信息（历史修改信息）



```
GIT:

1. 分布式版本控制
2. 不需要网络，本地就可以操作版本控制
3. 可以是公用的，可以分享
4. 不依赖于中央服务器，即使服务器有问题也不会有影响
5. 传输方式不一样，git要比svn快很多
6. 可以与github连接，功能更强大

```
`git缺点`

```
1、git没有严格的权限管理控制，一般通过系统文件读写权限的方式就可以做权限控制
2、git工作目录只能是整体项目。比如 checkout，建分支，都是基于整个项目的。
svn可以基于项目中的某一个目录
3、GIT没有一个全局的版本号，而SVN有

```

> ###### Svn

```
SVN:

1. 集中式版本控制
2. 需要联网，一旦断网将不能进行版本控制
3. 基本是公司内部才能访问，网外不方便访问
4. 非常依赖中央服务器，一旦服务器有问题，都会收到影响

```
---
###### github

> 一个存储代码，私有的域名空间网站

> 是一个大型的中央服务器，可以进行代码的远程仓库管理。可分配一个免费的静态页面域名

1. 申请github账号
2. 每天计算机中都需要秘钥




```
获取秘钥：
ssh-keygen -t rsa -C "注册的邮箱账号"
github->settings->SSH and GPG keys 进行绑定
ssh -T git@github.com 只要有“hi,用户名” 代表绑定成功
永久免密：
git config --global credential.helper store
```

```
设置贡献者信息：
git config --golbal user.name"名字""
git config --global user.email"邮箱"
git config --list 查看配置

//配置的名字和邮箱建议和github的保持一致
```
```
查看版本信息
git --version
```
能不能版本控制，关键看是否有.git文件
```
如何创建项目（版本）
1、git init(无网络情况下)
2、在github中创建项目（记得勾选readme 阅读协议）
```

```
cd 路径进入目录
cd .. 回退上层目录
进入盘符 cd c:
进入文件夹 cd 文件夹名字

```

```
git clone 克隆项目地址
```

```
ls || ll 查看下级目录有哪些
```

```
git status 查看文件状态（处于哪个区域）
红色代表当前处于工作区（还没有提交暂存区）
绿色代表当前处于暂存区（还没提交版本区）
无代表工作区，暂存区，版本区已经同步，历史版本已生成（该提交的都提交）
//暂存区始终存放提交的内容，并没有消失。
//以后工作区内容修改，会和暂存区比较，以此来判定哪些是最新处理更新的
```

```
工作区：编辑代码的地方
暂存区：临时存储要生成版本的地方
版本区：存储生成的每一个版本代码
```

```
工作区到暂存区
git add 文件名
git add . 批量传输
git add -A 批量传输
```

```
暂存区到版本区
git commit -m "注释的信息"
git commit 这样操作会出现提交文本输入界面，需要我们编写提交到版本区，给当前版本编写备注信息

1、按i进入编辑、插入模式
2、输入备注信息。比如 这是轮播图
3、按ESC
4、输入 :wq
```

```
快速从工作区到版本区（先要走一遍工作区到暂存区再到版本区）
git commit -a -m "注释"
```

```
查看当前版本信息
git log
git reflog 所有的操作的记录
```

```
撤销
git reset --hard 版本号
```

```
退出： q
```

```
上传
git push origin master
```

```
查看每个区域之间的差异：

git diff           //工作区查看暂存区
git diff --cached //暂存区查看版本区
git diff master  //工作区查看版本区
```


```
看github课件：

1. 先找到github的人名和项目，

2. 然后fork，再通过git clone 地址(第一次)

3. 要fork的项目每天都会变，他的项目变，你的fork的项目也得变，这样才能达到项目更新。
	
```

```
更新github课件：
1、先把fork过的项目删除（进入项目选择settings，最底下点击delete this repository删除）
2、重新fork项目
3、跑到之前克隆过的项目，打开git 输入git push(时间需等会) 即可
```

> ###### .gitignore

```
在当前目录（git仓库根目录）创建一个.gitignore文件。
这个文件存储了当git提交时候忽略的文件
```

```
创建.gitignore文件 命令：touch.gitignore
```

> ###### git和gitHub同步 
- 建立关联
```
git remote -v 查看和哪些有关联

git remote add 名字 [远程仓库git地址]
```
- 移除关联

```
git remote remove 名

```
- 拉取（下载）信息
```
git pull origin(默认名称和远程仓库关联的这个名字) master,从远程仓库的master分支拉取最新信息
```
- 将本地信息打包推到github远程仓储上
```
git push origin master

如果名字是origin,分支走的也是master分支，后面都可以不写。
比如 git pull 
     git push
```

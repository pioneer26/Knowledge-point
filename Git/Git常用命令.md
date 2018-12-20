> ## git常用命令

> ###### git与github进行连接

```
获取秘钥：
ssh-keygen -t rsa -C "注册github用的邮箱"
检测是否绑定：
ssh -T git@github.com
```
> ###### 设置贡献者的信息

```
git config --global user.name "用户名"
git config --global user.email "xx@.com"
git config --list 查看配置信息
```
linux命令

```
打印工作目录路径
git pwd (print working directory)
```




> ###### 创建项目
能不能版本控制，关键是看有没有.git的文件

```
1、git init
2、通过github创建项目
（+号里面->import repository）
```
```
打开git:
git clone 项目地址
```

```
粘贴命令:
ctrl+c
shift + insert
鼠标右击设置git的选项
```
> ###### 工作命令
- 查看版本信息

```
git version
```
- 查看文件状态

```
git status

红色代表当前处于工作区（还没有提交暂存区）
绿色代表当前处于暂存区（还没提交版本区）
无代表工作区，暂存区，版本区已经同步，历史版本已生成（该提交的都提交）
//暂存区始终存放提交的内容，并没有消失。
//以后工作区内容修改，会和暂存区比较，以此来判定哪些是最新处理更新的
```
- 工作区到暂存区

```
git add 文件名字
git add . 多个文件批量操作
git add -A多个文件批量操作
```
- 暂存区到版本区

```
git commit -m "注释信息"
//这里的注释主要是为了方便管理员查找、操作

git commit 这样操作会出现提交文本输入界面，需要我们编写提交到版本区，给当前版本编写备注信息
1、按i进入编辑、插入模式
2、输入备注信息。比如 这是轮播图
3、按ESC
4、输入 :wq
```
- 快速从工作区到版本区（前提文件添加过）

```
git commit -a -m "注释"
```
> ###### 查看每个区域之间的差异

```
工作区查看暂存区：
git diff

暂存区查看版本区：
git diff --cached

工作区查看版本区：
git diff master
```

> ###### 拉取和上传


- 把版本区文件上传到远程仓库

```
git push origin master
origin //默认名称和远程仓库关联的这个名字
master //从远程仓库的master分支拉取最新信息
```
- 将远程仓库的文件拉取/下载下来

```
git pull origin master
```
> ###### cd命令
- 路径进入目录

```
c
```
- 进入盘符
```
cd c:
```


- 进入文件夹

```
cd 文件夹名字
```
- 回退上层目录

```
cd ..
```
- 查看当前目录下的信息

```
ll 或者ls
```

```
输入一些关键字可以按tab键自动补全
git log||git diff....的时候回退不了此时使用:q键回退
```
- 清除屏幕

```
clear
```



> ###### 版本回退

```
git reset --hard 版本id

获取版本id（查看当前版本信息）
    git log
    git reflog //所有操作的记录
```
> ###### 永久免密

```
git config --global credential.helper store
// 克隆的文件不需要修改，如需修改需要重新复制一份到别的目录中
```

> ###### git/github关联
- 查看所有关联信息
```
git remote -v
```
- 建立关联

```
git remote add [远程仓库git地址]
```


- 移除关联

```
git remote remove [远程仓库git地址]
```
> ###### 分支
Linux命令-创建文件

```
touch 文件名（如1.js）
```

查看git分支数量

```
git branch //后面不加东西代表查看
```
创建git分支

```
git branch 分支名 //后面加东西代表创建
```


切换分支

```
git checkout 分支名 //后面加分支名称
```
创建并切换分支（快捷）

```
git checkout -b 分支名
```
当文件修改时切换分支

```
git stash 暂存文件
//分支有更改不能直接切换
//可以提交更改或暂存更改
//暂存使用过渡区覆盖掉工作区

```
还原暂存的内容（用的很少）

```
git stash pop
```

删除分支

```
git branch -D 分支名//D就是delete
```

```
删除分支时候，当前用户不能待在要删的分支下，需要切换到master后再删除

//比如某某某在屋里睡觉，
//这时候要拆迁房子，你得出去之后才能再拆房子
```

合并分支（重要）
- 先创建主干，在主干基础上添加一个分支
- 在分支上进行提交，切换到主干 合并分支

```
git merge //合并分支
```

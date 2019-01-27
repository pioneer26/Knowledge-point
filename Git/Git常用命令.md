> ## Git常用命令

> #### git与github进行连接

```
获取秘钥：
1、ssh-keygen -t rsa -C "注册github的邮箱账号"
2、C盘去找.ssh-->id_rsa.pub找到秘钥
3、在github上->设置settings->SSH and GPG keys 进行与git绑定
4、ssh -T git@github.com 只要有“hi,用户名”代表绑定成功

检测是否绑定：
ssh -T git@github.com

永久免密：
git config --global credential.helper store
//克隆的文件不需
修改，如需修改需要重新复制一份到别的目录中
```


> #### 设置贡献者的信息

```
git config --global user.name "用户名"
git config --global user.email "xx@.com"
git config --list 查看配置信息
//配置的名字和邮箱建议和github的保持一致
```

> #### cd命令
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


> #### 创建项目
能不能版本控制，关键是看有没有.git的文件

```
1、git init(无网络情况下)
2、通过github创建项目（记得勾选readme 阅读协议）
（+号里面->import repository）
/*
1)点击github右上角的+里面的new repository 
2)在repository name栏中输入仓库的名称
3)description 栏中可以设置仓库的说明
4)piblic（公开）通用 private（非公开）收费
5)勾选Initialize this repository with a README
6)点击create repository按钮，完成仓库的创建
*/
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

> #### 工作命令
- 查看版本信息

```
git version
```
- 查看当前历史记录（版本id）

```
git log

git reflog //查看所有的操作记录
```

- 查看文件状态

```
git status

红色代表当前处于工作区（还没有提交暂存区）
绿色代表当前处于暂存区（还没提交版本区）
无代表工作区，暂存区，版本区已经同步，历史版本已生成（该提交的都提交）
/*
暂存区始终存放提交的内容，并没有消失。
以后工作区内容修改，会和暂存区比较，以此来判定哪些是最新处理更新的
*/
```

`工作区：编辑代码的地方`

`暂存区：临时存储要生成版本的地方`

`版本区：存储生成的每一个版本代码`
- 工作区到暂存区

```JavaScript
git add 文件名字 //单个文件
git add . 多个文件批量操作
git add -A多个文件批量操作
```
- 暂存区到版本区

```JavaScript
git commit -m "注释信息"
//这里的注释主要是为了方便管理员查找、操作

git commit 这样操作会出现提交文本输入界面，需要我们编写提交到版本区，给当前版本编写备注信息
1、按i进入编辑、插入模式
2、输入备注信息。比如 这是轮播图
3、按ESC
4、输入 :wq
```
快速从工作区到版本区（前提文件提交过）

```
//先要走一遍工作区到暂存区再到版本区
git commit -a -m "注释"
```

> ###### 拉取和上传


- 把版本区文件上传到远程仓库

```JavaScript
git push origin master
//origin 默认名称和远程仓库关联的这个名字
//master 从远程仓库的master分支拉取最新信息
```
- 将远程仓库的文件拉取/下载下来

```
git pull origin master
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


> #### 撤销（版本回退）

```JavaScript
第一种情况
git reset HEAD 文件名
git reset --hard 版本id
//可以将暂存区文件撤销到工作区（绿色变红色）

第二种情况
git checkout -- 文件名
//工作区还原到暂存区或版本区
//(有暂存区就回滚暂存，没有就回滚版本区)

第三种情况
git commit -m “注释” --amend
/*
如果有2个以上文件，一个提交到版本库了，另一个忘记提交
可以先将没提交的文件拉到暂存区
然后通过git commit -m “注释” --amend 撤销回来
最后自动一次性提交暂存区中的文件和撤销回来的版本形成一个新版本
撤销回来的版本就销毁了，然后git log查看是否提交成功
*/
```
> #### 删除

```
git rm 文件名
//如果手动删除了工作区的文件，也想在git中把它删除

git rm -f 文件名
//一次性把工作区与暂存区的文件都删掉

git rm --cached 文件名
//只删除暂存区，不删除工作区

git rm -rf 文件名
//删除整个文件夹
```
> #### 恢复

```
恢复一个文件

git checkout 历史记录编码 文件名
//指定的文件还原

/*
    其中历史记录编码是唯一的
    文件名是要恢复的文件名
*/

git reset  --hard 历史记录编码
//还原整个版本

git reset --hard HEAD^(上一个)
//回滚版本记录
//输出第一次为最近的记录，第二次就是倒数第二个记录

git reset --hard HEAD~2
//回滚倒数第三个历史记录 （跳过了2个）
/*
    以当前版本，后退几个版本。比如当前是6
    git reset --hard HEAD~2
    等同于 6-2=4返回到4这个版本
*/

//*** 如果说git log找不到历史ID
//可以通过git reflog去查看操作过的历史记录
```

> #### git/github关联（同步到远程仓库）
- 查看远程仓库名
```
git remote (默认origin)
```
- 查看上传地址和下载地址是否为一个地址

```JavaScript
git remote -v
```

- 创建远程仓库

```
git remote add [远程仓库git地址]
```

- 上传远程仓库

```
git push origin(默认) master
```

- 移除关联

```
git remote remove [远程仓库git地址]
```
> #### 协作 （有权限）
需要创建项目者给协作者权限
```
进入项目 -> settings -> 
最左边 Collaborators -> 添加协作者名字
```
等待协作者确定

```
...
```
协作者

```
1、确定协作
2、clone项目
3、参与项目开发
4、提交上传
```
> ###### 有可能会遇到的冲突

```
git pull [直接把远程的代码覆盖到本地] //不推荐
git fetch [把远程仓库的代码拉取下来不会覆盖]//推荐
```
- 查看哪里有冲突
```
git diff master origin/master
```
- 合并冲突

```JavaScript
git merge origin/master  //你会发现master变成了matser|MERGEING

/*
    人为判断冲突，把冲突内容删除
    删除完成之后（代码被修改了，需要重新提交）
    保存到版本区之后，继续push
    （matser|MERGEING就变成了master）
*/
```
> #### 没有权限协作

```
1、找到想参与的项目，然后forx
2、把项目克隆到本地，然后修改->提交
3、点击Pull requests
4、点击 new pull request
5、点击 Create pull request
6、对话点击Create pull request
7、等待
```
- 收到修改消息

```
1、点击修改的消息
2、点击files changed（查看修改内容）
3、merge pull request(可以回复别人)
```

> #### 分支
Linux命令-创建文件

```
touch 文件名（如1.js）
```
创建git分支

```
git branch 分支名 //后面加东西代表创建
```
查看git分支数量

```
git branch //后面不加东西代表查看
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

删除未合并的分支

```JavaScript
git branch -D 分支名//D就是delete
```
删除已经合并的分支

```
git branch -d 分支名
```

```
删除分支时候，当前用户不能待在要删的分支下，需要切换到master后再删除

//比如某某某在屋里睡觉，
//这时候要拆迁房子，你得出去之后才能再拆房子
```
查看已经合并的分支

```
git branch --merged
```
查看未合并的分支

```
git branch --no-merged
```

合并分支（重要）
- 先创建主干，在主干基础上添加一个分支
- 在分支上进行提交，切换到主干 合并分支

```
git merge //合并分支
```

```
//人为修改文件 -> 提交，可能会出现冲突
```

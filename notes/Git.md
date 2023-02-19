# Git基础操作
1. git的流程就是 先本地操作，然后在提交给暂存区，然后给仓库，然后给远程
2. 本地文件的每一步改变，都要再次提交给仓库，仓库也有再次提交给远程
3. 仓库的每一次改变，包括删除文件，都要提交给仓库一次
4. 目的都是保证 仓库，远程，本地是逻辑上统一的，除非你本地不想让仓库看，但是，仓库一定是和远程统一的
5. 也就是说，一切都是本地操作，一损俱损，想给仓库或者远程赠增删改查，就要先改变本地，然后一步步提交上去
## 获取帮助信息

git help < verb >
git config -h

## 创建git仓库
>会创建一个名为 .git 的隐藏目录，这个 .git 目录就是当前项目的 Git 仓库，里面包含了初始的必要
文件，这些文件是 Git 仓库的必要组成部分。

git init 

## 查询文件状态
git status
git status -s

## 提交到暂存区
git add < verb >    可以用它开始跟踪新文件  把已跟踪的、且已修改的文件放到暂存区
git add .        一次性将所有的新增和修改过的文件加入暂存区

## 取消暂存区文件
git reset < verb >


## 提交到仓库
git commit -m ” 描述消息“        提交暂存区文件到仓库
git commit -a -m ”描述消息“    可以把工作区文件提交到暂存区

## 恢复工作区文件
>把对工作区中对应文件的修改，还原成 Git 仓库中所保存的版本

git checkout -- < verb > 命令

## 移除仓库文件
1. git rm -f < verb >    从仓库和工作区同时移除
2. git rm --cached  < verb >   只删除git仓库文件，保留工作区文件
3. git rm --cached -r 文件名   删除文件夹，包括递归文件夹  保留工作区文件

## 忽略文件
创建一个.gitignore的配置文件，在配置文件里，添加要忽略的文件，可以用glob模式简化正则表达式

## 查看提交仓库历史
git log  显示最近记录
git log -2  显示最进提交2个记录


## reset回退 和 revert撤销

在利用git实现多人合作程序开发的过程中，我们有时会出现 **错误提交** 的情况，

此时我们希望能撤销提交操作, 或者想要回退到某个版本

1. git log  --pretty=online   在一行上显示所有提交历史，查找提交ID
2. reset => 回退到某个版本 `git reset --hard 版本号`
3. evert => 撤销某个版本内容的内容修改 `git revert -n 版本号`

博客参考: [https://blog.csdn.net/yxlshk/article/details/79944535](https://blog.csdn.net/yxlshk/article/details/79944535 "https://blog.csdn.net/yxlshk/article/details/79944535")

如果git reset 后, 版本回退了, 无法直接push 到远程仓库(因为远程仓库版本更加新) => git push -f 覆盖推送即可
效果: 将远程仓库的版本, 也进行了回退
# Git本地分支
## 主分支
>master主分支，git默认帮忙创建的。一般不会直接操作主分支，用来保存和记录整个项目已完成的功能代码。
## 功能分支
>专门用来开发新功能的分支,从主分支上临时分出来，当任务介绍后再合并在主分支上

## 查看分支列表
git branch  查询仓库所有分支列表
带 * 号为当前分支

## 创建分支
>创建的分支不会自动切换到新分支，要手动切换

git branch 分支名称   基于当前分支创建新分支，新分支代码和当前分支一样


## 切换分支
git checkout 分支名称   

### 快速创建切换分支
git checkout -b  分支名称    创建新分支，同时自动切换为新分支

## 删除分支
* 当代码合并之后，老的分支就没有用了，要删除
git branch -d 分支名称

# Git远程分支
## 本地分支推送远程仓库
* 第一次推送加 -u  以后可以不加
git push -u 远程仓库别名 本地分支名称:远程分支名称    (推送本地分支，并创建不同名称的远程分支)
git push -u 远程仓库别名 本地分支名称    （推送本地分支，并创建名称一致的远程分支）


## 远程分支推送本地仓库
git checkout 远程分支名称
git checkout -b 本地分支名称 远程仓库名称/远程分支名称

## 查看远程分支列表
git remote show 远程仓库名称
git remote -v

## 拉取远程分支最新代码
git pull （拉取最新代码，保持当前分支与远程分支一致）

## 删除远程分支
git push 远程仓库名称 --delete 远程分支名称

## 修改远程仓库地址
1. 直接修改远程仓库地址

```javascript
git remote set-url origin 复制上仓库地址
```

2. 先删除再修改地址

```javascript
git remote rm origin
git remote add origin 复制上仓库地址
```

>

## 合并分支
* 合并分支一定要在主分支上进行
1. 切换到主分支 git checkout master
2. git merge 要合并分支名称    

### 合并分支冲突
1. git checkout master
2. git merge 分支名称
3. 打开冲突的文件，手动确认，此时文件变为已修改文件
4. git add .   添加文件到暂存区
5. git commit -m "描述消息"


# GIthub
1. 创建本地仓库，git init
2. 创建ssh秘钥，ssh-keygen -t rsa -b 4096 -C "github创建邮箱"     
3. 生成ssh秘钥,  存放在用户-用户名-.ssh里  --然后配置github
4. 检测是否成功  ssh -T git@github.com
5. 保证仓库里面有提交的信息，也就是保证仓库与工作区同步过
6. git remote add 远程仓库名 git@github.com:S-Huan/shaohuan_De_notes.git
7. git push -u 远程仓库名 远程分支名

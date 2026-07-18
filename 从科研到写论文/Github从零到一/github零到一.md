# github零到一



## 如何在VsCode上直接对自己的仓库处理

### 克隆仓库

在 VS Code 中打开终端（Ctrl+`），直接用 Git 命令操作：

```bash
# 克隆你的仓库到本地（如果还没有）
git clone https://github.com/你的用户名/仓库名.git
	比如:git clone https://github.com/Star-hhy/the-road-to-academic-self-rescue.git

cd 仓库名

#注意：如果已经克隆过了，会出现"fatal: destination path 'the-road-to-academic-self-rescue' already exists and is 	not an empty directory."，表示已经存在相同命名的文件夹
 	#处理1：直接打开之前已经克隆过的文件夹
 	cd the-road-to-academic-self-rescue
 	code .
 	#处理2：重新命名一个名字
 	git clone https://github.com/Star-hhy/the-road-to-academic-self-rescue.git 科研自救2.0
```

### 本地和远程仓库的交互

#### 本地

Git文件在本地分为三种状态：

**工作区**：你电脑里改完的代码文件（未被 Git 记录）

**暂存区**：`git add` 把改动放这里，准备打包提交，完全看不到，仅本地临时缓存，挑选要提交的文件。

**本地仓库**：项目隐藏文件夹 `.git`，`commit` 生成永久版本快照，硬盘存储，关机不丢

```bash
#只操作本地
git add .           # 暂存你的改动------只把改动移入暂存区，不会生成版本、不会上传 GitHub.（临时，关机 / 重置会丢失）

git commit -m "xx"  # 提交你的改动,生成版本快照（此时改动已安全保存）
					#把刚刚git add .放到暂存区里的文件都copy一份，放到本地仓库.git中(关机不会消失)
					#xx为这次修改的内容是什么，自己来写，方便团队合作
```





#### 远程交互

```bash
git pull            # pull是唯一主动拉取远程 GitHub 代码到本地的命令，同步远程更新（不会覆盖你的 commit）
git push            # 推送
```



##### git pull

从GitHub远程仓库拉取最新的版本快照，合并到本地仓库(.git)，自动更新本地的工作区。(暂存区不受影响)



```
git pull = git fetch + git merge
```

1. `git fetch`：去 GitHub 远程仓库下载本地没有的所有 commit、代码，只下载，不合并到你本地代码；
2. `git merge`：自动把下载下来的远程新版本，合并到你当前本地分支。

```bash
当远程仓库和本地仓库的同时对一个文件同一部分进行不同修改时候的处理：
git pull 触发冲突，命令终止；
原文件写入冲突标记，Git 标记文件冲突；
手动删除标记，编辑保留最终代码；
git add 冲突文件 标记冲突已解决；
git commit -m "解决合并冲突" 完成合并提交。

git merge --abort 恢复到pull之前的状态
```



##### git push

`git push` 将本地已经固化好的 commit 版本上传到 GitHub 远程；如果远程有别人新提交，会直接推送失败，需要先拉取合并再推送。

- 在上传到远程仓库(`git push`)之前，要先确保，远程仓库的版本不能领先于本地仓库。(即不能本地修改了，远程仓库也修改)
  务必先git push拉取——本地先同步远程

  >比如，远程偷偷改了readme文件，而我这里的readme文件落后于远程，我的本地要先同步一下远程仓库，然后再去提交我修改的其他新东西。没有`git pull`就`git push`会出现以下问题：

<img src="assets/image-20260717214226171.png" alt="image-20260717214226171" style="width:43%;" />

## Git颜色区别

| 颜色        | 状态           | 含义                                      | 对应操作                    |
| ----------- | -------------- | ----------------------------------------- | --------------------------- |
| 绿色        | A（Added）     | 全新文件，未被 Git 跟踪 / 已 git add 暂存 | 刚新建文件，执行过`git add` |
| 黄色 / 浅蓝 | M（Modified）  | 文件已修改，未暂存                        | 改了代码，没执行`git add`   |
| 红色        | D（Deleted）   | 文件被删除                                | 本地删掉文件，未提交        |
| 蓝色        | U（Untracked） | 完全未被 Git 管理的新文件                 | 新建文件，没`git add`       |
| 灰色        | Ignored        | 被`.gitignore`忽略的文件                  | 不会被 git 追踪             |
| 白色        | Clean          | 无任何改动，和仓库版本一致                | 无操作                      |







## Git常见指令

#### git status

查看工作区、暂存区当前所有改动状态，判断代码到了哪一步。



改代码，没 add：status 显示红色「modified」

git add . 后：变成绿色，属于待提交

git commit 后：干净，无任何变更提示

pull 产生冲突：status 标出冲突文件，提示需要解决、add、commit




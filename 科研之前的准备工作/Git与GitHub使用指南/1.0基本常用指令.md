# Git + GitHub 完整操作指南（新手版）

---

## 📌 你的项目信息

| 项目 | 值 |
|------|-----|
| GitHub 用户名 | **Star-hhy** |
| 仓库名 | **the-road-to-academic-self-rescue** |
| 仓库地址 | `https://github.com/Star-hhy/the-road-to-academic-self-rescue.git` |
| 本地路径 | `D:\githubProject\the-road-to-academic-self-rescue` |
| 默认分支 | `main` |

---

## 📖 第一部分：基础概念

### 什么是 Git？
Git 是一个**版本控制工具**，用来记录文件的每一次修改。你可以把它理解成游戏的"存档"——每做一点修改就存一次档，出了问题随时可以读档回到之前的状态。

### 什么是 GitHub？
GitHub 是一个**代码托管网站**，你可以把本地的 Git 仓库上传到 GitHub，相当于把存档备份到了云端。这样：
- 换电脑也能接着工作
- 可以和别人协作
- 不怕本地文件丢失

### 关键概念

| 概念 | 通俗解释 |
|------|----------|
| **仓库 (Repository)** | 一个项目文件夹，包含所有文件和修改记录 |
| **克隆 (Clone)** | 把云端仓库下载到本地 |
| **暂存 (Stage)** | 告诉 Git "这些文件我准备存档了" |
| **提交 (Commit)** | 正式存档，给当前状态拍一张快照 |
| **推送 (Push)** | 把本地的存档上传到 GitHub |
| **拉取 (Pull)** | 把 GitHub 上别人的存档下载到本地 |
| **分支 (Branch)** | 一条独立的开发线，默认分支叫 `main` |

### Git 工作流程示意图

```
[修改文件] → [git add 暂存] → [git commit 提交到本地] → [git push 推送到GitHub]
                                                    ← [git pull 拉取别人的修改]
```

---

## 🚀 第二部分：从零开始的完整流程

### 步骤 1：克隆仓库（把云端代码拉到本地）

打开终端（在 VS Code 中按 `` Ctrl+` `` 或 `Ctrl+Shift+` `` 打开）：

```bash
# 先进入你想存放项目的目录
cd D:\githubProject

# 克隆仓库
git clone https://github.com/Star-hhy/the-road-to-academic-self-rescue.git
```

**⚠️ 注意：** 如果提示连接超时（国内常见问题），需要配置代理：

```bash
# 使用代理克隆
git clone https://github.com/Star-hhy/the-road-to-academic-self-rescue.git -c http.proxy=http://127.0.0.1:7890
```

克隆完成后，你会看到一个新的文件夹 `the-road-to-academic-self-rescue`，这就是你的本地仓库。

---

### 步骤 2：在 VS Code 中打开仓库

在 VS Code 中：
- 按 `Ctrl+K, Ctrl+O`（打开文件夹）
- 选择 `D:\githubProject\the-road-to-academic-self-rescue`
- 点击"选择文件夹"

或者直接在终端输入：
```bash
code D:\githubProject\the-road-to-academic-self-rescue
```

---

### 步骤 3：修改代码

在 VS Code 中正常编辑文件，比如：
- 修改 `README.md`
- 在 `从科研到写论文/` 文件夹里添加新文件
- 删除不需要的文件

修改完记得保存（`Ctrl+S`）。

---

### 步骤 4：查看修改状态

在 VS Code 终端中执行（**确保当前在仓库目录下**）：

```bash
git status
```

输出解读：
- 🔴 **红色 `modified:`** — 修改了但还没暂存的文件
- 🔴 **红色 `deleted:`** — 删除了但还没暂存的文件
- 🔴 **红色 `Untracked files:`** — 新创建但 Git 还不知道的文件
- 🟢 **绿色 `new file:` / `modified:`** — 已经暂存、准备提交的文件

> **💡 提示：** VS Code 左侧的源代码管理图标（`Ctrl+Shift+G`）也会直观地显示文件的修改状态，还能看到修改的具体内容。

---

### 步骤 5：暂存修改（git add）

把你要提交的文件加入暂存区：

```bash
# 暂存所有修改（最常用）
git add .

# 或者只暂存某个文件
git add README.md

# 暂存某个文件夹
git add "从科研到写论文/"

# 交互式暂存（可以一个一个文件确认）
git add -i
```

> **⚠️ 注意：** 不要轻易用 `git add .` 把不该提交的文件也加进去了（比如配置文件、临时文件等）。设置好 `.gitignore` 可以避免这个问题（见第三部分）。

---

### 步骤 6：提交到本地仓库（git commit）

```bash
git commit -m "这里写清楚你改了什么"
```

**提交信息（commit message）示例：**
```bash
git commit -m "添加了第一篇论文笔记"
git commit -m "修改了README中的项目说明"
git commit -m "修复了引言部分的格式问题"
```

**⚠️ 提交信息的规范：**
- ✅ 写清楚"做了什么"，不要写"更新"、"修改"这种模糊的词
- ✅ 用现在时态：比如"添加xxx"而不是"添加了xxx"
- ❌ 不要写：`"111"`、`"修bug"`、`"。"`、`"update"`

---

### 步骤 7：推送到 GitHub（git push）

```bash
git push origin main
```

>`main` 是分支名，如果你的仓库用的是 `master`，就换成 `master`（用 `git branch` 可以查看当前分支）。

**如果推送失败（网络问题）：**
```bash
# 使用代理推送
git -c http.proxy=http://127.0.0.1:7890 push origin main
```

推送成功之后，打开浏览器访问 [github.com/Star-hhy/the-road-to-academic-self-rescue](https://github.com/Star-hhy/the-road-to-academic-self-rescue)，就能看到你的修改了！

---

## 📋 第三部分：日常操作速查表

每天打开电脑后的标准流程：

```bash
# 1. 先拉取最新代码（防止冲突）
git pull origin main

# 2. 改你的代码...

# 3. 查看改了哪些
git status

# 4. 暂存所有修改
git add .

# 5. 提交到本地
git commit -m "写清楚你这次做了什么"

# 6. 推送到 GitHub
git push origin main
```

> **💡 一句话记住：** `status` → `add .` → `commit -m "..."` → `push origin main`

---

## 🛡️ 第四部分：常见问题与注意事项

### 1. 网络不通、连接超时

GitHub 在国内经常连不上。解决方案：

```bash
# 方法一：每次命令临时加代理
git clone https://... -c http.proxy=http://127.0.0.1:7890
git push -c http.proxy=http://127.0.0.1:7890
git pull -c http.proxy=http://127.0.0.1:7890

# 方法二：给当前仓库设置代理（一劳永逸）
cd D:\githubProject\the-road-to-academic-self-rescue
git config http.proxy http://127.0.0.1:7890
# 取消代理
git config --unset http.proxy
```

> `127.0.0.1:7890` 是 Clash 的默认代理端口，如果你用其他代理工具，端口可能不同。

### 2. 忘记自己改了什么

```bash
# 查看还没暂存的修改内容
git diff

# 查看已经暂存了的修改内容
git diff --staged

# 查看提交历史
git log --oneline
```

### 3. 暂存后想反悔

```bash
# 把某个文件从暂存区退回（文件还在，只是不提交它了）
git restore --staged 文件名

# 把全部文件从暂存区退回
git restore --staged .
```

### 4. 修改了文件想恢复原样

```bash
# 撤销对某个文件的修改（回到上次 commit 的状态）
git restore 文件名

# 撤销所有修改
git restore .
```

> **⚠️ 警告：** 这个操作不可撤销！恢复之前确认你真的不要这些修改了。

### 5. 提交信息写错了怎么办

```bash
# 修改最后一次的提交信息
git commit --amend -m "新的提交信息"
```

> **⚠️ 注意：** 如果已经 `push` 到 GitHub 了，尽量不要用这条命令，会导致推送失败。

### 6. 推送被拒绝（push rejected）

通常是因为别人（或者你在另一台电脑上）推送了新内容，你的本地版本落后了。

```bash
# 先拉取并合并
git pull origin main

# 如果有冲突（conflict），解决冲突后：
git add .
git commit -m "解决了冲突"
git push origin main
```

### 7. 出现合并冲突（merge conflict）

当你和别人的修改有冲突时（改了同一行的内容），Git 会提示冲突。冲突文件里会显示：

```
<<<<<<< HEAD
这是你的版本
=======
这是远程的版本
>>>>>>> main
```

**解决方法：**
1. 手动编辑文件，决定保留哪些内容
2. 删除 `<<<<<<<`、`=======`、`>>>>>>>` 这些标记
3. 保存文件
4. 执行 `git add .` 和 `git commit -m "解决冲突"` 和 `git push origin main`

> **💡 VS Code 会帮你高亮冲突区域**，点击"Accept Current"或"Accept Incoming"即可快速解决。

### 8. 不想提交某些文件（.gitignore）

在仓库根目录创建 `.gitignore` 文件，写上不需要 Git 跟踪的文件：

```gitignore
# Python 缓存文件
__pycache__/
*.pyc

# 临时文件
*.tmp
*.log

# 系统文件（Windows）
Thumbs.db
desktop.ini

# 编辑器配置
.vscode/
.idea/
```

### 9. 大文件不要提交到 GitHub

- GitHub 单个文件限制 **100MB**
- 超过 **50MB** 就会收到警告
- 图片、PDF、数据集等大文件可以用 Git LFS 或者放网盘

### 10. 密码和个人信息不要提交

- 绝对不要把密码、API Key、数据库连接串等提交到仓库
- 用环境变量或单独的配置文件管理敏感信息
- 这些文件加入 `.gitignore`

---

## 🔄 第五部分：特殊操作（进阶）

### 创建新分支

当你想尝试一个实验性的修改，但不想影响主代码时：

```bash
# 创建并切换到新分支
git checkout -b 分支名

# 在新分支上工作、提交
git add .
git commit -m "实验性修改"

# 推送到 GitHub
git push origin 分支名

# 切换回 main 分支
git checkout main
```

### 查看提交历史

```bash
# 简洁版
git log --oneline

# 详细版
git log

# 带图的分支历史
git log --oneline --graph --all
```

### 对比两次提交的差异

```bash
git diff 提交ID1 提交ID2
```

---

## ✅ 检查清单

每次提交前，确认以下事项：

- [ ] 我的代码没有语法错误
- [ ] 我运行过 `git status`，确认了要提交的文件
- [ ] 我没有包含敏感信息（密码、Key 等）
- [ ] 我没有提交临时文件和大文件
- [ ] 提交信息写清楚了"做了什么"

---

## 📚 常用命令速记

| 命令 | 作用 |
|------|------|
| `git status` | 查看当前状态 |
| `git add .` | 暂存全部修改 |
| `git add 文件名` | 暂存指定文件 |
| `git commit -m "描述"` | 提交到本地 |
| `git push origin main` | 推送到 GitHub |
| `git pull origin main` | 从 GitHub 拉取最新代码 |
| `git log --oneline` | 查看提交历史 |
| `git diff` | 查看具体修改内容 |
| `git restore 文件名` | 撤销修改 |
| `git restore --staged 文件名` | 取消暂存 |
| `git checkout -b 分支名` | 创建新分支 |
| `git checkout main` | 切换到 main 分支 |
| `git clone 仓库地址` | 克隆仓库 |

---

> 📅 生成时间：2026年6月11日
> 👤 GitHub: [Star-hhy](https://github.com/Star-hhy)




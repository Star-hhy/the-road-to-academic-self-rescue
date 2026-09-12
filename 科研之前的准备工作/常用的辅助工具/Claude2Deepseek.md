



[TOC]



## Claude Code + DeepSeek API 完整配置指南（Windows）

### 原理

DeepSeek 提供原生 Anthropic 兼容端点，Claude Code 直接对接，**零中间代理**（这里就不用去下载`littel LLM`了）。

------

### 前提准备

#### 1. 安装 Node.js

前往 [nodejs.org](https://nodejs.org)，下载 LTS 版本安装。

安装完成后验证：

cmd输入：

```cmd
node --version
npm --version
```

#### 2. 获取 DeepSeek API Key

前往 [platform.deepseek.com](https://platform.deepseek.com) 注册账号，进入控制台创建 API Key，复制保存好。

------

### 第一步：解决 PowerShell 执行策略问题

Windows 默认 PowerShell 禁止运行脚本，**全程使用 CMD（命令提示符）**，不要用 PowerShell。

打开方式：按 `Win+R`，输入 `cmd`，回车。

------

### 第二步：安装 Claude Code

在 CMD 里运行：

cmd输入：

```cmd
npm install -g @anthropic-ai/claude-code
```

验证安装：

cmd输入：

```cmd
claude --version
```

输出类似 `2.1.158 (Claude Code)` 说明安装成功。

------

### 第三步：清除所有可能的干扰环境变量（根据情况做选择）

如果你之前尝试过任何配置，必须先清除残留变量，否则会产生 `Auth conflict` 冲突报错：

cmd输入：

```cmd
setx ANTHROPIC_API_KEY ""
setx ANTHROPIC_AUTH_TOKEN ""
setx ANTHROPIC_BASE_URL ""
setx OPENAI_API_KEY ""
setx OPENAI_BASE_URL ""
```

**关闭当前 CMD，重新打开一个新的 CMD 窗口**，让清除生效。

------

### 第四步：删除旧的登录状态（根据情况做选择）

如果之前登录过 Claude Code，旧的 token 会干扰新配置：

cmd输入：

```cmd
del %USERPROFILE%\.claude.json
```

> 文件不存在的话会报错，忽略即可，继续下一步。

------

### 第五步：写入 DeepSeek 配置

创建 `.claude` 目录（如果不存在）：

cmd输入：

```cmd
mkdir %USERPROFILE%\.claude
```

打开配置文件：

cmd输入：

```cmd
notepad %USERPROFILE%\.claude\settings.json
```

> 如果提示文件不存在，选**是**新建。

粘贴以下内容，**把 `你的DeepSeek_API_Key` 替换成你的真实 Key**：

json

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "你的DeepSeek_API_Key",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  },
  "permissions": {
    "allow": [],
    "deny": []
  },
  "theme": "dark"
}
```

保存（`Ctrl+S`），关闭记事本。

> ⚠️ **只写 `ANTHROPIC_AUTH_TOKEN`，绝对不要同时写 `ANTHROPIC_API_KEY`**，两个共存会导致冲突。

------

### 第六步：验证命令行是否成功

在 CMD 里运行：

cmd输入：

```cmd
claude "你好，用中文回复我"
```

看到 DeepSeek 用中文回复（比如"你好！有什么我可以帮你的吗？"），说明配置完全成功。



### 第七步(拓展)：接入 VS Code 扩展(强推)

<u>注意</u>：方案一**以后可以切换其他模型**，方案二的话更直接、方便。

#### 方案一：使用 `cce` 工具 (最推荐)

`cce` (Claude Code Env Launcher) 是一个专门为解决这类问题而设计的工具，由社区开发者创建。它通过创建一个独立的子进程来启动 Claude Code，并为其注入所需的环境变量，从而完全绕过了 VS Code 启动时环境变量丢失的问题 。

**操作步骤如下：**

1. **安装 `cce`**：在**任何**一个你已配置好 DeepSeek 环境的终端里，运行以下命令进行全局安装：

   ```
   npm install -g @xiaofuzhou/cce
   ```

2. **配置 `cce`**：安装完成后，运行配置命令。它会打开一个配置文件，你只需将之前为 Claude Code 配置的 DeepSeek API 信息填入即可。

   ```bash
   cce edit
   ```

   参考配置如下（请替换为你的真实 API Key）：

   ```json
   {
     "version": 1,
     "default": "deepseek",
     "envs": {
       "deepseek": {
         "description": "我的 DeepSeek 配置",
         "env": {
           "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
           "ANTHROPIC_AUTH_TOKEN": "你的DeepSeek API Key"
         }
       }
     }
   }
   ```

3. **在 VS Code 中使用**：以后，无论你通过什么方式（直接点击图标或通过命令行）打开 VS Code，只需在 VS Code 的**新建终端**里输入 `cce` 并回车，就会自动启动一个配置好 DeepSeek 后端的 Claude Code 对话。

#### 方案二：直接在 VS Code 插件中配置（最直接）

这是最直接的方法，适用于图方便的开发者。前提是你使用的是 `Claude Code for VS Code` 这个官方插件。

**操作步骤如下：**

1. 在 VS Code 中，点击左下角的齿轮图标，选择“设置”。

2. 在搜索框输入 `Claude`，找到插件的扩展设置。

3. 寻找 `Code: Settings Link` 或类似的选项，点击 `Edit in settings.json`，将你的 DeepSeek 配置直接写入 `settings.json` 文件中 ：

   json

   ```
   {
     "claude-code.environmentVariables": [
       {
         "name": "ANTHROPIC_BASE_URL",
         "value": "https://api.deepseek.com/anthropic"
       },
       {
         "name": "ANTHROPIC_AUTH_TOKEN",
         "value": "你的DeepSeek API Key"
       }
     ]
   }
   ```

   

4. 保存后，重启 VS Code 或重新打开 Claude Code 侧边栏即可生效。

------

### 全流程总结

```
第一次配置（只做一次）：
  安装 Node.js → 安装 Claude Code → 清除旧变量 → 写入 settings.json → 创建启动脚本

日常使用：
  双击"启动Claude.bat" → 直接对话（调用DeepSeek）
  或
  双击"用DeepSeek打开VSCode.bat" → VS Code里用Claude Code侧边栏
```

------



### 费用参考

截止2026-6-1日

| 模型              | 缓存命中输入   | 普通输入      | 输出          |
| ----------------- | -------------- | ------------- | ------------- |
| DeepSeek V4 Flash | $0.003/M token | $0.14/M token | $0.28/M token |
| DeepSeek V4 Pro   | $0.004/M token | $0.44/M token | $0.88/M token |

相比 Claude Sonnet 原价 $3/$15 per M token，**便宜约 10–50 倍**。

------



### 常见报错速查

| 报错                                         | 原因                                                   | 解决                                                         |
| -------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| `Auth conflict`                              | `ANTHROPIC_API_KEY` 和 `ANTHROPIC_AUTH_TOKEN` 同时存在 | settings.json 只保留 `ANTHROPIC_AUTH_TOKEN`                  |
| `Invalid API key`                            | API Key 填错，或两个 Key 字段冲突                      | 检查 settings.json，确认只有一个 Key 字段                    |
| `无法加载文件 claude.ps1`                    | 在 PowerShell 里运行了命令                             | 换用 CMD                                                     |
| `claude` 不是内部命令                        | Node.js 未安装或 npm 全局路径未加入 PATH               | 重新安装 Node.js，重启 CMD                                   |
| **`API Error: 402 Insufficient Balance`**    | DeepSeek 账户余额为零                                  | 去 platform.deepseek.com 充值                                |
| **`Failed to connect to api.anthropic.com`** | settings.json 未配置或未生效，仍连 Anthropic 官方      | 检查 settings.json 是否有 `ANTHROPIC_BASE_URL`，清除系统环境变量残留 |

### 补充

#### 对于常见的问题的小补充

如遇到**`Failed to connect to api.anthropic.com`**

**第一步**：检查 settings.json 是否存在且内容正确

cmd输入

```cmd
type %USERPROFILE%\.claude\settings.json
```

应该输出：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-xxxxxxxx",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  },
  ...
}
```

**如果文件不存在或内容不对**，重新创建：

cmd输入

```cmd
mkdir %USERPROFILE%\.claude
notepad %USERPROFILE%\.claude\settings.json
```

粘贴正确内容保存。

------

**第二步**：确认没有系统环境变量干扰

cmd输入

```cmd
echo %ANTHROPIC_BASE_URL%
```

如果输出是空的或者是 `api.anthropic.com`，说明系统环境变量有残留，运行清除：

cmd输入

```cmd
setx ANTHROPIC_BASE_URL ""
setx ANTHROPIC_AUTH_TOKEN ""
setx ANTHROPIC_API_KEY ""
```

**重新打开 CMD**，再试。

------

**第三步**：验证

cmd输入

```cmd
claude "你好"
```

#### 如何生成deepseek的API key

##### 打开https://platform.deepseek.com/ 

(建议开始充1r就可以。)

<img src="C:\Users\gump_\AppData\Roaming\Typora\typora-user-images\image-20260603114725780.png" style="zoom:33%;" />

##### 充值后，创建API keys

<img src="C:\Users\gump_\AppData\Roaming\Typora\typora-user-images\image-20260603114847742.png" alt="image-20260603114847742" style="zoom:33%;" />

注意，不限制生成次数，但是每次生成的Key要及时保存。一定要避免泄露，如果泄露的话要及时删除。

​	这里生成Key就我们json文件里需要写的。


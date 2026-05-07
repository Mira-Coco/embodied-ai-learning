# Codex 配置流程学习记录

本文记录从零开始配置 Codex 开发环境的完整流程，适合作为后续复盘、排错和维护参考。

## 1. Git 安装与配置

### 安装 Git

在 Windows 上安装 Git：

1. 访问 Git 官网下载安装包。
2. 使用默认选项安装即可。
3. 安装完成后，打开 PowerShell 或 Git Bash，确认 Git 可用：

```powershell
git --version
```

如果能看到 Git 版本号，说明安装成功。

### 配置用户名和邮箱

Git 需要记录每次提交的作者信息：

```powershell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看配置：

```powershell
git config --global --list
```

常见配置项：

```powershell
git config --global init.defaultBranch main
git config --global core.autocrlf true
```

说明：

- `user.name` 和 `user.email` 会写入 commit 记录。
- `init.defaultBranch main` 用于让新仓库默认使用 `main` 分支。
- `core.autocrlf true` 适合 Windows 环境处理换行符。

## 2. GitHub 仓库创建

### 创建远程仓库

在 GitHub 上创建新仓库：

1. 登录 GitHub。
2. 点击右上角 `New repository`。
3. 输入仓库名，例如 `embodied-ai-learning`。
4. 选择公开或私有。
5. 如果本地已经有 README、LICENSE 或 `.gitignore`，远程仓库可以先不要初始化这些文件，避免后续合并冲突。

### 本地仓库关联远程仓库

在本地项目目录中执行：

```powershell
git init
git remote add origin https://github.com/<用户名>/<仓库名>.git
git branch -M main
```

查看远程地址：

```powershell
git remote -v
```

如果远程地址写错，可以修改：

```powershell
git remote set-url origin https://github.com/<用户名>/<仓库名>.git
```

## 3. OpenAI API Key 创建与充值

### 创建 API Key

Codex CLI 需要使用 OpenAI API Key 访问模型能力。

基本流程：

1. 登录 OpenAI 平台。
2. 进入 API Keys 页面。
3. 创建新的 API Key。
4. 复制并妥善保存 Key。

注意事项：

- API Key 只会完整显示一次。
- 不要把 API Key 写进 Git 仓库、README、截图或聊天记录。
- 如果 Key 泄露，应立即删除并重新创建。

### 充值或开通计费

如果账号没有可用额度，Codex 调用 API 时可能失败。需要在 OpenAI 平台完成：

1. 添加付款方式。
2. 充值或开通计费。
3. 检查账户余额和用量限制。

### 配置环境变量

Windows PowerShell 临时配置：

```powershell
$env:OPENAI_API_KEY="你的 API Key"
```

该方式只对当前 PowerShell 窗口有效。

长期配置可以使用系统环境变量，或按 Codex CLI 的登录流程保存认证信息。

## 4. Node.js 与 npm 安装

Codex CLI 通常通过 npm 安装，因此需要先安装 Node.js。

### 安装 Node.js

推荐安装 Node.js LTS 版本。

安装完成后，确认版本：

```powershell
node -v
npm -v
```

如果两个命令都能输出版本号，说明 Node.js 和 npm 安装成功。

### 常见检查

查看 npm 全局包安装路径：

```powershell
npm root -g
```

查看 npm 全局命令路径：

```powershell
npm bin -g
```

如果系统提示找不到 `npm` 或 `node`，通常是 PATH 环境变量没有生效，可以重新打开 PowerShell，或重启电脑后再试。

## 5. Codex CLI 安装

使用 npm 全局安装 Codex CLI：

```powershell
npm install -g @openai/codex
```

确认安装结果：

```powershell
codex --version
```

如果能看到版本号，说明 Codex CLI 已经安装成功。

如果提示 `codex` 不是可识别的命令，优先检查：

- npm 全局命令目录是否加入 PATH。
- PowerShell 是否需要重新打开。
- Node.js 和 npm 是否正确安装。

## 6. 第一次启动 Codex

在项目目录中启动 Codex：

```powershell
cd C:\Users\GMY\Desktop\embodied-ai-learning
codex
```

第一次启动时可能需要完成：

- 登录或配置 API Key。
- 授权 Codex 访问当前工作目录。
- 确认模型、权限和执行策略。

启动后，Codex 会读取当前项目上下文，并可以根据指令查看文件、修改代码、运行命令和帮助排错。

## 7. 用 Codex 修改 README

可以让 Codex 根据自然语言指令修改 `README.md`，例如：

```text
请帮我完善 README.md，加入项目目标、学习路线和使用方法。
```

推荐工作方式：

1. 先让 Codex 查看现有 README。
2. 明确要新增或修改的内容。
3. 让 Codex 只修改指定文件。
4. 修改后检查差异：

```powershell
git diff README.md
```

注意：

- 修改前最好确认工作区状态。
- 如果 README 中已有人工写好的内容，应要求 Codex 保留原意并增量修改。
- 重要文档修改后，需要人工再读一遍，确认表达和事实准确。

## 8. git add / commit / push 流程

### 查看状态

```powershell
git status
```

### 查看修改内容

```powershell
git diff
```

### 添加文件到暂存区

添加指定文件：

```powershell
git add README.md
```

添加全部修改：

```powershell
git add .
```

### 创建提交

```powershell
git commit -m "docs: update README"
```

提交信息建议：

- 简短说明本次修改目的。
- 使用英文或中文都可以，但要保持项目内风格一致。
- 常见前缀包括 `docs:`、`fix:`、`feat:`、`chore:`。

### 推送到 GitHub

第一次推送主分支：

```powershell
git push -u origin main
```

后续推送：

```powershell
git push
```

### 完整常用流程

```powershell
git status
git diff
git add .
git commit -m "docs: update project notes"
git push
```

## 9. 常见报错与解决方案

### `git` 不是可识别的命令

原因：

- Git 未安装。
- Git 安装后 PATH 未生效。

解决：

1. 重新安装 Git。
2. 重新打开 PowerShell。
3. 重启电脑。
4. 执行 `git --version` 验证。

### `node` 或 `npm` 不是可识别的命令

原因：

- Node.js 未安装。
- PATH 未生效。

解决：

1. 安装 Node.js LTS。
2. 重新打开 PowerShell。
3. 执行 `node -v` 和 `npm -v` 验证。

### `codex` 不是可识别的命令

原因：

- Codex CLI 未安装成功。
- npm 全局命令目录不在 PATH 中。
- PowerShell 还没有刷新环境变量。

解决：

```powershell
npm install -g @openai/codex
codex --version
```

如果仍然失败，检查 npm 全局安装路径，并确认该路径已加入 PATH。

### API Key 无效或未配置

可能表现：

- Codex 启动后无法调用模型。
- 提示 authentication、unauthorized、invalid API key 等错误。

解决：

1. 确认 API Key 是否复制完整。
2. 确认环境变量名是 `OPENAI_API_KEY`。
3. 确认当前 PowerShell 窗口已经设置环境变量。
4. 如果 Key 泄露或不确定是否有效，删除旧 Key 并创建新 Key。

### 账户余额不足或计费未开通

可能表现：

- 请求失败。
- 提示 quota、billing、insufficient balance 等错误。

解决：

1. 登录 OpenAI 平台查看余额。
2. 添加付款方式或充值。
3. 检查项目和组织的用量限制。

### `git push` 失败

常见原因：

- 没有登录 GitHub。
- 没有仓库权限。
- 远程地址错误。
- 本地分支和远程分支不一致。

排查命令：

```powershell
git remote -v
git branch
git status
```

常见解决：

```powershell
git remote set-url origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

如果 GitHub 要求认证，按提示使用浏览器登录或使用 Personal Access Token。

### `fatal: not a git repository`

原因：

- 当前目录不是 Git 仓库。
- 没有进入项目目录。

解决：

```powershell
cd C:\Users\GMY\Desktop\embodied-ai-learning
git status
```

如果项目还没有初始化 Git：

```powershell
git init
```

### `nothing to commit, working tree clean`

含义：

- 当前没有需要提交的新修改。
- 所有修改都已经提交。

处理：

- 如果这是预期结果，不需要操作。
- 如果刚刚修改了文件但 Git 没有识别，确认文件是否保存，或是否被 `.gitignore` 忽略。

### 推送时提示远程包含本地没有的提交

可能表现：

```text
Updates were rejected because the remote contains work that you do not have locally.
```

解决思路：

1. 先拉取远程更新：

```powershell
git pull --rebase origin main
```

2. 如果有冲突，手动解决冲突。
3. 冲突解决后继续：

```powershell
git add .
git rebase --continue
git push
```

## 维护建议

- 每次完成一个阶段性配置或排错后，及时更新本文档。
- 重要命令尽量保留原始命令和错误信息，方便以后搜索。
- 不要把 API Key、Token、密码或个人敏感信息写入本文档。
- 配置类文档建议和代码一起提交，形成可追踪的学习记录。


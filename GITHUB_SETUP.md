# GitHub 推送指南

本地 git 仓库已初始化：
- `dev` 分支：当前开发版（v1 + v2 + v3 都在，HEAD = 2071704）
- `main` 分支：当前等于 dev；v3 经你确认稳定后再合并

下一步：把本地仓库推到 GitHub。两种方式任选。

## 方式 A · 用 GitHub CLI（推荐，gh CLI 未装的话先装）

```bash
# 1. 安装 gh CLI（Windows）
winget install GitHub.cli
# 或下载：https://cli.github.com/

# 2. 登录
gh auth login
# 选 GitHub.com → HTTPS → Login with a web browser

# 3. 在 VISjudy 名下创建仓库并推送
cd "C:\Users\yj_zo\WorkBuddy\3DGS学术论文工作台\project"
gh repo create 3dgs-paper-workbench --public --source=. --remote=origin --description "lidar-img-3DGS 论文 Review 工作台 (WorkBuddy 资料库配套)"

# 4. 推送两个分支
git push -u origin main
git push -u origin dev
```

仓库创建后会出现在 https://github.com/VISjudy/3dgs-paper-workbench

## 方式 B · 用 WorkBuddy 的 GitHub 连接器（已连接 ✓）

GitHub MCP 已连接（账号 VISjudy），可直接让智能体用 `push_files` 推送文件到已存在的仓库。
注意：MCP 推送会生成新提交（不保留本地 commit 历史），推送后本地需同步一次：
```bash
git fetch origin && git reset --hard origin/main   # main 分支
```

## 方式 C · 纯手动（无 gh CLI 也不连 connector）

1. 在 https://github.com/new 手动建仓 `VISjudy/3dgs-paper-workbench`（不要勾 README/license/.gitignore，留空）
2. 在本地 project/ 目录执行：
   ```bash
   git remote add origin https://github.com/VISjudy/3dgs-paper-workbench.git
   git push -u origin main
   git push -u origin dev
   ```
3. 第一次 push 需用 Personal Access Token（PAT）认证，不要用密码。
   - 在 https://github.com/settings/tokens 创建一个 fine-grained PAT，权限选 `Contents: Read and write`
   - 推送时用户名填 `VISjudy`，密码填 token

## 后续日常流程

### 改新版本（在 dev 上开发）
```bash
git checkout dev
# 编辑文件，比如做 v3：
mkdir v3 && cp v2/* v3/ && edit v3/workbench.html
# 自验 + lint（见 README.md）
git add -A
git commit -m "v3: 新增xxx功能"
git push origin dev
```

### v3 经你确认稳定后，合并到 main
```bash
git checkout main
git merge dev
git tag v3
git push origin main --tags
```

### 任意版本回退
```bash
git checkout main
# 看历史
git log --oneline
# 回退到某标签
git checkout v2    # 切到 v2 tag
# 或硬重置（会丢失 main 之后的提交，慎用）
git reset --hard v2
```

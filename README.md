# 3DGS 论文 Review 工作台

lidar-img-3DGS 论文资料库配套的可视化工作台：概览 / 阅读 / 思维导图 / 论文写作 / 论文抓取 五大模块。

## 版本管理

本仓库采用"主分支=稳定版 / dev 分支=开发版"的双分支策略：

- `main` —— 你已确认可用的稳定版本（每代正式发布）
- `dev` —— 当前正在开发、可能含未验证改动的版本（最新开发在此）
- `v1`、`v2`、`v3` ... —— 资料库不同代际的快照（self-contained HTML + schema + CHANGELOG + 资源 ID 列表），方便任意回退

## 目录

```
project/
├── README.md                            # 本文件
├── library_ids.md                       # 资料库资源 ID 速查（重要！）
├── CHANGELOG.md                         # 跨版本变更摘要
├── skills/                              # 用户级 skill 源码
│   ├── 3dgs论文结构化翻译/SKILL.md
│   ├── 3dgs学术论文写作/SKILL.md
│   └── 3dgs每日论文搜索/SKILL.md
├── v1/                                  # 2026-08-25 v1（4模块）
│   ├── workbench.html
│   ├── canonical_schema.json
│   └── CHANGELOG.md
└── v2/                                  # 2026-08-25 v2（5模块 + 详细翻译+配图 + 5格笔记 + 搜索渠道）
    ├── workbench.html
    ├── canonical_schema.json
    └── CHANGELOG.md
```

## 工作流

### 启用新版本
1. 编辑 `dev` 分支上的新版本（拷贝 vN/ 到 vN+1/，调整）
2. 在资料库重新跑 `import_html.py` 部署新 page 节点
3. 自验：本地 `python -m http.server` + 浏览器打开 HTML 截图核对
4. 跑两道 lint（见下）
5. 确认无误后合并 `dev → main`，并在 GitHub 上 `git tag vN+1`

### 浏览器自验 + lint（强制）
每个版本做完必跑：
```bash
# 1. 自检：起本地服务，浏览器打开 HTML 截图各 tab
cd v2
python -m http.server 8765
# 在另一终端：
# agent-browser open http://127.0.0.1:8765/workbench.html
# agent-browser screenshot v2-overview.png --full
# 切换 tab 检查（snapshot -i 拿按钮 ref，再 click @eN）
# agent-browser close

# 2. lint 第一道（schema + selector 校验）
cd ../..
python "<library skill dir>/page/lint_schema.py" \
  --schema "v2/canonical_schema.json" --html "v2/workbench.html"

# 3. lint 第二道（SDK 调用校验，先拉服务端真 schema）
# 用资料库 skill 取 schema 后：
python "<library skill dir>/page/lint_database_sdk_usage.py" \
  --schema server_schema.json --html v2/workbench.html
```

### 同步资料库
HTML 改完自验通过后，用资料库 skill（library）的 `page/import_html.py` 推送到资料库：
```bash
python "<library skill dir>/page/import_html.py" "v2/workbench.html" \
  --file-name "3DGS论文Review工作台v2.html" \
  --databases '[{"id":"OJLJkCoJSkV3TdHWBVEjhH"},{"id":"H2ElVIUQlDNgS1SxXD0A19"}]' \
  --space-id "EDDRwZ0Oq6nqiHm2WnmrkE" \
  --parent-id "BgSQ6CbjAKeDk9qKLtGHCA"
```

数据库表结构更新（如新增字段）走 `database/add_database_field.py` 增列。

## 资料库资源 ID 速查

详见 [library_ids.md](library_ids.md)。任何字段名调整后请同步更新该文档。

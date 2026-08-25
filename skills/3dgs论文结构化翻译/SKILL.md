---
name: 3dgs-paper-translation
description: 论文结构化翻译skill（lidar-img-3DGS论文资料库专用）。当用户提供 arXiv/CVPR 等论文链接、PDF 或标题，要求"结构化翻译、生成中文摘要、精读、加入资料库、入库"时使用。产出：结构化中文文档（概要+详细章节翻译+配图）+ 论文总表记录（含「详细章节翻译(带图)」「配图」字段）+ 分类导航更新。
version: 2.0.0
---

# 论文结构化翻译（lidar-img-3DGS 论文库）

## 固定资源 ID（资料库"我的文档"空间 EDDRwZ0Oq6nqiHm2WnmrkE）

| 资源 | ID |
|---|---|
| 论文根目录（文件夹） | `BgSQ6CbjAKeDk9qKLtGHCA` |
| 📥 待阅读区（目录文档） | `th8CPBrjAOWbnGgRWJ3mIt` |
| 📚 论文分类导航（文档） | `ClHqszSsqjyI15Yb0gNh8e` |
| 3DGS论文总表（database） | `OJLJkCoJSkV3TdHWBVEjhH` |
| 工作台 v2 page | `2KgYz0c9VePzq0qUX0Tog5` |
| 搜索渠道 database | `H2ElVIUQlDNgS1SxXD0A19` |

## 工作流

### 1. 获取论文内容
- arXiv 链接：WebFetch `https://arxiv.org/abs/<id>` 取摘要与元数据；需要方法/章节细节时再抓 `https://arxiv.org/html/<id>`（LaTeX 渲染版，含图）。
- 本地 PDF：用 Read 工具直接读（分页读取）。
- 仅标题：WebSearch 定位论文来源。

### 2. 判断入库位置
- 用户明确"加入资料库/入库"或论文已确认相关 → 挂到论文根目录 `BgSQ6CbjAKeDk9qKLtGHCA` 下，状态=`待精读`。
- 用户说"先放待阅读区"或来自每日搜索 → 挂到待阅读区 `th8CPBrjAOWbnGgRWJ3mIt` 下，状态=`待判断相关性`。

### 3. 提取配图（v2 新增）
对 arXiv html 论文：抓 `https://arxiv.org/html/<id>` 后，从 `<figure>`/`<img>` 标签中提取前 3-6 张关键图（method overview / architecture / 主要结果），用资料库 skill（library）的 `manage/upload_image.py --url <imageUrl>` 上传得到平台内链，记录到 `images[]` 数组：
```
images = [{ imageUrl: 'workbuddy平台链接', title: '图1：方法架构', width: 1200, height: 800 }, ...]
```

### 4. 生成两份产物

**A. 结构化中文文档**（doc，挂在论文目录下或待阅读区）
用 `doc/create_doc.py` 创建，标题=`【中文简译标题】英文原标题`，正文 Markdown 模板：

```markdown
> 原文：[链接] ｜ 年份/发表源 ｜ 原文标题

## 📌 一句话总结
（30字以内）

## 📋 基本信息
作者/机构、发表源、年份、代码链接（如有）、数据类型（图像/激光点云/无人机/UAV/室内/室外）、输出（3DGS模型/网格/位姿/语义…）

## 🔬 研究问题与动机
…

## 🧠 核心方法（按论文章节压缩，配图嵌入）
### 章节1：…
![图1：xxx](workbuddy内链)

### 章节2：…
![图2：xxx](workbuddy内链)

## 🧪 实验与结果
…

## ⚠️ 局限与适用条件
…

## 🔗 与 LiDAR+图像 3DGS 的关联
…

## 💡 潜在启发与后续行动
```

**B. 详细章节翻译 markdown（写入总表的「详细章节翻译(带图)」字段）**
按原始论文章节（Introduction / Method / Experiments / Conclusion 等）适当压缩为中文版，保留图片引用（用 `![图X说明](workbuddy内链)`），控制在 3000-6000 字。最终一并在「中文结构化摘要」字段写 800-1500 字的概要版。

### 5. 写入论文总表
用 `database/batch_add_database_records.py`（database-id=`OJLJkCoJSkV3TdHWBVEjhH`），字段：
- 论文标题、作者/机构、年份、发表源、分类、状态、相关性
- 一句话总结、关键词
- **中文结构化摘要**（概要版，纯文本，\n 分段，【...】小标题，供阅读模式左栏概要部分渲染）
- **详细章节翻译(带图)**（按章节压缩的中文 markdown，含 `![...](workbuddy内链)`，供阅读模式左栏详细部分渲染，可滚轮下滑）
- **配图**（image 字段，images 数组，每项 `{imageUrl, title, width?, height?}`，供阅读模式左栏"图集"渲染）
- 论文原文链接、结构化文档链接、添加日期
- 个人阅读体会、5 个笔记字段（核心创新点/方法关键/实验结论/局限与质疑/对我的启发）默认留空，由用户在工作台阅读模式填写

写入前先 query 总表按论文标题去重，已存在则用 `batch_update_database_records.py` 增量更新。

### 6. 更新分类导航
正式入库时：读 `📚 论文分类导航`（`ClHqszSsqjyI15Yb0gNh8e`），在对应分类小标题下插入一行：
`- 论文名（年份，发表源）——一句话总结｜[结构化文档](链接)`

### 7. 回执
告知用户：结构化文档链接、已写入总表（状态/分类/详细翻译/配图），可在工作台 v2 阅读模式中查看并填写 5 格笔记。

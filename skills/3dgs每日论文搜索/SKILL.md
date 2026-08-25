---
name: 3dgs-daily-paper-search
description: 每日/主动论文搜索skill（lidar-img-3DGS论文资料库专用）。当用户说"搜论文、今天的arxiv、每日论文推荐、更新待阅读区"或在「🔍论文抓取」tab 点击生成搜索指令时使用。每日从「论文搜索渠道」database的启用渠道中按 3 篇/渠道 上限推送最相关论文到待阅读区。
version: 2.0.0
---

# 每日/主动论文搜索（lidar-img-3DGS 论文库）

## 固定资源 ID（资料库"我的文档"空间 EDDRwZ0Oq6nqiHm2WnmrkE）

| 资源 | ID |
|---|---|
| 📥 待阅读区（目录文档） | `th8CPBrjAOWbnGgRWJ3mIt` |
| 3DGS论文总表（database） | `OJLJkCoJSkV3TdHWBVEjhH` |
| 搜索渠道 database | `H2ElVIUQlDNgS1SxXD0A19` |
| 论文根目录 | `BgSQ6CbjAKeDk9qKLtGHCA` |

## 研究方向关键词组

- 核心：`3D Gaussian Splatting` / `3DGS` / `Gaussian splatting`
- 模态：`LiDAR camera fusion` / `LiDAR SLAM` / `point cloud image fusion` / `multi-modal reconstruction` / `ground LiDAR` / `handheld laser scanning` / `UAV`
- 加速：`Gaussian splatting acceleration` / `fast training` / `efficient 3DGS` / `compression` / `speeding up`
- 质量：`anti-aliasing Gaussian` / `artifact Gaussian splatting` / `geometry quality 3DGS` / `surface reconstruction Gaussian` / `pixel fusion`
- 中文：`三维高斯溅射 点云 图像` / `激光雷达 彩色图像 三维重建`

## 搜索渠道（来自「论文搜索渠道」database，可被用户在 workbench 🔍tab 增删改）

- `arXiv` — http://export.arxiv.org/api/query?search_query=...
- `计算机顶会` — CVPR/ICCV/ECCV open access
- `测绘SCI一区` — ISPRS JPRS、IEEE TGRS、Remote Sensing of Environment
- `RSS` / `其他` — 用户自定义

## 工作流

### 1. 读取搜索渠道
调用 `database/query_database_record.py`（database-id=`H2ElVIUQlDNgS1SxXD0A19`）拉所有渠道记录，按"启用=true"过滤，再按用户传入的"渠道类型"过滤（留空=全部）。每日任务遍历所有启用渠道；主动搜索任务按用户指定渠道范围跑。

### 2. 去重
先 `database/query_database_record.py`（database-id=`OJLJkCoJSkV3TdHWBVEjhH`）拉现有记录，按论文标题（忽略大小写与标点）过滤掉已入库的。

### 3. 检索每个渠道
对每个启用的渠道，按其「默认关键词」+ 任务关键词（每日任务用默认关键词；主动任务用用户输入）调对应 URL/API：
- **arXiv** 渠道：WebFetch arXiv API 拿 XML，解析 `<entry>`，限定近 3 天。
- **顶会/期刊** 渠道：WebSearch + WebFetch 开放获取页，按关键词命中。
- **RSS/自定义**：直接 WebFetch URL 解析。

### 4. 相关性筛选
对每篇候选论文，满足以下任一视为相关：
- 摘要同时命中 "Gaussian splatting" 与（LiDAR / 点云 / 多模态 / 加速 / 伪影 / 几何质量 / UAV）任一词
- 属于 3DGS 训练加速 / 质量提升的直接方法论文

每篇打相关性（高/中/低），每个渠道**只保留前 N 篇（N=渠道「每日推送数量」字段，默认 3）**，跨渠道全量再按相关性去重保留前 5。

### 5. 生成中文摘要文档
对入选论文，WebFetch arXiv abs 页拿摘要，写 300–600 字中文摘要文档：
```
> 原文链接/日期 ｜ 一句话总结 ｜ 研究问题 ｜ 方法思路 ｜ 与我研究的可能关联
```
用 `doc/create_doc.py` 创建，**parent-id=`th8CPBrjAOWbnGgRWJ3mIt`**，标题=`（候选）论文英文标题`。

### 6. 写入总表
`database/batch_add_database_records.py`（database-id=`OJLJkCoJSkV3TdHWBVEjhH`）：
- 状态=`待判断相关性`；分类按内容预判（可多选）；相关性=高/中/低
- 一句话总结、关键词、中文结构化摘要（精简 300–600 字）、论文原文链接、结构化文档链接、添加日期=当天
- 详细章节翻译(带图)、配图、5个笔记字段留空

### 7. 更新待阅读区目录页
读 `th8CPBrjAOWbnGgRWJ3mIt`（get_doc_reviews.py），在"今日候选"下追加当日日期 + 论文链接列表（直接编辑 submit_doc_edit.py）。

### 8. 回执
列出：今日新增 N 篇候选（标题+一句话总结+相关性+文档链接+来源渠道）；如无可说明今日未发现高相关新论文。提醒用户到工作台概览模块判断，相关的说"把《xxx》正式加入资料库"（转 3dgs-paper-translation skill 流程）。

## 自动化注意
本 skill 被每日自动化调用时无人值守：跳过一切询问，直接执行；找不到高相关论文时如实记录"今日无新增"，不要为凑数降低相关性标准。

# 资料库资源 ID 速查

所有 ID 都在「我的文档」空间 `EDDRwZ0Oq6nqiHm2WnmrkE` 下。

## 工作台与文件夹

| 资源 | 类型 | ID | 备注 |
|---|---|---|---|
| lidar-img-3DGS论文 | folder | `BgSQ6CbjAKeDk9qKLtGHCA` | 论文库根目录 |
| 📥 待阅读区 | doc | `th8CPBrjAOWbnGgRWJ3mIt` | 每日新论文挂这里 |
| 📚 论文分类导航 | doc | `ClHqszSsqjyI15Yb0gNh8e` | 按四大方向分小标题 |
| 3DGS论文Review工作台 v1 | page | `EoR3BCTV6FS42ThkuksFuK` | 4模块 v1 |
| 3DGS论文Review工作台 v2 | page | `2KgYz0c9VePzq0qUX0Tog5` | 5模块 v2 |
| 3DGS论文Review工作台 v3 | page | `cnNeSwq1b6IJhMfx4D2tKe` | 5模块+思维导图美化+智能搜索，当前主推 |
| 3DGS论文总表 | database | `OJLJkCoJSkV3TdHWBVEjhH` | 21字段 |
| 论文搜索渠道 | database | `H2ElVIUQlDNgS1SxXD0A19` | 7字段 |

## 工作台 v2 字段列表（论文总表）

| 字段 | 类型 | 说明 |
|---|---|---|
| 论文标题 | text | 必填 |
| 作者/机构 | text | |
| 年份 | number | |
| 发表源 | select | arXiv/CVPR/ICCV/ECCV/SIGGRAPH/其他顶会/测绘SCI一区/其他期刊 |
| 分类 | multi_select | 训练加速/LiDAR+图像多模态融合/几何质量增强/图像质量与伪影抑制/综述/其他 |
| 状态 | select | 待判断相关性/待精读/已读已评价 |
| 相关性 | select | 高/中/低 |
| 一句话总结 | text | |
| 关键词 | text | |
| 中文结构化摘要 | text | 概要版（800-1500字） |
| 个人阅读体会 | text | 自由备注（v2 折叠到details） |
| 论文原文链接 | url | |
| 结构化文档链接 | url | |
| 添加日期 | date | yyyy-mm-dd |
| 详细章节翻译(带图) | text | 按章节压缩的中文 markdown 含 ![](url) |
| 配图 | image | images 数组，{imageUrl, title, width?, height?} |
| 核心创新点 | text | v2 5格笔记 ① |
| 方法关键 | text | v2 5格笔记 ② |
| 实验结论 | text | v2 5格笔记 ③ |
| 局限与质疑 | text | v2 5格笔记 ④ |
| 对我的启发 | text | v2 5格笔记 ⑤ |

## 搜索渠道表字段

| 字段 | 类型 | 说明 |
|---|---|---|
| 渠道名称 | text | |
| 渠道类型 | select | arXiv/计算机顶会/测绘SCI一区/RSS/其他 |
| 搜索URL/API | text | |
| 启用 | checkbox | |
| 每日推送数量 | number | 默认 3 |
| 默认关键词 | text | |
| 备注 | text | |

## 自动化

| 资源 | ID | 频率 |
|---|---|---|
| 每日3DGS论文搜索 | `automation-1787644164111` | 每天 09:00 |

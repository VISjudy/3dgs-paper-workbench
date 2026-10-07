# Proposal Logic Review｜研究计划书写作与逻辑审查

Codex 分发适配版 **1.0.1**。保留已确认的 1.0.0 核心规则、手册、范例、模板和行为用例；本次只补充分发、安装和可选展示文件。

**先检查目标—进展—Gap—问题—方法—验证为什么相连，再修改文字。**

## 在本地 Codex 安装

本版位于 `VISjudy/3dgs-paper-workbench` 的独立分支 `add-proposal-logic-review`，未改动原有技能和 main 分支。上传到 GitHub 不等于已经安装到电脑。

### 推荐：在 Codex 中粘贴

```text
$skill-installer 从以下 GitHub 目录安装完整的 proposal-logic-review：
https://github.com/VISjudy/3dgs-paper-workbench/tree/add-proposal-logic-review/skills/proposal-logic-review
保留整个目录及 references、templates、examples、evals、scripts、agents。
安装到当前 Codex 用户可发现的技能目录；报告实际路径。
若已有同名技能，先比较并询问，不要覆盖。
```

### 手动安装

在新的工作目录执行，需要 Git 与 Python 3：

```bash
git clone --branch add-proposal-logic-review --single-branch https://github.com/VISjudy/3dgs-paper-workbench.git
python 3dgs-paper-workbench/skills/proposal-logic-review/scripts/install_local.py
```

Windows 可以用 `py`，macOS/Linux 可以用 `python3` 代替 `python`。请在运行 Codex 的同一系统和用户环境执行；Windows 与 WSL 的用户目录不同。

脚本默认将整个目录复制到 `$HOME/.agents/skills/proposal-logic-review`；支持 `--destination` 指定完整目标目录，不联网、不删除、不覆盖已有安装，也不修改 `AGENTS.md` 或 `config.toml`。当前官方文档将 `$HOME/.agents/skills` 列为用户级目录，`.agents/skills` 列为项目级目录。安装到项目时，可使用：

```bash
python skills/proposal-logic-review/scripts/install_local.py --destination .agents/skills/proposal-logic-review
```

不要只复制 README、下载网页 HTML，或把未解压的 ZIP 当作已经安装的技能。目录应包含真正的 `SKILL.md` 和相对引用文件。

## 调用

```text
$proposal-logic-review
审查我的 proposal。先用一句话解释每个部分到下一部分的连接，指出最多三个关键逻辑断点，不立即重写全文。
```

CLI/IDE 可输入 `/skills` 或 `$` 选择技能。未出现时重新启动 Codex，并检查安装目录、同名副本及禁用设置。仍有问题时保留版本和报错排查；手动让 agent 读取实际路径下的 `SKILL.md` 可临时使用，但不等于自动发现问题已修复。

## 工作模式

- **梳理**：先确定目标、进展、Gap、问题与连接句。
- **写作／双语定稿**：按已确认主线成稿，保持编号、论断力度和编辑范围。
- **严格审查**：原句 → 缺失前提 → 关键追问 → 所需证据 → 最小修订。
- **导师式拷问**：每轮只问一个关键问题，不替作者发明结果。
- **局部修改**：只处理授权段落，不重开已确认方向。

## 文件

| 文件 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | 核心流程、触发条件与交付检查 |
| [写作手册](references/writing-playbook.md) | 组织、压缩、过渡句与双语规则 |
| [审查协议](references/review-protocol.md) | 逻辑、证据、方法对应与公平验证 |
| [案例解读](references/urban-3dgs-case.md) | 已确认稿的链条、反例与边界 |
| [来源记录](references/source-ledger.md) | 原始版本和提炼范围 |
| [论证工作表](templates/argument-workbook.md) | 主张、证据、方法、验证 |
| [两页骨架](templates/two-page-proposal.md) | 中英文正文结构 |
| [审查模板](templates/review-report.md) | 最多三项核心缺陷 |
| [独立评审包](templates/blind-review-brief.md) | 仅供实际可隔离委派使用 |
| [范例节选](examples/approved-excerpts.md) | 双语节选及文献快照 |
| [行为用例](evals/behavior-tests.md) | 20个待执行的模型行为测试 |
| [结构校验](scripts/validate_package.py) | 标准库校验，无网络请求 |
| [本地安装器](scripts/install_local.py) | 不覆盖已有目录的复制安装 |
| [展示元数据](agents/openai.yaml) | 可选名称、说明和默认提示 |
| [变更记录](CHANGELOG.md) | 版本与修订范围 |

## 检验与边界

```bash
python scripts/validate_package.py
```

原包已经包含 `name` 和 `description`；缺少本地报错时，不能断定无法使用的原因是格式错误。此次添加安装入口与说明，未在用户电脑上测试实际 Codex 加载；结构校验、复制安装测试与模型行为测试分别报告。

核心写作流程不需要 Python；脚本仅用于可选安装与校验。未打包原始CV、申请书、账号凭据、学号、生日或演示附件。范例文献是原稿快照，并非最新SOTA，也不构成新的科学验证。

安装依据核对日期：2026-10-07。

官方文档：https://developers.openai.com/codex/skills （当前重定向至 https://learn.chatgpt.com/docs/build-skills）。

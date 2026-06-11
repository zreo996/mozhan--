<div align="center">

<img src="ui/static/logo.svg" width="120" alt="墨栈 LOGO">

# 墨栈 · MoZhan

**一栈写尽万千江湖**

为中文长篇网文量身打造的 AI 创作 Skill。
十二大题材 · **九大发布平台** · 文风印记自动注入 · **四步构思向导** · Web UI 一键接入 Claude / GPT / DeepSeek。

</div>

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/tests-49%20passed-brightgreen)](tests/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.com)
[![Genres](https://img.shields.io/badge/%E9%A2%98%E6%9D%90-12%20%E5%A4%A7-purple)](genres/)
[![Platforms](https://img.shields.io/badge/%E5%B9%B3%E5%8F%B0-9%20%E5%AE%B6-magenta)](core/scripts/platform_profiles.py)
[![Version](https://img.shields.io/badge/version-v1.18-orange)](CHANGELOG.md)

[5 分钟上手](docs/GETTING_STARTED.md) · [快速开始](#-快速开始) · [功能介绍](#-它能做什么) · [界面预览](#-界面预览) · [使用文档](SKILL.md) · [更新日志](CHANGELOG.md)

</div>

---

## 🎯 这是什么?

**墨栈 (MoZhan)** 是一个装在 Claude / GPT / DeepSeek 上的中文小说创作 Skill。

**一句话**:你说一个模糊的想法,墨栈帮你一路写到完本 —— 构思 → 大纲 → 章节 → 质量检查 → 平台调优。

### 解决了什么痛点?

| 痛点 | 墨栈的方案 |
|:---|:---|
| AI 写着写着忘了主角叫啥、修为到了哪里 | **四表压缩上下文** + **P0 一致性锚定**,主角名/简介/主题强制注入 |
| 写到第 50 章伏笔忘了交代 | **三层红线 + 自动伏笔追踪**,超期未回收自动标 P0 |
| 番茄和起点的风格天差地别 | **9 大平台调性档案**,书名/简介/字数自动适配 |
| 玄幻不像玄幻,武侠像古偶 | **文风印记自动注入**,每个题材的精品分析经 Prompt Caching 注入 |
| 一章 1 次 API 太烧钱 | **L1-L4 成本分级**,百万字从 18M token 降到 7M(省 61%) |
| 写完不知道质量如何 | **质量评分 + 爽点密度 + 连续性自检**,全部自动化 |

### 适用人群

- 📖 中文网文作者(玄幻/都市/言情/历史/科幻 ……)
- 🤖 AI 工具爱好者(想让 Claude/GPT 真的能"写百万字完本")
- 💻 想搭"长内容生成框架"的开发者(这套系统可复用到其他长内容)
- 🎓 研究 LLM 长期记忆 / 成本优化的团队

---

## 🚀 快速开始

### 1. 装到你的 Claude(30 秒)

```bash
# 1) 克隆仓库
git clone https://github.com/你的用户名/mozhan.git
cd mozhan

# 2) 装依赖
pip install -r requirements.txt

# 3) 链接到 Claude Code skills 目录
mkdir -p ~/.claude/skills
# macOS / Linux
ln -s "$(pwd)" ~/.claude/skills/mozhan
# Windows(PowerShell)
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\mozhan" -Target "$(pwd)"
```

然后在 Claude Code 里说:

> "用 mozhan 帮我写一本武侠,主角是个被除名的剑客。"

### 2. 或:直接跑 Web UI(零配置,2 分钟)

```bash
python -m ui
# Windows 直接双击 scripts\start_ui.bat
```

打开浏览器 → http://127.0.0.1:8765 → 填 API Key → 开始创作

**Web UI 完全中文化**:
- 12 题材卡片 + 9 平台调性
- 四表/简介 tab 中文结构化视图
- 键盘快捷键:`Ctrl+Enter` 提交 / `Esc` 返回
- 一键跳到设置页(没配 API Key 时)

> 📖 完整 Web UI 文档:[ui/README.md](ui/README.md)

### 3. CLI 写作流程

```bash
# 初始化项目
python core/scripts/one_click_writer.py init "我的小说" 玄幻 主角名

# 规划章节
python core/scripts/one_click_writer.py plan 1 L3 大纲.txt

# 写章节
python core/scripts/one_click_writer.py write 1 内容.txt

# 写后自检
python core/scripts/self_check.py 1 --data-dir projects/我的小说/data

# 爽点密度追踪
python core/scripts/climax_tracker.py projects/我的小说/chapters
```

---

## ✨ 它能做什么

### 场景 A:脑子里有画面,想开书

```
1. 打开 Web UI,点「开始构思新书」
2. 选题材(12 选 1) + 平台(9 选 1,可不选)
3. 写三五句话的核心想法
4. 定全本规模(总字数 + 总章节,30 万/100 万/200 万预设)
5. 墨栈一次性返回:书名 / 副标 / 简介 / 主角 / 全章大纲(每章带标题+钩子)
6. 一键建项目,全章纲落到 data/outline_full.json
```

### 场景 B:已经有人设和大纲,直接写

```bash
python core/scripts/one_click_writer.py init "沧海剑歌" wuxia "裴然"
python core/scripts/one_click_writer.py plan  1 outline.txt
python core/scripts/one_click_writer.py write 1 content.txt
```

### 场景 C:写了 50 章,想看连续性

```bash
python core/scripts/self_check.py 50 --data-dir projects/沧海剑歌/data --verbose
python core/scripts/climax_tracker.py projects/沧海剑歌/chapters
```

### 核心特性(七大模块)

| 模块 | 解决什么 | 关键文件 |
|:---|:---|:---|
| 🪄 **四步构思向导** | 从想法到完整章纲 | `ui/app.py` + `core/scripts/llm_client.py` |
| 📡 **9 大平台调性** | 一份稿子适配多平台 | `core/scripts/platform_profiles.py` |
| 🎨 **文风印记** | 题材风格不漂移 | `core/scripts/genre_style.py` + `genres/*/masterpiece_analysis.md` |
| 📋 **四表压缩上下文** | LLM 长期不"失忆" | `core/scripts/post_process.py` + `core/templates/*.json` |
| 🛡 **P0 主角一致性** | 主角名/简介不跑偏 | `ui/app.py` (`build_frozen_system`) |
| 🚦 **三层红线 + 自检** | 自动拦时间倒序/修为倒退 | `core/scripts/self_check.py` + `core/references/red_lines.md` |
| ⚡ **L1-L4 成本分级** | 百万字从 18M → 7M tokens | `core/references/cost_tiers.md` |

---

## 🖼 界面预览

> 跑 `python -m ui` 后访问 `http://127.0.0.1:8765` 即可看到。

| 首页 | 构思向导 · 选题材 | 写作台 |
|:---:|:---:|:---:|
| Hero + 4 数据徽章 + 作品列表 | 12 题材卡片 × 9 平台 | 写作 / 大纲 / 四表 / 文风 / 简介 5 tab |

**Web UI 中文化亮点**(v1.18):
- 「📋 四表」tab 改为中文结构化表格(时间线/角色/伏笔/物品)
- 「ℹ 简介」tab 改为中文卡片(标题/简介/主角/主题/分卷/前三章)
- 默认中文视图,右上角一键切到 JSON 高级模式

> 完整截图见 [docs/screenshots/](docs/screenshots/)(欢迎 PR 你的实际截图)

---

## 🆕 v1.18 新特性

### 🎨 UI 中文化(四表 / 简介 tab)

旧版四表直接 `JSON.stringify` 出来,满是英文字段。新版:
- 时间线分主线/副线/反派线三段,角色按定位显示彩色徽章
- 伏笔 P0 红 / P1 金徽章,自动统计活跃/已回收/休眠
- 物品按武器/线索/丹药/防具/饰品/功法分类
- 简介 tab 改 5 张中文卡片:标题、简介、主角、主题、分卷、前三章钩子

### 📂 仓库整合
- 历史仓库 `novel-writer-universal` 合并到本仓,项目数据迁入 `projects/`
- `projects/` 已在 `.gitignore` 中,**不会被上传到 GitHub**(保护个人作品)

### ⌨️ 交互增强
- 首页 4 数据徽章(12 题材 / 9 平台 / 4 级成本 / 3 层红线)
- 快捷键 `Ctrl+Enter` 提交、`Esc` 返回
- 未配 API Key 时一键跳到「设置」页
- `style.css` 补 `--font-mono` 变量
- 新增 `scripts/verify.ps1` 一键验证脚本

完整变更:[CHANGELOG.md](CHANGELOG.md)

---

## 📂 项目结构

```
mozhan/
├── SKILL.md                   # Skill 主入口(给 Claude 看的)
├── README.md                  # 你在这里
├── CHANGELOG.md               # 变更日志
├── ITERATION_LOG.md           # 三模型辩证迭代史
├── GITHUB_UPLOAD_GUIDE.md     # GitHub 上传指南
├── LICENSE                    # MIT
├── requirements.txt
│
├── core/
│   ├── scripts/               # 11 个核心 Python 脚本
│   │   ├── self_check.py            # 五重自检(P0 红线)
│   │   ├── post_process.py          # 写后四表更新
│   │   ├── quality_score.py         # 质量评分
│   │   ├── one_click_writer.py      # 一键写作 CLI
│   │   ├── llm_client.py            # LLM 客户端 + 冻结上下文
│   │   ├── continuity_enforcer.py   # P0 状态机
│   │   ├── climax_tracker.py        # 爽点密度追踪
│   │   ├── genre_style.py           # 文风印记(注入 Prompt)
│   │   ├── genre_validator.py       # 题材风格校验
│   │   ├── platform_profiles.py     # 9 平台调性档案
│   │   ├── multi_agent.py           # 多 Agent 会诊
│   │   └── auto_update.py           # 事件驱动自动迭代
│   ├── references/            # 12 篇参考文档
│   └── templates/             # 7 个 JSON/MD 模板
│
├── genres/                    # 12 大题材
│   └── {fantasy|urban|scifi|historical|game|apocalypse|superpower|
│         fanfiction|mystery|wuxia|romance|isekai}/
│       ├── worldbuilding.md
│       ├── red_lines.md
│       ├── plot_templates.md
│       └── masterpiece_analysis.md  # 文风印记来源
│
├── examples/                  # 武侠最小可运行示例
│   └── demo_wuxia/
│
├── commands/                  # 7 个 slash command 文档
├── ui/                        # Flask Web UI(完全中文化)
│   ├── app.py
│   ├── templates/index.html
│   ├── static/{app.js, style.css, logo.svg}
│   └── README.md
│
├── tests/                     # 49 个 pytest 测试
├── scripts/
│   ├── start_ui.sh / .bat     # UI 启动
│   └── verify.ps1             # 一键验证
│
├── docs/                      # 文档与截图
└── projects/                  # 用户真实项目(.gitignore 排除,不上传)
```

---

## 🧪 测试

```bash
pip install pytest flask
pytest tests/ -v
```

**49 个测试覆盖**:连续性强制器、爽点追踪、文风印记、题材校验、LLM 客户端、评分、写后处理。

Windows 用户也可以:

```powershell
.\scripts\verify.ps1
# 自动跑 14 个 py 文件 py_compile + pytest,结果写入 verify.log
```

---

## 🛣 Roadmap

- [x] v1.10 基础整合
- [x] v1.11 中文网文节奏引擎 + 题材兼容矩阵
- [x] v1.12 三方辩证共识
- [x] v1.13 补全 + 27 个测试 + 12 题材红线
- [x] v1.14 文风印记自动注入
- [x] v1.15 Web UI 首次发布
- [x] v1.16 品牌"墨栈" + 三步构思向导
- [x] v1.17 **9 平台调性 + 四步向导 + P0 一致性锚定**
- [x] v1.18 **UI 中文化(四表/简介 tab) + 仓库整合**
- [ ] v1.19 多 Agent 会诊实装
- [ ] v1.20 RAG 检索跨章文风一致性
- [ ] v1.21 桌面端(Tauri / Electron)打包

完整迭代史:[`ITERATION_LOG.md`](ITERATION_LOG.md)

---

## 🤝 贡献

欢迎 PR!特别需要:

- 🌐 **国际化翻译**(把 `app.js` 里的中文翻译成英文/日文等)
- 🖼 **真实 UI 截图**(放到 `docs/screenshots/`)
- 🐛 **Bug 报告**(用 Issue 模板)
- 📚 **新题材**(12 题材之外,比如克苏鲁、规则怪谈)
- 🎨 **新文风印记**(把更多精品分析喂给 `genre_style.py`)

详见 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 📜 许可证

[MIT](LICENSE) — 自由使用,保留版权即可。

---

## 🙏 致谢

- **[novel-writer-inkos-fusion](https://github.com/...)** — 早期整合的母版
- **DeepSeek / MiniMax / Claude-Opus-4.7** — 三方辩证对手,贡献了 5 个致命缺陷定位
- **Anthropic Claude** — 主力写作模型
- **51 部网文精品** — 12 题材的文风印记都来自它们的 `masterpiece_analysis.md`

---

## ⭐ Star History

如果这个项目对你有帮助,点个 ⭐ 鼓励一下!

---

<div align="center">

**墨栈 · MoZhan**  
*一栈写尽万千江湖*

[⬆ 回到顶部](#-墨栈--mozhan)

</div>



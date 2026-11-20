# UI与交互设计准则

一份面向AI辅助开发的精简、框架中立的UI与交互Skill。目标是在实现功能的同时，保持页面布局、操作反馈、移动适配与视觉层级清晰，减少反复返工。

## 来源与维护

本项目由 **liuxiaogang-com** 发起并维护，内容来自其实际项目经验、界面观察与持续积累的UI和交互偏好，经AI辅助整理为可执行的短规则。

这些规则来自实践经验与设计取舍。项目的明确约束优先。待确认偏好不作为默认规则。

唯一发布与更新仓库：https://github.com/liuxiaogang-com/ui-interaction-guidelines

## 设计思路

准则分两层：

- **交互层（严格执行）**：开工前流程、四条交互原则、组件行为规范、交付前验收。这一层解决「莫名其妙的刷新」「点击后在原位置插入表单」「一排堆满按钮」这类操作逻辑问题。
- **视觉层（自由发挥）**：只要求先声明视觉方向、全局统一变量，并避开少量模板化做法。具体风格由Agent按项目决定，避免千篇一律。

组件规范只写用户能验证的行为，不规定外观。每个组件分「通用」「桌面」「移动」三段，Agent只读取当前任务涉及的组件文件。

## 文件说明

```text
ui-interaction-guidelines/
├── SKILL.md                  # 主文件：开工流程、交互原则、视觉层、形态对照表、组件索引、验收清单
├── references/
│   ├── components/           # 22 个组件的行为规范
│   ├── patterns/             # 流程规范（登录注册）
│   └── setup/                # 新项目初始化：基线样式、Browserslist、前缀、Stylelint、截图方式
├── templates/
│   └── AGENTS.md             # 复制到项目中的兜底规则与写作风格要求
├── INSTALL.md                # 给 Agent 阅读的安装步骤
└── README.md
```

公开仓库不包含私人聊天、内部来源记录或凭据。

## 安装

### 方式一：让 Agent 自己安装（推荐）

对任意 Agent 说一句：

> 阅读 https://raw.githubusercontent.com/liuxiaogang-com/ui-interaction-guidelines/main/INSTALL.md ，按步骤帮我安装这个 Skill。

Agent 会选择合适的安装方式，并询问你是否在当前项目加入兜底规则。

### 方式二：命令行

```bash
npx skills add liuxiaogang-com/ui-interaction-guidelines -g
```

使用 Vercel 的 [Skills CLI](https://github.com/vercel-labs/skills)，会安装到本机已有的 Claude Code、Codex、Cursor 等 Agent 中。该工具默认收集匿名使用数据，设置环境变量 `DO_NOT_TRACK=1` 可关闭。

### 方式三：git 克隆

```bash
git clone --depth 1 https://github.com/liuxiaogang-com/ui-interaction-guidelines.git ~/.claude/skills/ui-interaction-guidelines
```

把目标路径换成你所用 Agent 的 Skill 目录，各 Agent 的目录见 [INSTALL.md](INSTALL.md)。

### 项目兜底规则（推荐，Codex 尤其需要）

Agent 是否加载 Skill，取决于它对任务的判断。以功能为主的任务（如「加一个导出功能」）可能不会触发 Skill。

把 `templates/AGENTS.md` 的内容加入项目根目录的 `AGENTS.md`（Codex、Cursor）或 `CLAUDE.md`（Claude Code），替换其中的占位内容。这个文件每次都会加载，能保证最重要的规则始终生效。用方式一安装时，Agent 会询问并帮你完成这一步。

不支持 Skill 的工具，可以在规则文件中写明 `SKILL.md` 的路径，让它在涉及 UI 的任务中先读取。

新项目直接遵守。已有项目调整整体布局和滚动方式前，必须取得用户明确同意。

## 补充规则

遇到Agent犯了新的错误时，在对应组件文件中补一条规则，并在「常见错误」里记录反例。写法要求：

- 只写行为，不写外观。
- 一句话只讲一个结论，写成可验证的结果。
- 避免「可以…但不…，也不…」这类转折句。Agent会模仿规则的文风。

## 更新

### 自动更新

Agent 在每个会话中首次使用本 Skill 时，会检查一次更新：

- git 安装：执行 `git pull --ff-only`。
- Skills CLI 安装：执行 `npx skills update ui-interaction-guidelines -y`。
- 手动复制安装：无法自动更新。

检查失败时 Agent 会直接跳过，继续当前任务，并在验收清单中注明。有更新时，Agent 会重新读取规则并告诉你。

以下情况会跳过自动更新，需要手动更新：

- 网络不可用。
- Agent 运行在禁止联网或禁止写入 Skill 目录的沙箱中（如 Codex 的默认沙箱）。

不想自动更新时，在项目规则文件中写一句：「不要自动更新 ui-interaction-guidelines」。

### 手动更新

对 Agent 说「更新 ui-interaction-guidelines Skill」，或自己运行：

```bash
# Skills CLI 安装
npx skills update ui-interaction-guidelines

# git 安装
git -C <Skill 目录>/ui-interaction-guidelines pull --ff-only
```

规则更新统一提交到本仓库的 `main` 分支。每次更新的内容可在 Git 提交记录中查看。

## 支持这个项目

如果这份准则对你有帮助，欢迎点个 Star，也可以通过微信赞赏支持后续整理与维护。感谢支持！

<img src="assets/donation.jpg" alt="微信赞赏码：给 LXG 赞赏" width="240">

[查看赞赏码原图](assets/donation.jpg)

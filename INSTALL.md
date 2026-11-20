# 安装 ui-interaction-guidelines（供 Agent 阅读）

你正在为用户安装一个 UI 与交互规范 Skill。按顺序执行以下步骤。某一步失败时，按该步的「备选」处理。

命令中的 `~` 指用户主目录。在 PowerShell 中运行原生命令时，把 `~` 换成主目录的绝对路径。

## 1. 安装

**首选：Skills CLI**（需要 Node.js）

```bash
npx skills add liuxiaogang-com/ui-interaction-guidelines -g -y
```

它会识别本机已安装的 Agent，并安装到各自的用户级 Skill 目录。

**备选：git 克隆**（没有 Node.js，或上面的命令失败时）

```bash
git clone --depth 1 https://github.com/liuxiaogang-com/ui-interaction-guidelines.git <Skill 目录>/ui-interaction-guidelines
```

`<Skill 目录>` 按当前 Agent 选择：

| Agent | 用户级 Skill 目录 |
|---|---|
| Claude Code | `~/.claude/skills` |
| Codex | `~/.agents/skills`（新会话中未识别时改用 `~/.codex/skills`） |
| Cursor | `~/.cursor/skills` |
| Gemini CLI | `~/.gemini/skills` |
| 其他 | 查阅该 Agent 文档中的 Skill 目录 |

当前 Agent 不支持 Skill 时，仍用上面的 git 命令克隆到任意目录（如 `~/ui-interaction-guidelines`），然后直接执行第 3 步。

## 2. 验证

确认安装位置下存在以下文件：

- `ui-interaction-guidelines/SKILL.md`
- `ui-interaction-guidelines/references/components/`（应有 22 个文件）

## 3. 项目兜底规则（先询问用户）

询问用户：是否在当前项目中加入兜底规则。用户同意后执行：

1. 读取 `templates/AGENTS.md`。
2. 向用户确认本项目的平台（桌面 / 移动 / 响应式）和组件库，替换模板中的占位内容。
3. 把内容追加到项目根目录的规则文件。文件已存在时追加到末尾，禁止覆盖原有内容。

| Agent | 规则文件 |
|---|---|
| Codex、Cursor 及其他支持 AGENTS.md 的工具 | `AGENTS.md` |
| Claude Code | `CLAUDE.md`（已有 AGENTS.md 时，可在 CLAUDE.md 中写一行 `@AGENTS.md` 引用） |
| Gemini CLI | `GEMINI.md` |

当前 Agent 不支持 Skill 时，在规则文件中写明 `SKILL.md` 的完整路径，要求涉及 UI 的任务先读取它。

## 4. 汇报

用三行告诉用户：
- 安装方式和安装位置。
- 是否加入了项目兜底规则。
- 重启 Agent 或新开会话后生效。

## 更新

Skill 会在每个会话首次使用时自动检查一次更新，规则见 `SKILL.md` 的「〇、检查更新」。

手动更新：

```bash
# Skills CLI 安装
npx skills update ui-interaction-guidelines -y

# git 安装
git -C <Skill 目录>/ui-interaction-guidelines pull --ff-only
```

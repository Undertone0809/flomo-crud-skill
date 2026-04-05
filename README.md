# flomo-skills

Production-ready flomo skills for agent workflows.

这个仓库提供 3 个可独立安装的 flomo agent skills，分别覆盖：

- `mac` 上更快的本地登录态 + API 工作流
- Windows / 非 `mac` 上的 Web UI 自动化工作流
- 面向 AI / NotebookLM 的 Markdown 导出工作流

它不是一个“大而全”的 flomo 框架，而是一个清晰的技能仓库。根 README 是唯一入口，不再把说明拆散到多个子 README 里。

## What is inside

| Skill | 主要用途 | 适合谁 |
| --- | --- | --- |
| `flomo-local-api` | 查询、总结、创建、编辑、tag 复用 | `mac` 用户 |
| `flomo-web-crud` | 通过浏览器做 live flomo CRUD | Windows / 非 `mac` 用户 |
| `flomo-memo-to-markdown` | 导出 Markdown、tag 统计、NotebookLM 素材 | 需要归档或 AI 消费的用户 |

## The important routing rule

`flomo-web-crud` 能做的事，`flomo-local-api` 在 `mac` 上基本都能做。

区别不在“功能能不能做”，而在“用哪条路径更合理”：

- 对 `mac` 用户，默认只需要安装 `flomo-local-api`
- 对 Windows 或其他非 `mac` 用户，默认安装 `flomo-web-crud`
- 对导出场景，无论平台，直接用 `flomo-memo-to-markdown`

换句话说：

- `flomo-local-api` 是 `mac` 用户的默认主力技能
- `flomo-web-crud` 主要是给 Windows 用户和浏览器场景准备的
- `flomo-memo-to-markdown` 是独立导出技能，不是 CRUD 附属能力

## Why `flomo-local-api` is better on mac

如果你在 `mac` 上，优先用 `flomo-local-api`。原因很实际：

1. 更快  
   走本地登录态 + API，不需要打开 flomo Web 页面，不需要浏览器自动化。

2. 更稳  
   不依赖 UI 结构，不会因为按钮位置、弹窗状态或页面布局变化而变脆。

3. 更适合高频使用  
   查 memo、总结最近想法、快速写一条 memo，这些日常动作都比 Web 自动化顺手很多。

4. 写 memo 时会复用你现有的 tag 体系  
   这是 `flomo-local-api` 很重要的优点。它会先扫描你已有的 tag，再优先复用成熟 tag，而不是随手新造一堆新标签。长期用下来，检索质量会明显更好。

## Typical cases

| 真实场景 | 推荐 skill | 为什么 |
| --- | --- | --- |
| “帮我看看最近 30 天我在反复写什么” | `flomo-local-api` | 这是标准查询 + 总结场景，`mac` 上没必要走 Web UI |
| “我想快速记一条 memo，并尽量沿用我已有的 tag” | `flomo-local-api` | 它会先看已有 tag，再写入，更适合长期维护 tag 体系 |
| “我在 Windows 上，想查几条 memo 再改掉其中一条” | `flomo-web-crud` | 非 `mac` 默认走浏览器路径 |
| “我想删掉一条 flomo memo” | `flomo-web-crud` | 删除是 live account 操作，Web UI 路径更自然 |
| “导出 2025 年 memo，按季度拆分，喂给 NotebookLM” | `flomo-memo-to-markdown` | 这是独立导出场景，不该混进 CRUD skill |
| “我想把 flomo 当成长周期知识输入源喂给 AI” | `flomo-memo-to-markdown` | 重点是 Markdown 切分和 tag 统计，不是交互式查改 |

## Installation

### mac 用户

大多数情况下，你只需要安装 `flomo-local-api`：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-local-api
```

如果你还需要导出 Markdown，再额外安装：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-memo-to-markdown
```

### Windows / 非 mac 用户

默认安装 `flomo-web-crud`：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-web-crud
```

## Skill details

### `flomo-local-api`

- 面向 `mac`
- 依赖本地 flomo 登录态
- 负责查询、总结、创建、编辑、tag 复用
- 写 memo 时优先复用你现有的 tag
- 不负责删除 memo

入口：
- [SKILL.md](./.agents/flomo-local-api/SKILL.md)

### `flomo-web-crud`

- 面向任意平台，主要给 Windows / 非 `mac` 用户使用
- 不依赖 flomo 官方 API
- 依赖 Chrome MCP 和已登录浏览器
- 可以做 live Web UI 的 query / create / edit / delete
- 在 `mac` 上不是默认入口，只在本地 API 不可用或必须走浏览器时使用

入口：
- [SKILL.md](./.agents/flomo-web-crud/SKILL.md)

### `flomo-memo-to-markdown`

- 面向导出、归档、NotebookLM、长文本 AI 消费
- 依赖本地 flomo 登录态和 API 读取能力
- 负责 Markdown 分片、tag 统计、附件处理
- 不承接“查找并修改一条 memo”这类交互

入口：
- [SKILL.md](./.agents/flomo-memo-to-markdown/SKILL.md)

## Repository layout

```text
.agents/
  flomo-local-api/
  flomo-web-crud/
  flomo-memo-to-markdown/
```

每个 skill 都自带自己的 `SKILL.md`、配置、脚本和 references。这个仓库故意不做 `shared/`，也不在各 skill 目录下再放重复 README，避免入口分裂。

## Discoverability

可以先用这个命令确认远端仓库能被 `skills` CLI 正常识别：

```bash
npx skills add https://github.com/Undertone0809/flomo-skills --list --full-depth
```

公开仓库被正常安装后，也会逐步进入 [skills.sh](https://skills.sh/) 的可发现路径。

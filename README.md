# flomo-skills

让 AI 能直接操作你的 flomo 笔记，也能基于 flomo 分析你自己。

```bash
# Mac 用户，先装这个
npx skills add Undertone0809/flomo-skills --skill flomo-local-api

# Windows 用户，装这个
npx skills add Undertone0809/flomo-skills --skill flomo-web-crud
```

## 一句话选技能

- **Mac 用户** → 先装 `flomo-local-api`
- **Windows / 非 Mac 用户** → 装 `flomo-web-crud`
- **要导出 Markdown** → 再装 `flomo-memo-to-markdown`
- **要基于 flomo 做自我分析** → 在 Mac 上再装 `flomo-analysis-studio`

## What

flomo-skills 是一组面向 Claude Code 等 AI Agent 的 flomo skills。

安装后，你可以直接对 AI 说：

- “帮我总结最近一周记的关于 AI 的想法”
- “记一条 memo，尽量复用我已有的 tag”
- “找出我今年用得最多的 tag”
- “基于我的 flomo，分析一下我最近到底在想什么”
- “帮我把笔记里的困惑转换成行动建议”

AI 会根据你装的 skill，直接走本地 API、浏览器自动化，或者高层分析流程，不需要你手动复制粘贴。

## 包含的技能

| 技能 | 用途 | 适用平台 |
| --- | --- | --- |
| `flomo-local-api` | 查询、创建、编辑、总结、tag 分析 | Mac |
| `flomo-web-crud` | 查询、创建、编辑、删除（浏览器自动化） | Windows / 非 Mac |
| `flomo-memo-to-markdown` | 导出 Markdown、NotebookLM / AI 素材 | Mac |
| `flomo-analysis-studio` | 基于 flomo 做高层自我分析 | Mac，且需先有 `flomo-local-api` |

## 最重要的路由规则

`flomo-web-crud` 能做的事，`flomo-local-api` 在 Mac 上基本都能做。

区别不在“能不能做”，而在“哪条路径更合理”：

- 对 **Mac 用户**，默认只需要安装 `flomo-local-api`
- 对 **Windows / 非 Mac 用户**，默认安装 `flomo-web-crud`
- 对 **导出场景**，单独用 `flomo-memo-to-markdown`
- 对 **自我分析场景**，在 Mac 上用 `flomo-analysis-studio`，底层仍然走 `flomo-local-api`

唯一明显的功能差异是：

- `flomo-web-crud` 支持删除 memo
- `flomo-local-api` 出于安全边界，不提供删除

## 为什么 Mac 用户应该先装 `flomo-local-api`

### 1. 更快

直接走本地登录态 + API，不需要打开 flomo Web 页面，不需要浏览器自动化。

### 2. 更稳

不依赖 UI 结构，不会因为 flomo 改版、按钮位置变化、弹窗状态变化而变脆。

### 3. 更适合高频使用

查 memo、快速记一条、改一条、做最近总结，这些高频动作都更适合本地 API 路线。

### 4. 写 memo 时会复用你现有的 tag

这是 `flomo-local-api` 很重要的优点。

它会先扫描你已有的 tag 体系，再优先复用成熟 tag，而不是随手新造一堆新标签。长期用下来，你的 tag 系统会更稳，检索质量也会更高。

## Typical cases

| 真实场景 | 推荐 skill | 为什么 |
| --- | --- | --- |
| “帮我看看最近 30 天我在反复写什么” | `flomo-local-api` | 标准查询 + 总结场景，Mac 上没必要走 Web UI |
| “我想快速记一条 memo，并尽量沿用我已有的 tag” | `flomo-local-api` | 它会先看已有 tag，再写入 |
| “我在 Windows 上，想查几条 memo 再改掉其中一条” | `flomo-web-crud` | 非 Mac 默认走浏览器路径 |
| “我想删掉一条 flomo memo” | `flomo-web-crud` | 删除是 live account 操作，浏览器路径更自然 |
| “导出 2025 年 memo，按季度拆分，喂给 NotebookLM” | `flomo-memo-to-markdown` | 这是独立导出场景 |
| “基于我的 flomo，分析一下我最近到底在想什么” | `flomo-analysis-studio` | 这是高层自我分析，不是原始查询 |
| “帮我从笔记里找盲区，或者把困惑变成行动建议” | `flomo-analysis-studio` | 这是解释自己，不是 CRUD |

## 安装

### Mac 用户

大多数情况下，你先装这个就够了：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-local-api
```

如果你还需要导出 Markdown：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-memo-to-markdown
```

如果你还想基于 flomo 做高层自我分析：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-analysis-studio
```

### Windows / 非 Mac 用户

默认安装：

```bash
npx skills add Undertone0809/flomo-skills --skill flomo-web-crud
```

## 各 skill 负责什么

### `flomo-local-api`

- 面向 Mac
- 负责查询、总结、创建、编辑、tag 复用
- 写 memo 时优先复用你现有的 tag
- 不负责删除 memo

入口：
- [SKILL.md](./.agents/flomo-local-api/SKILL.md)

### `flomo-web-crud`

- 面向 Windows / 非 Mac 用户
- 通过浏览器完成 live flomo CRUD
- 支持删除
- 在 Mac 上不是默认入口，只在本地 API 不可用或必须走浏览器时使用

入口：
- [SKILL.md](./.agents/flomo-web-crud/SKILL.md)

### `flomo-memo-to-markdown`

- 面向导出、归档、NotebookLM、AI 长文本消费
- 负责 Markdown 分片、tag 统计、附件处理
- 不承接“查找并修改一条 memo”这类交互

入口：
- [SKILL.md](./.agents/flomo-memo-to-markdown/SKILL.md)

### `flomo-analysis-studio`

- 面向 Mac 用户的高层自我分析
- 底层用 `flomo-local-api` 取数据
- 支持总览、ACT 视角、复利飞轮、行动指南、盲区探索、MBTI 风格解读
- 当用户不知道要分析什么时，会先给出值得继续分析的方向

入口：
- [SKILL.md](./.agents/flomo-analysis-studio/SKILL.md)

## 项目结构

```text
.agents/
  flomo-analysis-studio/
    SKILL.md
  flomo-local-api/
    SKILL.md
  flomo-web-crud/
    SKILL.md
  flomo-memo-to-markdown/
    SKILL.md
```

根 README 是唯一入口，不再在每个 skill 目录下放重复 README。

## Discoverability

可以先用这个命令确认远端仓库能被 `skills` CLI 正常识别：

```bash
npx skills add https://github.com/Undertone0809/flomo-skills --list --full-depth
```

公开仓库被正常安装后，也会逐步进入 [skills.sh](https://skills.sh/) 的可发现路径。

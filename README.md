# 用 Codex 写 Figma 设计稿

`figma-editable-design` 是一个可复用的 Codex Skill：把需求梳理、Figma 画稿、按反馈修改和视觉检查串成同一套工作方法，交付可编辑的图层、节点链接和预览。以后不用每次重写一份长 Prompt。

它包含当前使用过的 [Grab / cursor-talk-to-figma-mcp](https://github.com/grab/cursor-talk-to-figma-mcp) 接入说明，也能复用接收者已有的官方 Figma 写入工具。Skill 提供工作方法；实际读写仍由插件／MCP 完成，需要接收者自己的 Figma 账号和文件编辑权限。

## 下载

最方便的方式是下载 [完整 Skill 压缩包 v1.0.0](https://github.com/yaowenhu-pm/figma-editable-design/releases/download/v1.0.0/figma-editable-design.zip)。解压后会得到 `figma-editable-design` 文件夹，里面直接有 `SKILL.md`。

要获取仓库最新内容，也可以在本页点击 **Code → Download ZIP**，或下载 [main 分支 ZIP](https://github.com/yaowenhu-pm/figma-editable-design/archive/refs/heads/main.zip)。仓库压缩包多一层外壳：解压后取出其中的 **`figma-editable-design` 子文件夹**安装，不要把整个仓库外壳当成 Skill。

习惯使用 Git 的人可以运行：

```bash
git clone https://github.com/yaowenhu-pm/figma-editable-design.git
```

## 交给自己的 Codex 安装

把下载的 ZIP 交给 Codex，或把仓库地址和下面这段话一起发给它：

```text
请下载 https://github.com/yaowenhu-pm/figma-editable-design 中的
figma-editable-design 文件夹，安装为我的 Codex Skill。
保留 SKILL.md、agents/openai.yaml 和 references/figma-connection.md 的目录关系。
先核对当前版本支持的用户 Skills 目录；已有同名 Skill 时先比对并保留备份。
安装后读取 SKILL.md，确认可以调用 $figma-editable-design。
优先复用我已有的 Figma 写入工具；如果没有，请按接入说明配置 Grab TalkToFigma，
保留其他 MCP 配置，并告诉我需要在 Figma 插件中完成的操作。
```

当前 [Codex Skills 官方文档](https://learn.chatgpt.com/docs/build-skills) 推荐用户目录 `~/.agents/skills`；项目内可放在 `.agents/skills`。Windows 用户目录通常对应 `%USERPROFILE%\.agents\skills`。旧环境可能使用 `~/.codex/skills`，由接收者的 Codex 核对实际支持的位置，避免重复安装。安装后若还未识别，刷新或重新打开 Codex 会话。

例如安装到当前用户目录后，结构应是：

```text
~/.agents/skills/figma-editable-design/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── figma-connection.md
```

请保留完整目录，单独下载 `SKILL.md` 会缺少接入说明。

## 第一次使用

```text
使用 $figma-editable-design。
Figma 文件：<你的文件链接>
本轮需求：<要设计或修改的页面>
主要用户与任务：<谁用它完成什么事>
参考与限制：<品牌、参考稿或需要保留的内容>
先完成一张核心页面，写成可编辑图层，给我节点链接和预览。
```

继续修改时可以直接说：

```text
使用 $figma-editable-design。修改这个节点：<Figma 节点链接>。
把右侧改成可收起的聊天区，输入框固定在底部，去掉重复按钮。
保留其余内容，完成后检查是否重叠或溢出。
```

## 连接 Figma

已有官方 Figma 写入工具时可以直接复用；只有读取工具时，需要补充写入通路。选择 Grab 路线时，按 [完整接入说明](figma-editable-design/references/figma-connection.md) 配置 Git、Bun、WebSocket relay、Codex MCP 和 Figma 插件，并使用插件本次显示的 channel 连接。两条路线选一条即可。

连接验证应包括：会话中能调用工具、核对正确文件和页面、在授权对象上写入并回读节点、查看导出预览。`codex mcp list` 只证明服务器已注册。

接入说明中的特定提交及官方插件版本是维护者环境的验证记录，不是接收者必须使用的版本，也不代表其电脑已连接成功。本包不包含第三方工具源码、账号、令牌或私人设计材料。

## 文件内容

| 文件 | 用途 |
| --- | --- |
| [SKILL.md](figma-editable-design/SKILL.md) | 设计流程、可编辑图层要求、回读和视觉检查 |
| [agents/openai.yaml](figma-editable-design/agents/openai.yaml) | Codex 中的显示名称与默认调用提示 |
| [references/figma-connection.md](figma-editable-design/references/figma-connection.md) | 工具项目、MCP 配置、插件连接和验证方法 |

首版核对日期：2026-10-03。已通过 Skill 结构校验和接入流程审阅；接收者环境的端到端连接需要首次使用时验证。

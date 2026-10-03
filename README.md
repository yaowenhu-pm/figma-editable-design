# 用 Codex 写 Figma 设计稿

让 Codex 直接在 Figma 里画稿、按反馈修改，得到可编辑的设计稿。把设计流程沉淀成 Skill，以后不用反复写长 Prompt。

## 从 GitHub 安装

把下面这段话发给自己的 Codex，由它直接从仓库获取并安装：

```text
请从 https://github.com/yaowenhu-pm/figma-editable-design
获取 figma-editable-design 子目录，安装到我当前 Codex 支持的用户 Skills 目录。
保留完整目录；已有同名 Skill 时先比对并备份。
安装后读取 SKILL.md，并帮我连接 Figma，优先复用已有的 Figma 写入工具。
```

## 使用

连接完成后，提供 Figma 链接和需求：

```text
用 $figma-editable-design。
Figma 文件：<你的文件链接>
需求：<要设计或修改什么>
先画一张核心页面，给我可编辑的设计稿、节点链接和预览。
```

继续修改时，直接给出节点链接和修改意见即可。

## 连接 Figma

绘图由 Figma 插件／MCP 完成，需要你自己的 Figma 账号和文件编辑权限。优先复用已有的写入工具，也可使用当前采用的 [Grab / cursor-talk-to-figma-mcp](https://github.com/grab/cursor-talk-to-figma-mcp)。

详细配置见 [Figma 接入说明](figma-editable-design/references/figma-connection.md)，工作流程见 [SKILL.md](figma-editable-design/SKILL.md)。

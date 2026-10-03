# Figma 写入工具接入

核对日期：2026-10-03。具体工具、权限和安装方式仍以接收者当前环境为准。

## 当前项目与适用路线

本 Skill 包含实际使用过的工具项目 [Grab / cursor-talk-to-figma-mcp](https://github.com/grab/cursor-talk-to-figma-mcp)。本机历史记录表明，该项目通过本地 bridge 完成过读取、原生图层写入与 PNG 导出；对应提交为 `ddd90f3a6d454ea0b2fc29f1b084f50fd062b880`。这是可追溯的使用记录，不代表对方电脑已安装、当前服务在运行或新机 Codex 注册已验证。

当前 Codex 环境另有官方 Figma 插件 15.0.0，并暴露 `use_figma` 等工具。对方已有此类写入工具时优先复用，先遵守工具要求的 Skill 与接口。仅连接读取设计的 MCP 时，仍需核对写入能力。两条路线择一即可，不需要为了使用本 Skill 同时安装。

## 官方插件已经可用时

查看当前工具清单和授权账号，核对目标文件可编辑。调用前加载工具要求的 Figma Skill；通过只读请求核对目标后，再按本轮需求写稿。若只有读取接口，说明缺少写入通路，不凭名称推断支持。

## 使用 Grab TalkToFigma

此路线由 Figma 插件、WebSocket relay 和 stdio MCP server 配合工作。用户需要在自己的 Figma 账号中打开有编辑权限的文件并运行插件。

### 取得工具与运行时

检查接收者的操作系统、Git、Bun 和 Codex CLI。按仓库当前 README 取得代码和依赖，记录使用的提交。已有工具目录可复用，但先核对 remote、代码和实际路径。

```text
git clone https://github.com/grab/cursor-talk-to-figma-mcp.git
```

relay 使用端口 3055。启动前核对源码中的监听设置；同机使用时设置为本机回环地址，并核对端口占用，复用已确认属于本任务的服务。不要为了连接而扩大到局域网或添加开机启动。历史本机版本做过 `127.0.0.1` 监听及 Figma Origin 校验；这些本地修改没有随此 Skill 分发，应检查取得源码的当前行为。

确认监听设置后，在取得的仓库目录运行：

```text
bun install
bun run src/socket.ts
```

若 Bun 未安装，按 [Bun 官方安装文档](https://bun.sh/docs/installation) 选择当前平台的方式；不要假设 Windows 有 bash。README 的 `bun setup` 针对 Cursor 配置，不作为 Codex 注册步骤。

启动后核对 relay 的实际运行状态，再继续注册和连接。

### 注册到接收者的 Codex

先确认接收者要求配置写入通路，然后按当前 `codex mcp add --help` 和 [Codex MCP 官方文档](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) 注册。保留已有配置，不直接复制 Cursor 的 `mcpServers` JSON 到 TOML。

以下是模板，必须由 Codex 解析并替换成接收者电脑的真实绝对路径，不能照抄尖括号占位符：

```toml
[mcp_servers.talk_to_figma]
command = "<BUN_EXE_ABS_PATH>"
args = ["<REPO_ABS_PATH>/src/talk_to_figma_mcp/server.ts"]
```

也可使用 CLI 注册同一 stdio 入口；不要再重复写一份同名配置：

```text
codex mcp add talk_to_figma -- "<BUN_EXE_ABS_PATH>" "<REPO_ABS_PATH>/src/talk_to_figma_mcp/server.ts"
codex mcp list
```

Windows TOML 路径可使用正斜杠。新增 MCP 后按接收者 Codex 版本刷新或重新打开会话，确认工具确实出现在会话中。`mcp list` 出现服务器只证明注册状态，连接成功还需后面的实际调用。

### 在正确文件里连通插件

运行 [TalkToFigma 社区插件](https://www.figma.com/community/plugin/1485687494525374295/cursor-talk-to-figma-mcp-plugin)，或在 Figma 支持的开发插件入口导入仓库中的 `src/cursor_mcp_plugin/manifest.json`。历史显示名称可能是 `Cursor MCP Plugin`；使用 Codex 不需要另外安装 Cursor。

读取插件本次显示的 channel，并调用实际 MCP 工具 `join_channel`，参数 `channel` 使用这个值。随后调用 `get_document_info` 和 `get_selection` 核对文件、页面和选中对象。Figma 文件切换或插件重开后重新检查连接和目标。

MCP server 默认连接 `localhost:3055`。历史源码对自定义 `--server=127.0.0.1` 的协议处理存在差异，因此不要自行加这个参数；遇到连接错误先查看当前版本源码与日志。

### 写入与完成验证

按本次工具 schema 使用创建、布局与样式能力，例如 `create_frame`、`create_text`、`set_layout_mode`、`set_padding`、`set_item_spacing`。用 `get_node_info` 回读目标节点，使用 `export_node_as_image` 取得预览并查看。若工具导出返回编码数据，应按返回格式保存并查看，不把“返回了数据”当作视觉检查完成。

核对结果之前，不宣称已打通写入。连接测试仅在用户授权的设计对象上进行；不要额外修改无关文件或生产设计。

## 已知限制与维护

- 本 Skill 只打包工作方法和接入说明，不打包第三方 MCP 源码、Figma 插件或私人凭据。
- Grab 本地通路依赖已登录的 Figma、正在运行的插件和匹配的 channel；上述历史通路没有使用 Figma REST PAT。官方插件按其 connector 授权方式使用。
- 官方插件与 Grab 的调用协议不同，能力以当前会话工具为准。
- 用户只要求设计稿时，复用已有连接即可；没有获准安装配置时，给出具体接入步骤，继续准备布局，等待必要的连接操作。

来源：[Grab 项目 README](https://github.com/grab/cursor-talk-to-figma-mcp)、[Codex MCP](https://learn.chatgpt.com/docs/extend/mcp?surface=cli)、[Codex Skills](https://learn.chatgpt.com/docs/build-skills)。

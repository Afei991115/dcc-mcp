# 本机 DCC MCP

这台 Windows 电脑上，Cursor 用户级 MCP（`%USERPROFILE%\.cursor\mcp.json`）里和 DCC 相关的五条。配置快照在 [`cursor/mcp.json`](cursor/mcp.json)。

只收录 Unity、Spine、3ds Max、Blender、Substance 3D Designer。不含数据库、SVN、Jira 等其它服务。

| Cursor 名称 | 启动方式 | 本机状态 | 上游 |
| --- | --- | --- | --- |
| `unityMCP` | HTTP `127.0.0.1:10090/mcp` | 编辑器里的包 `com.coplaydev.unity-mcp` **10.3.1-beta.7**，路径 `D:\UnityProject\GT\LocalPackage\MCPForUnity` | [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp)（MIT） |
| `spine-mcp` | `node` 跑本地 `dist/index.js` | 源码 `D:\workspace\mcp\spine-mcp-server`，包版本 **0.1.1**，`SPINE_EXE=D:\Spine\Spine.com` | 已在 [zhoushengmin/spine-mcp-server](https://github.com/zhoushengmin/spine-mcp-server) |
| `3dsmax-mcp` | uv 工具入口 | PyPI **3dsmax-mcp 1.6.8**，安装在 `%APPDATA%\uv\tools\3dsmax-mcp` | [cl0nazepamm/3dsmax-mcp](https://github.com/cl0nazepamm/3dsmax-mcp)（MIT） |
| `blender` | `uv run blender-mcp` | 源码 `D:\workspace\mcp\blender_mcp`，当前提交 `dbbf836` | [blender.org/lab/blender_mcp](https://projects.blender.org/lab/blender_mcp) |
| `substance3d-designer` | HTTP `127.0.0.1:9765/mcp` | 由已安装的 Designer **16.0.7**（`D:\Substance3DDesigner`）在进程内提供，没有单独的源码目录 | Adobe Substance 3D Designer 内置 MCP |

Unity 和 Substance 是软件自己开的 HTTP 服务。对应程序没开时，Cursor 里这两条会报连接失败，这是预期行为。

## 在另一台机器上接上

把 `cursor/mcp.json` 合并进 Cursor 的用户级 `mcp.json`，并把里面的绝对路径改成那台机器的实际位置。

依赖：

- Unity：用 MCP for Unity 打开工程，确认 HTTP 端口是 `10090`。
- Spine：`node` 20+，先在 `spine-mcp-server` 里执行构建，保证 `dist/index.js` 存在，并设置本机 `Spine.com` 路径。
- 3ds Max：`uv tool install 3dsmax-mcp`，3ds Max 里装好它的桥接插件。入口路径随 uv 安装位置变化，不必照搬本机的 `Scripts\3dsmax-mcp.exe`。
- Blender：克隆 `blender_mcp`，在 Blender 里启用 `addon/blender_mcp_addon`，再用 uv 从 `mcp` 目录启动 `blender-mcp`。
- Substance：打开 Designer 16，确认偏好里 MCP 监听 `127.0.0.1:9765`。

## 本机恢复

用这份 `cursor/mcp.json` 覆盖 `%USERPROFILE%\.cursor\mcp.json` 里同名的五个键即可。覆盖前先看一眼原文件，避免冲掉其它 MCP。

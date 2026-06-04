# WeFlow · 今天不加班 ChaseZ 特供版

WeFlow 是一款**完全本地**的微信聊天记录查看、分析与导出工具。本仓库为**公开发行版**：提供安装包下载、用户使用文档与 Cursor/Codex Skill，**不包含应用源码**。

| 项目 | 说明 |
|------|------|
| **产品** | WeFlow · 今天不加班 ChaseZ 特供版本 |
| **二次开发** | Z503424 |
| **客服微信** | `sihai332211`（以应用内首页为准） |

---

## 下载安装

**[→ 最新版本 Releases](https://github.com/NewbieCheng/WeFlow-Release/releases/latest)**

| 平台 | 安装包 |
|------|--------|
| Windows 10+ x64 | `.exe` |
| Windows arm64 | `.exe`（arm64） |
| macOS Apple Silicon | `.dmg` |
| Linux x64 | `.AppImage` 或 `.tar.gz` |

**系统要求**：微信 **4.0 及以上**；Windows 建议 Win10+；macOS 为 Apple Silicon（M 系列）。

macOS 若提示应用已损坏，可在终端执行：

```bash
xattr -dr com.apple.quarantine "/Applications/WeFlow.app"
```

---

## 文档

- **[用户使用指南](docs/用户使用指南.md)** — 安装、连接数据库、各功能分步教程
- **[定时导出说明](docs/SCHEDULED-EXPORT.md)** — 设置页定时导出与计划任务
- **[macOS 密钥排障](docs/MAC-KEY-FAQ.md)** — 自动获取密钥失败时的处理流程
- 配置示例：[config/weflow-export-schedule.config.example.json](config/weflow-export-schedule.config.example.json)

配图可放入 `docs/images/`（文件名与指南内一致即可显示）。

---

## Agent Skill（定时导出）

在 **Cursor** 或 **Codex** 中安装 Skill，可用自然语言配置定时导出、计划任务与 API 排障：

1. 将本仓库目录 [`skills/weflow-scheduled-export/`](skills/weflow-scheduled-export/) 复制到：
   - **Cursor**：`~/.cursor/skills/weflow-scheduled-export/` 或任意项目的 `.cursor/skills/weflow-scheduled-export/`
   - **Codex CLI**：`~/.codex/skills/weflow-scheduled-export/`
2. 对 AI 说：「按 weflow-scheduled-export skill 帮我配置 WeFlow 定时导出。」

详见 Skill 内说明与 [用户使用指南 · Skill 章节](docs/用户使用指南.md#skill-功能教程)。

---

## 更新

在 WeFlow **设置 → 版本更新** 中检查更新，或从上方 Releases 下载新版本覆盖安装。

---

## 说明

- 本仓库**不提供源代码**；仅分发构建产物与文档。
- 功能问题请联系特供版客服 **sihai332211**。
- 深度分析导出内容可搭配 [ChatLab](https://chatlab.fun/)。

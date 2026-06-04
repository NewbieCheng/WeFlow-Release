# 配置参考

## 配置文件路径

| 文件 | 说明 |
|------|------|
| `文档/WeFlow/weflow-export-schedule.config.json` | 用户本地配置（应用 UI 写入） |
| [config/weflow-export-schedule.config.example.json](../../config/weflow-export-schedule.config.example.json) | 仓库示例 |

Windows 完整路径示例：`%USERPROFILE%\Documents\WeFlow\weflow-export-schedule.config.json`

## 全局字段

| 字段 | 类型 | 默认 | 说明 |
|------|------|------|------|
| `apiBaseUrl` | string | `http://127.0.0.1:5031` | WeFlow API 根地址 |
| `apiToken` | string | — | Bearer Token；可用 `WEFLOW_API_TOKEN` |
| `defaultOutputDir` | string | `exports/scheduled` | 相对输出根 |
| `pageSize` | number | `500` | 1–10000 |
| `requestTimeoutMs` | number | `60000` | 毫秒 |
| `keepSystemMessages` | boolean | `false` | 全局默认 |
| `tasks` | array | `[]` | 任务列表 |

## 任务字段

| 字段 | 必填 | 说明 |
|------|------|------|
| `id` | 是 | 唯一 ID |
| `enabled` | 否 | 默认 true |
| `name` | 否 | 显示名 |
| `group` | 二选一 | 群名关键词 |
| `talker` | 二选一 | `xxx@chatroom` |
| `dateMode` | 是 | 见下表 |
| `date` | 条件 | `specific` 时 `YYYYMMDD` |
| `lastNDays` | 条件 | `lastNDays` 时整数 |
| `format` | 否 | `markdown` \| `json` \| `chatlab` |
| `exportMedia` | 否 | `{ image, voice, video, emoji }` |
| `outputDir` | 否 | 任务输出目录 |
| `keepSystemMessages` | 否 | 覆盖全局 |

### dateMode → API 参数

| dateMode | start / end |
|----------|-------------|
| `yesterday` | 昨天 YYYYMMDD（本地时区） |
| `today` | 今天 YYYYMMDD |
| `specific` | `task.date` 当天 |
| `lastNDays` | 从 N 天前到今天 |

映射到 `GET /api/v1/messages?talker=&start=&end=`（需 Bearer Token）。

## 应用内操作

| 操作 | 位置 |
|------|------|
| 新建/编辑任务 | 设置 → 定时导出 |
| 预览日期范围 | 任务列表 → 预览 |
| 立即导出 | 任务列表 → 立即运行 |
| 打开配置目录 | 定时导出页 → 打开配置目录 |

## 常见错误

| 消息 | 原因 | 处理 |
|------|------|------|
| 无法连接 WeFlow API | 服务未启 / 端口错 | Agent 连接开 API |
| 缺少 API Token | 未配置 | 设置 Token 或 env |
| 未找到目标群聊 | group 无匹配 | 用 talker |
| 匹配到多个群 | group 太模糊 | 用 talker |
| 群列表为空 | 会话未加载 | 先打开聊天页 |

## 安装本 Skill

| 工具 | 路径 |
|------|------|
| Cursor | `~/.cursor/skills/weflow-scheduled-export/` |
| Codex CLI | `~/.codex/skills/weflow-scheduled-export/` |

从 [WeFlow-Release/skills/weflow-scheduled-export](https://github.com/NewbieCheng/WeFlow-Release/tree/main/skills/weflow-scheduled-export) 复制整个目录。

# 定时聊天记录导出

通过 **WeFlow 本地 HTTP API** + **配置文件** + **系统计划任务**（或应用内手动执行），按天、按群自动拉取并归档聊天记录。**触发时刻 WeFlow 须保持运行且已连接数据库。**

## 与应用内「自动化导出」的区别

| 方式 | 入口 | 适用 |
|------|------|------|
| 导出页自动化 | WeFlow → 导出 → 自动化 | 应用长期开着，HTML 等完整导出模板，间隔调度 |
| **本方案** | 设置 → 定时导出 + 计划任务 / API | 每日固定时刻、Markdown/JSON/ChatLab、多群多任务 |

## 1. 启用 Agent 连接（HTTP API）

1. 打开 WeFlow，完成数据库连接。
2. **设置 → Agent 连接**：
   - 开启本地 API 服务
   - 记录 **Host**（默认 `127.0.0.1`）、**Port**（默认 `5031`）、**Token**
3. 建议开启开机自启动，确保计划任务触发时应用在运行。

常用接口：`GET /health`、`GET /api/v1/sessions`、`GET /api/v1/messages?talker=&start=&end=`（需 Bearer Token）。

## 2. 配置任务

### 方式 A：应用内 UI（推荐）

1. 打开 **设置 → 定时导出**
2. 点击 **新建任务**，从列表中选择实际群聊（若列表为空，请先打开 **聊天** 页加载会话）
3. 配置时间范围、格式、媒体选项后 **保存任务**
4. 使用 **预览** / **立即导出** 验证
5. 配置写入：`文档/WeFlow/weflow-export-schedule.config.json`（Windows 示例）

### 方式 B：手动编辑 JSON

复制本仓库 [config/weflow-export-schedule.config.example.json](../config/weflow-export-schedule.config.example.json) 到上述路径并编辑（勿将含真实 Token 的文件上传到公开位置）。

### 全局字段

| 字段 | 说明 |
|------|------|
| `apiBaseUrl` | API 地址，默认 `http://127.0.0.1:5031` |
| `apiToken` | 访问令牌（也可用环境变量 `WEFLOW_API_TOKEN`） |
| `defaultOutputDir` | 默认输出根目录 |
| `pageSize` | 分页大小 `1~10000`，默认 `500` |
| `requestTimeoutMs` | 请求超时（毫秒） |
| `keepSystemMessages` | 是否保留系统消息 |
| `tasks` | 任务数组 |

### 任务字段（`tasks[]`）

| 字段 | 必填 | 说明 |
|------|------|------|
| `id` | 是 | 任务唯一 ID |
| `enabled` | 否 | 默认 `true` |
| `name` | 否 | 备注名称 |
| `group` | 二选一 | 群名关键词 |
| `talker` | 二选一 | 群 ID，如 `xxx@chatroom` |
| `dateMode` | 是 | `yesterday` \| `today` \| `specific` \| `lastNDays` |
| `date` | 条件 | `specific` 时 `YYYYMMDD` |
| `lastNDays` | 条件 | `lastNDays` 时天数 |
| `format` | 否 | `markdown` \| `json` \| `chatlab` |
| `exportMedia` | 否 | `{ image, voice, video, emoji }` |
| `outputDir` | 否 | 本任务输出目录 |

## 3. 手动执行（应用内）

在 **设置 → 定时导出** 中对单条任务点击 **立即导出**，或对全部已启用任务使用界面提供的批量执行（若版本支持）。

也可在 Agent 连接开启后，用 HTTP 客户端调用消息导出相关 API（需自行组合分页与写文件逻辑）。

## 4. Windows 任务计划程序

配合「每天导出昨天」的 `dateMode: yesterday`，建议在凌晨触发，且 **WeFlow 已登录并运行**。

**方案 A（推荐）**：在 WeFlow **设置 → 定时导出** 中配置好任务后，使用 Windows「任务计划程序」在固定时间：

1. 确保 WeFlow 已设置 **开机自启动**（设置 → 版本更新）
2. 创建任务，触发器例如每天 01:00
3. 操作可选：运行已安装的 WeFlow（若需唤醒应用），或依赖用户已登录会话中 WeFlow 常驻

**方案 B（HTTP 触发）**：若你熟悉 API，可用 `curl` 在计划任务中调用（示例，请替换 Token 与 talker）：

```bat
curl -s -H "Authorization: Bearer YOUR_TOKEN" "http://127.0.0.1:5031/health"
```

完整导出需按会话拉取消息并写入文件，建议优先使用应用内 **立即导出** 与 UI 配置，减少脚本维护成本。

环境变量（可选）：在任务属性中设置 `WEFLOW_API_TOKEN`，避免 Token 明文写入配置文件。

## 5. Linux / macOS

- 保持 WeFlow 在计划任务触发时段运行
- 使用 cron 在固定时间检查 `/health` 或启动 WeFlow（路径因安装方式而异）
- 导出任务本身建议在应用内 **定时导出** 页配置并 **立即导出** 验证

## 6. 排障

| 现象 | 处理 |
|------|------|
| 无法连接 WeFlow API | 确认应用已开、Agent 连接已启用、端口正确 |
| 401 / Token 错误 | 检查 Token 或 `WEFLOW_API_TOKEN` |
| 未找到群聊 | 使用更精确的群名或 `talker` |
| 定时导出群列表为空 | 先打开 **聊天** 页加载会话 |
| 计划任务无输出 | 确认触发时刻 WeFlow 在运行、已连库、API 已开启；查看输出目录下 `export.log` |
| 导出为空 | 检查 `dateMode` 与当天是否有消息 |

## 7. 相关资源

- [用户使用指南](用户使用指南.md) — 含 Skill 安装与图文教程
- [config 示例](../config/weflow-export-schedule.config.example.json)
- Cursor/Codex Skill：[skills/weflow-scheduled-export](../skills/weflow-scheduled-export/)

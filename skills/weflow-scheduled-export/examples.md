# 配置示例

## 每日导出昨天群聊（Markdown + 图片）

在 **设置 → 定时导出** 中新建任务，或编辑 `weflow-export-schedule.config.json`：

```json
{
  "apiBaseUrl": "http://127.0.0.1:5031",
  "apiToken": "YOUR_TOKEN",
  "tasks": [
    {
      "id": "daily-project-group",
      "enabled": true,
      "group": "项目群",
      "dateMode": "yesterday",
      "format": "markdown",
      "exportMedia": { "image": true, "voice": false },
      "outputDir": "exports/scheduled/项目群"
    }
  ]
}
```

验证：在应用内点击 **预览** → **立即运行**。

计划：每天 01:00 触发（导出「昨天」），且 WeFlow 已开机自启并在运行。

## 指定某一天、指定群 ID

```json
{
  "id": "once-20260115",
  "enabled": true,
  "talker": "12345678@chatroom",
  "dateMode": "specific",
  "date": "20260115",
  "format": "chatlab"
}
```

在 **定时导出** 列表中对应该任务点击 **立即运行**。

## 最近 7 天 JSON（不导出媒体）

```json
{
  "id": "last7-json",
  "enabled": true,
  "group": "运营群",
  "dateMode": "lastNDays",
  "lastNDays": 7,
  "format": "json",
  "exportMedia": { "image": false, "voice": false, "video": false, "emoji": false }
}
```

## Windows 任务计划程序（保持 WeFlow 运行）

| 项 | 建议 |
|----|------|
| 触发器 | 每天 01:00 |
| 前置 | **设置 → 版本更新** 开启开机自启 |
| 导出执行 | 在 WeFlow **设置 → 定时导出** 已配置任务；触发时刻应用须已登录运行 |
| 可选检查 | 任务中运行 `curl -s -H "Authorization: Bearer TOKEN" http://127.0.0.1:5031/health` 确认 API 可用 |

若需无人值守批量导出，优先使用应用内 **立即运行**；复杂 HTTP 流水线需自行调用 Agent API（本仓库不提供 Node 脚本）。

环境变量（任务属性 → 可选）：`WEFLOW_API_TOKEN=...`

## Linux / macOS

- cron 在固定时间启动或唤醒 WeFlow（安装路径因平台而异）
- 导出在 **设置 → 定时导出** 配置，触发时在应用内执行或保持应用常驻

```cron
# 示例：每天 1:00 检查 API（需替换 TOKEN）
0 1 * * * curl -sf -H "Authorization: Bearer YOUR_TOKEN" http://127.0.0.1:5031/health >> /var/log/weflow-export.log 2>&1
```

## 与聊天整理 / SOP 对比

| 能力 | 入口 | 说明 |
|------|------|------|
| 聊天整理 / SOP | 首页 → 聊天整理 | 单次群聊、docx/Markdown、可选 AI |
| 定时导出 | 设置 → 定时导出 | 多任务、日期模式、计划任务 |

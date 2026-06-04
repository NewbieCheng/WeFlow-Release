---
name: weflow-scheduled-export
description: >-
  Configures WeFlow Agent Connect HTTP API and scheduled chat export via
  Settings UI and weflow-export-schedule.config.json. Use when the user asks
  for timed/auto export, cron, Task Scheduler, API token setup, exporting a
  group chat for a specific day, or weflow-export-schedule configuration.
disable-model-invocation: true
---

# WeFlow 定时聊天记录导出

帮助用户配置 **WeFlow Agent 连接** + **设置 → 定时导出** + **系统计划任务**，实现按天、按群自动导出。

权威文档：[docs/SCHEDULED-EXPORT.md](../../docs/SCHEDULED-EXPORT.md)  
字段与排障：[reference.md](reference.md)  
示例：[examples.md](examples.md)

## 前置条件

1. WeFlow 已安装（从 [WeFlow-Release](https://github.com/NewbieCheng/WeFlow-Release/releases/latest) 下载）、已连接微信数据库。
2. **设置 → Agent 连接**：开启本地 API，记录 Host/Port/Token。
3. 计划任务触发时 **WeFlow 必须正在运行**。

## 标准流程（推荐）

1. **Agent 连接**  
   开启 API，设置 Token；确认 `http://127.0.0.1:5031/health` 可访问（Bearer Token）。

2. **应用内配置任务**  
   **设置 → 定时导出** → **新建任务** → 选择群聊 → 设置 `dateMode`、格式、媒体 → **保存**。

3. **验证**  
   对任务点击 **预览** 或 **立即运行**；配置写入 `文档/WeFlow/weflow-export-schedule.config.json`（Windows）。

4. **系统定时**  
   - 开启 **设置 → 版本更新 → 开机自启动**  
   - Windows：任务计划程序每天触发，且触发时刻 WeFlow 已登录运行  
   - 详见 [examples.md](examples.md)

5. **手动编辑 JSON（可选）**  
   参考仓库 [config/weflow-export-schedule.config.example.json](../../config/weflow-export-schedule.config.example.json)，复制到上述 Documents 路径（勿提交含 Token 的文件）。

## 排障要点

- `/health` 失败 → API 未开或 WeFlow 未运行。  
- 401 → Token 错误。  
- 群列表为空 → 先打开 **聊天** 页加载会话。  
- 与 **聊天整理 / SOP** 区别：SOP 为单次整理导出；本方案为多任务 + 日期模式 + 计划任务。

## 不要做的事

- 不要假设 WeFlow 关闭时仍能导出。  
- 不要要求用户 clone 源码仓库或执行 `npm run`（本发行包无 Node 工程）。  
- 不要将含真实 Token 的配置提交到 Git。

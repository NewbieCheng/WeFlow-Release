# 发布到 GitHub 公开仓

本目录为 [NewbieCheng/WeFlow-Release](https://github.com/NewbieCheng/WeFlow-Release) 的**唯一来源**。安装包不提交 Git，由私有仓 CI 同步到公开仓 Releases。

## 首次创建公开仓并推送文档

### 方式 A：一键脚本（需已安装 GitHub CLI）

本机已安装便携版 `gh`（路径：`%LOCALAPPDATA%\Programs\gh-cli\bin`），并已加入用户 PATH。

**先登录**（新开终端执行一次）：

```powershell
gh auth login -h github.com -p https -w
```

**再发布**：

```powershell
powershell -ExecutionPolicy Bypass -File F:\Dev\Desktop-Projects\wechat\scripts\setup-gh-and-publish-release.ps1
```

脚本会自动：检查登录 → 创建 `NewbieCheng/WeFlow-Release`（若不存在）→ 推送 `release-public` 的 `main`。

### 方式 B：手动 git

1. 在 GitHub 创建**公开**仓库 `WeFlow-Release`（不要初始化 README）。
2. 在本目录执行：

```powershell
cd F:\Dev\Desktop-Projects\wechat\release-public
git push -u origin main
```

若本目录尚无 `.git`，见仓库根目录 `scripts/setup-gh-and-publish-release.ps1`。

## 更新文档 / Skill

编辑本目录文件后：

```powershell
git add -A
git commit -m "docs: update user guide or skills"
git push origin main
```

## 安装包 Releases

- 私有仓 Secret 配置说明：私有仓内 [docs/PUBLIC-RELEASE-SECRET.md](https://github.com/NewbieCheng/WeFlow/blob/main/docs/PUBLIC-RELEASE-SECRET.md)（仓库为私有时请在本地打开 `wechat/docs/PUBLIC-RELEASE-SECRET.md`）。
- Secret 名称：`PUBLIC_RELEASE_GH_TOKEN`；快捷脚本：`scripts/set-public-release-secret.ps1`。
- 私有仓打 tag `v*` 或手动触发 `Build and Release` 后，CI 会将构建产物镜像到本仓同名 Release。

本地已有 `release/` 产物时，也可手动上传：

```powershell
# 需安装 GitHub CLI 且已 gh auth login
gh release create v5.0.0 --repo NewbieCheng/WeFlow-Release --title v5.0.0 --notes "知聊 5.0 · 知销" F:\Dev\Desktop-Projects\wechat\release\ZhiLiao-5.0.0-Setup.exe
```

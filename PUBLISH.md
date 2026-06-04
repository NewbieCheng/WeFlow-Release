# 发布到 GitHub 公开仓

本目录为 [NewbieCheng/WeFlow-Release](https://github.com/NewbieCheng/WeFlow-Release) 的**唯一来源**。安装包不提交 Git，由私有仓 CI 同步到公开仓 Releases。

## 首次创建公开仓并推送文档

1. 在 GitHub 创建**公开**仓库 `WeFlow-Release`（不要初始化 README）。
2. 在本目录执行：

```powershell
cd F:\Dev\Desktop-Projects\wechat\release-public
git init -b main
git add -A
git commit -m "Initial public release repo: docs and Agent skills"
git remote add origin https://github.com/NewbieCheng/WeFlow-Release.git
git push -u origin main
```

若本目录已有 `.git`，仅需 `git push -u origin main`。

## 更新文档 / Skill

编辑本目录文件后：

```powershell
git add -A
git commit -m "docs: update user guide or skills"
git push origin main
```

## 安装包 Releases

- 在私有仓 `NewbieCheng/WeFlow` 配置 Actions Secret：`PUBLIC_RELEASE_GH_TOKEN`（对 `WeFlow-Release` 有 Contents 写权限的 PAT）。
- 私有仓打 tag `v*` 或手动触发 `Build and Release` workflow 后，CI 会将构建产物镜像到公开仓同名 Release。

本地已有 `release/` 产物时，也可手动上传：

```powershell
# 需安装 GitHub CLI 且已 gh auth login
gh release create v4.3.0 --repo NewbieCheng/WeFlow-Release --title v4.3.0 F:\Dev\Desktop-Projects\wechat\release\*.exe
```

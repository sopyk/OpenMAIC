# OpenMAIC 自动同步构建

这个仓库是 `THU-MAIC/OpenMAIC` 的 fork，用于自动同步上游正式 release 并构建 Docker 镜像。

## ✨ 功能

- **每日自动检查**：每天北京时间 8 点自动检查上游最新正式 release
- **智能同步**：仅当上游有新版本时才同步到本仓库并触发构建
- **多架构镜像**：自动构建 `amd64` + `arm64` 双架构镜像
- **GHCR 分发**：镜像推送到 GitHub Container Registry

## 🚀 使用方法

### 拉取镜像

```bash
# 最新版本
docker pull ghcr.io/sopyk/openmaic:latest

# 指定版本
docker pull ghcr.io/sopyk/openmaic:v1.2.3
```

### 运行

```bash
docker run -d \
  -p 3000:3000 \
  -v /path/to/.env:/app/.env \
  ghcr.io/sopyk/openmaic:latest
```

## 🔧 手动触发

如果不想等每日定时，可以手动触发同步：
1. 进入仓库的 **Actions** 页面
2. 选择 **Sync Upstream Release & Build Docker**
3. 点击 **Run workflow** → 选择 main 分支 → 确认

## 📋 工作流程

```
定时/手动触发 → 检查上游最新 release
    ↓
发现新版本 → 拉取对应 tag → 推送到本仓库
    ↓
触发构建 → 构建双架构镜像 → 推送到 GHCR
```

## 🔒 权限说明

本 workflow 使用 GitHub 自动生成的 `secrets.GITHUB_TOKEN`，拥有：
- 仓库写入权限（用于推送 tag）
- 包管理权限（用于推送镜像到 GHCR）

无需额外配置。

---

*原项目：[THU-MAIC/OpenMAIC](https://github.com/THU-MAIC/OpenMAIC)*

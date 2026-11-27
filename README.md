# S-V-R · SharkVideo Release

这个仓库**只放发布产物**，用来给客户端做自动更新。

**这里没有源码，也不会有。** 源码在私有仓库里，不对外公开。

## 里面有什么

| 文件 | 说明 |
|---|---|
| `SharkVideo-<版本>-win64.zip` | Windows 免安装包（PyInstaller onedir，解压即用） |
| `sharkvideo-android-<版本>.apk` | Android 安装包（Kotlin + ExoPlayer，minSdk 24） |
| `latest.json` | 自动更新的**兜底**数据源（main 分支根目录，API 被限流时用） |

> ### 资产名必须是纯 ASCII
>
> GitHub 的上传接口会把 `?name=` 里的中文截掉：实测 `鲨鱼剧场-Android-1.7.7.apk`
> 落库后变成 `-Android-1.7.7.apk`（中文整段消失）。客户端是按资产名匹配的，
> 名字歪了的表现就是"永远没有新版"。
>
> 所以 Android 包在这里统一叫 `sharkvideo-android-<版本>.apk`；
> 本地构建产物叫 `鲨鱼剧场-Android-<版本>.apk`，发布脚本会改名。

## 自动更新怎么工作

1. 客户端启动（或点「检查更新」）→ 匿名请求
   `api.github.com/repos/2436136932/S-V-R/releases/latest`
2. 拿到 tag（= 版本号）、资产大小、下载地址 → 与自己的打包版本比对
3. 有新版本 → 下载 → 校验 size + sha256 → 交给系统安装器（Android）/
   退出后覆盖替换再重启（Windows）
4. API 被限流（匿名 60 次/小时/IP）时，退回 `latest.json`

**客户端不带任何 token。** 这也是本仓库必须保持 **public** 的原因：
私有仓库匿名读 `api.github.com` / `raw.githubusercontent.com` 一律 404，
自动更新会整块失效（v1.7.0 那版就是这么废掉的）。

## 硬约束（改之前先看）

- **不要把这个仓库改成 private** —— 一改，所有人的自动更新立刻失效
- **tag 版本必须等于两端的安装包版本**：客户端拿 tag 当"远端版本"、拿打包版本当
  "本地版本"；两者不同号的表现是「每次启动都提示更新 → 装完还提示」
- **不要手传资产**：用发布脚本（`PC播放器/release.py`），它会写 `latest.json`、
  校验大小与哈希，并且重跑是幂等的

## 手动下载

到 [Releases](../../releases/latest) 页挑对应平台的包。

- **Windows**：解压后进入 `SharkVideo/`，双击 `SharkVideo.exe`
  （升级：退出旧版，把新包解压覆盖原目录；`progress.json` / `settings.json` /
  `downloads/` 都在同级，想稳妥先备份）
- **Android**：允许「安装未知来源应用」后安装即可，可覆盖升级
  （同一张签名证书，不需要卸载）

校验：每个 release 正文里都写了两个包的 SHA-256，也可以看源码仓库同一个 tag 下的
`SHA256SUMS.txt`。

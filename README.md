# S-V-R · SharkVideo Release

这个仓库**只放发布产物**，用来给客户端做自动更新。

## 内容

| 文件 | 说明 |
|---|---|
| `SharkVideo-<版本>-win64.zip` | Windows 免安装包 |
| `鲨鱼剧场-Android-<版本>.apk` | Android 安装包 |

客户端启动后会匿名读取本仓库的 Release，比对版本号；有新版就提示并下载。
所以**本仓库必须保持 public** —— 私有仓库别人读不到，自动更新就失效了。

## 关于源码

**这里没有源码，也不会有。** 源码在私有仓库里，不对外公开。

## 手动下载

到 [Releases](../../releases/latest) 页挑对应平台的包。

## 安装 / 升级

Windows 包是 PyInstaller **onedir** 结构，解压后是一个 `SharkVideo` 目录，
里面 `SharkVideo.exe` 和 `_internal/`。

* **首次安装**：整个目录解压到任意位置，双击 `SharkVideo.exe`
* **升级**：退出旧版，把新包解压覆盖原目录即可。
  观看进度（`progress.json`）、设置（`settings.json`）、下载的文件和封面缓存都在
  exe 同级目录 —— 想稳妥就先备份这四个再覆盖。
* 用客户端里的「检查更新」会自动完成上面这套（下载 → 退出后替换 → 重启）。

## 校验

每个 Release 的说明里会给出 zip 的 sha256。核对方法：

```powershell
Get-FileHash .\SharkVideo-*-win64.zip -Algorithm SHA256
```

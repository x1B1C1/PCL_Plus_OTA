# PCL_Plus OTA（备用更新服务器）

这个仓库只放 PCL_Plus 的更新清单与发布包副本，源码在 [x1B1C1/PCL_PLus](https://github.com/x1B1C1/PCL_PLus)。

- **主服务器**：源码仓 `x1B1C1/PCL_PLus`（清单在仓库根目录，发布包在同名 tag 的 Releases）
- **备用服务器**：本仓库（主服务器的所有清单通道都失败后才会切过来）

客户端按分支区分发布线：`main` 是正式版，`beta` 是测试版。清单里的 `self` 会展开成当前分支的 `releases/latest`，所以发布包必须传到对应分支的 tag。

## 分支

| 分支 | 发布线 | 当前版本 |
|---|---|---|
| `main` | 正式版（稳定，不含预发布） | 2.0R |
| `beta` | 测试版（预发布，普通用户收不到） | 2.5.0-beta |

## 文件

| 文件 | 说明 |
|---|---|
| `update.json` | v1 清单，`{"update":"self"}` 表示按当前仓库与分支解析最新 Release |
| `updatev2.json` | v2 清单，列出主/备服务器与权重（official 100 / backup 40） |

## 发布包

| tag | 发布线 | 文件 | 含 client id | 主页彩蛋 |
|---|---|---|---|---|
| `v2.5.0-beta` | beta | `PCL_Plus_v2.5.0-beta_x64.exe` | 是 | 否 |
| `v2.0-R` | main | `PCL_Plus_x64.exe` | 是 | 否 |
| `v2.0.0-beta` | beta | `PCL_Plus_v2.0.0-beta_x64.exe` | 是 | 否 |
| `v1.5.1-beta` | beta | `PCL_Plus_v1.5.1-beta_x64.exe` | 是 | 是 |
| `v1.5.0-beta` | beta | `PCL_Plus_v1.5.0-beta_x64.exe` | 是 | 否 |
| `v1.3.0-beta` | beta | `PCL_Plus_v1.3.0-beta_x64.exe` | 否 | 否 |
| `v1.2.0-beta` | beta | `PCL_Plus_v1.2-beta_x64.exe` | 否 | 否 |
| `v1.1.1-R` | main | `PCL_Plus_v1.1.1R_x64.exe` | 是 | 是 |
| `v1.1.0` | main | `PCL_Plus_v1.1.0_x64.exe` | 否 | 否 |
| `v1.0.0` | main | `PCL_Plus_v1.0.0_x64.exe` | 否 | 否 |

每个 Release 都附带源码压缩包；本仓库的 Release 另外附带该 exe 的 `.sha256`。

## 发布流程

1. 在源码仓打 tag 并创建 Release，上传 exe 与源码压缩包。
2. 在本仓库用同一个 tag 创建 Release，上传同一份 exe、它的 `.sha256` 与源码压缩包。
3. 确认 `releases/latest` 落在预期版本上（测试版要标成 prerelease，否则会抢走 latest）。

清单文件本身正常不需要改动；换发布仓库时才需要同步修改 `PCL.Core/App/ProjectLinks.cs` 里的 `ServiceOwner` / `ServiceRepo`。

## 许可

发布包沿用《PCL 分发有限许可》与《PCL 存储库合理使用指南》，详见源码仓的 `LICENSE`。

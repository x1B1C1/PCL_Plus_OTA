# PCL_Plus OTA（备用服务器）

本仓库是 PCL_Plus 的**备用更新服务器**，只放更新清单和发布包的镜像。

- **主服务器**：源码仓 [`x1B1C1/PCL_PLus`](https://github.com/x1B1C1/PCL_PLus)（清单在仓库根目录，发布包在它的 Releases）
- **备用服务器**：本仓库（主服务器的清单通道全部失败时，客户端才会切到这里）

## 两个分支

客户端按 `metadata.json` 的版本后缀选分支：后缀以 `beta` 开头读 `beta`，其余读 `main`。

| 分支 | 对应发布线 | 清单文件 |
|---|---|---|
| `main` | 正式版（`1.1.1R`、`1.1.0` …） | `update.json`、`updatev2.json` |
| `beta` | 测试版（`1.5.1-beta` …） | 同名两份 |

两份清单在主服务器与备用服务器上**逐字节相同**，改的时候要同时改两边。

- `update.json`：`{"update": "self"}`，表示「清单就在本仓库 Releases 里」。展开后是 `releases/latest`，`latest` 会跳过预发布版本。
- `updatev2.json`：带权重的多源清单，`official` = 主服务器 `x1B1C1/PCL_PLus`（权重 100），`backup` = 本仓库（权重 40）。高权重那一档问到了就定，低权重档只在前面全挂时才登场。

## 发布包

本仓库的 Releases 与源码仓的 Releases 保持同一批 tag，每个 tag 里放同样的 exe，另外多一份 `.sha256`。

| Tag | 版本 | 通道 | exe | 含 client id | 主页彩蛋 |
|---|---|---|---|---|---|
| `v1.5.1-beta` | `1.5.1-beta` | 测试 | `PCL_Plus_v1.5.1-beta_x64.exe` | ✅ | ✅ |
| `v1.5.0-beta` | `1.5.0-beta` | 测试 | `PCL_Plus_v1.5.0-beta_x64.exe` | ✅ | ❌ |
| `v1.3.0-beta` | `1.3.0-beta` | 测试 | `PCL_Plus_v1.3.0-beta_x64.exe` | ❌ | ❌ |
| `v1.2.0-beta` | `1.2.0-beta` | 测试 | `PCL_Plus_v1.2-beta_x64.exe` | ❌ | ❌ |
| `v1.1.1-R` | `1.1.1R` | 正式 | `PCL_Plus_x64.exe` | ✅ | ✅ |
| `v1.1.0` | `1.1.0` | 正式 | `PCL_Plus_v1.1.0_x64.exe` | ✅ | ❌ |
| `v1.0.0` | `1.0.0-pre` | 正式 | `PCL_Plus_v1.0.0_x64.exe` | ❌ | ❌ |

早期三个包（`v1.0.0`、`v1.2.0-beta`、`v1.3.0-beta`）按「历史版本原样保留」没有重出，因此不含 client id；从 `v1.1.0` 起每次构建都会嵌入。

「R」是正式标记：界面显示 `1.1.1R`，GitHub tag 与文件名仍是 SemVer 合法的 `v1.1.1-R`。

## 发布流程

1. 在源码仓出包（`tools/build-release.ps1 -All -Portable`），产物在 `release/`。
2. 把 exe、`.sha256`、源码 zip 传到**两个仓库**的同一个 tag 下。
3. 清单有变动时，两个仓库的两个分支都要更新。

完整说明见源码仓的 [`docs/UPDATE.md`](https://github.com/x1B1C1/PCL_PLus/blob/main/docs/UPDATE.md)。

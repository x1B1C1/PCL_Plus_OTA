# PCL_Plus_OTA

`PCL_Plus` 的**备用更新服务器**。主服务器是源码仓 [`x1B1C1/PCL_PLus`](https://github.com/x1B1C1/PCL_PLus)，只有它所有通道都失败时，客户端才会切到这里。

## 文件

| 文件 | 说明 |
|---|---|
| `update.json` | v1 清单，内容固定 `{"update": "self"}`，表示更新源就是本仓库的 Releases |
| `updatev2.json` | v2 清单，声明主/备两个源与权重（official 100 / backup 40） |

两份文件在 `main` 与 `beta` 分支上内容相同；发布包要同时传到两个仓库的同一个 tag。

## 分支

| 分支 | 发布线 | 后缀 |
|---|---|---|
| `main` | 正式版 | `R`（如 `2.1R`） |
| `beta` | 测试版 | `-beta` 或当期命名的 `R` |

## 发布包

| tag | 线路 | 文件 | 含 client id | 主页彩蛋 |
|---|---|---|---|---|
| `v1.0` | 正式 | `PCL_Plus_x64.exe` | ❌ | ❌ |
| `v1.1.0` | 正式 | `PCL_Plus_x64.exe` | ✅ | ❌ |
| `v1.1.1-R` | 正式 | `PCL_Plus_x64.exe` | ✅ | ✅ |
| `v1.2-beta` | 测试 | `PCL_Plus_v1.2-beta_x64.exe` | ❌ | ❌ |
| `v1.3.0-beta` | 测试 | `PCL_Plus_v1.3.0-beta_x64.exe` | ❌ | ❌ |
| `v1.5.0-beta` | 测试 | `PCL_Plus_v1.5.0-beta_x64.exe` | ✅ | ❌ |
| `v1.5.1-beta` | 测试 | `PCL_Plus_v1.5.1-beta_x64.exe` | ✅ | ✅ |
| `v2.0.0-beta` | 测试 | `PCL_Plus_v2.0.0-beta_x64.exe` | ✅ | ❌ |
| `v2.0-R` | 正式 | `PCL_Plus_x64.exe` | ✅ | ❌ |
| `v2.5.0-beta` | 测试 | `PCL_Plus_v2.5.0-beta_x64.exe` | ✅ | ❌ |
| `v2.1-R` | 正式 | `PCL_Plus_x64.exe` | ✅ | ❌ |
| `v2.5.1-R` | 测试 | `PCL_Plus_v2.5.1-R_x64.exe` | ✅ | ❌ |

每个 Release 另附源码 zip（`PCL_Plus-<版本>-src.zip`）与本仓库特有的 `.sha256` 校验文件。

## 发布流程

1. 在源码仓 `x1B1C1/PCL_PLus` 的对应分支打 tag 并推送；
2. 两个仓库各建一个同名 Release，资产保持一致（本仓库额外放 `.sha256`）；
3. 测试线的 Release 必须勾选「预发布」，否则会抢走 `releases/latest`，把正式用户推到测试线；
4. 客户端读取 `raw.githubusercontent.com/<owner>/<repo>/<branch>/updatev2.json` → 按权重选源 → 从对应 Release 下载。

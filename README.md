# PCL_Plus_OTA（备用更新服务器）

这个仓库是 **PCL_Plus 的备用更新服务器**，不是源码仓。

- 源码仓（**主服务器**）：[x1B1C1/PCL_PLus](https://github.com/x1B1C1/PCL_PLus)
- 本仓库（**备用服务器**）：`x1B1C1/PCL_Plus_OTA`

## 客户端怎么选

1. 先问主服务器 `x1B1C1/PCL_PLus`：`raw.githubusercontent.com` → `cdn.jsdelivr.net` → `api.github.com`。
2. 主服务器三条通道**全部失败**才会切到本仓库重试同一套通道。
3. 清单里的 `updatev2.json` 还带权重：主服务器 `weight: 100`、本仓库 `weight: 40`，发布包也只在高权重源全挂时才用低权重源。

也就是说：**主服务器正常时，本仓库一次都不会被访问。**

## 两个分支

| 分支 | 用途 | 对应版本后缀 |
| --- | --- | --- |
| `main` | 正式版 | 无后缀 / `hotfix` |
| `beta` | 测试版 | `beta` |

分支由客户端自己决定（读 `metadata.json` 的 `version.suffix`），不看用户的更新通道设置：正式版永远读 `main`，beta 构建永远读 `beta`。

## 文件

| 文件 | 说明 |
| --- | --- |
| `update.json` | 更新指针对（`{"update": "self"}` 表示「就是本仓库的 Releases」） |
| `updatev2.json` | 多源清单：列出主服务器（weight 100）与本仓库（weight 40） |

两份文件在主服务器和本仓库里**逐字节相同**，改的时候两边都要改。

## 发布包

本仓库的 Releases 与主服务器**同一个 tag、同一份资产**：

- `v1.0.0`：`PCL_Plus_x64.exe`、`PCL_Plus-1.0.0-src.zip`
- `v1.2.0-beta`（预发布）：`PCL_Plus_v1.2-beta_x64.exe`、`PCL_Plus-1.2.0-beta-src.zip`

另有对应的 `.sha256` 校验文件。发版时要同时上传到两个仓库的同一个 tag，否则备用服务器切换过去也拿不到包。

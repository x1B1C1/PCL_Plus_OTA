# PCL_Plus OTA

这是 **PCL_Plus 的更新清单仓库**，客户端只从这里读 `update.json` / `updatev2.json`，
发布包（exe）放在本仓库的 Releases 里。

| 分支 | 用途 | 谁能读到 |
| --- | --- | --- |
| `main` | **正式版**（版本后缀不带 `beta`） | 所有正式版客户端 |
| `beta` | **测试版**（版本后缀是 `beta`） | 所有 beta 版客户端 |

分支由 exe 里的版本后缀决定（`Plain Craft Launcher 2/metadata.json` 的 `version.suffix`），
不看用户在「设置 → 更新」里选的通道；想换线只能换包。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `update.json` | 锚点清单。内容只有 `{"update": "self"}`，`self` 展开成 `x1B1C1/PCL_Plus_OTA/releases/latest` |
| `updatev2.json` | 多源清单（schema 2）。`sources[].repo` 指向真正放发布包的仓库，`tier` 表示信任级别 |

## 发布新版本

1. 在源码仓 `x1B1C1/PCL_PLus` 打 tag 并发布 Release（exe 与源码包）。
2. 在本仓库对应的分支的 Releases 里上传同一份 exe 与 `.sha256`。
3. 只改发布包位置（不换仓库）时：改 `updatev2.json` 里的 `repo`，客户端无需更新。

详细流程见源码仓的 `docs/UPDATE.md` 与 `docs/BUILD.md`。

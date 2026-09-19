# AIwriter MiniMax — 更新通道（release-only）

本仓库**只存放发布产物**，源码在私有仓库 `TsSnakeTang/AIwriter_MiniMax`（不随本仓库公开）。

## 内容

| 文件 | 说明 |
|---|---|
| `latest.json` | 桌面端自动更新清单。客户端端点：`https://github.com/TsSnakeTang/aiwriter-updates/releases/latest/download/latest.json` |
| `AIwriter.MiniMax_<version>_x64-setup.exe` | NSIS 安装包（已用 minisign/Ed25519 签名） |
| `AIwriter.MiniMax_<version>_x64-setup.exe.sig` | 对应签名文件；公钥内置于客户端 `tauri.conf.json`，客户端只在验签通过时才安装 |

## 流程

1. 私有仓库 CI 在 `v*` tag 上完成构建 + 签名 + 私有 Release 发布；
2. 同一条流水线把「安装包 + `.sig` + `latest.json`」经 **deploy key** 推送到本仓库 `latest` 分支；
3. 本仓库的 `publish-release` workflow 依 `release-meta.json` 建/更新对应 tag 的 Release 并标记为 latest；
4. 私有仓库 CI 再以**匿名**身份回读本仓库端点复核（客户端视角）。

`latest` 分支为滚动分支（每次 force-push），仅作传输与排查用途；对外分发一律走 Releases。

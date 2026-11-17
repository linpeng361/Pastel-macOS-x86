<div align="center">
  <br>
  <img src="https://github.com/user-attachments/assets/f302f5d2-730c-44d2-a2c1-63cdd75804f2" width="136" alt="Pastel 图标">
  <h1>Pastel (x86_64)</h1>
  <h3>下载 App 的历史版本 · Intel Mac 适配版</h3>
  <p>在 Mac 上搜索 iOS、iPadOS 或 visionOS App，浏览版本记录，并从 Apple 获取所选版本。</p>
  <br>
  <p>
    <a href="#从源码构建"><strong>从源码构建</strong></a>
    &nbsp;&nbsp;·&nbsp;&nbsp;
    <a href="#适配说明">查看适配说明</a>
  </p>
  <p><sub>需要 macOS 26 或更高版本，本分支为 Intel Mac (x86_64) 构建。</sub></p>
  <br>
</div>

<p align="center">
  <img width="980" alt="Pastel 主界面" src="https://github.com/user-attachments/assets/2807691d-7f1d-4060-89b5-ae90839d37d5">
</p>

<br>

## 适配说明

本项目是 [EEliberto/Pastel-macOS](https://github.com/EEliberto/Pastel-macOS) 的 Intel Mac (x86_64) 适配分支。原版仅支持 Apple Silicon (arm64)，本分支将所有原生组件重新编译为 x86_64 架构，使其可在 Intel Mac 上运行。

### 改动内容

| 组件 | 原版 (arm64) | 本分支 (x86_64) |
|---|---|---|
| Node.js 运行时 | arm64 Node.js 24.x | **x86_64 Node.js v24.10.0** |
| Store Helper (Go) | `GOARCH=arm64` 编译 | **`GOARCH=amd64` 重新编译** |
| Xcode ARCHS | `arm64` | **`x86_64`** |
| SAPSigner.m | `arm64` 编译 | **`x86_64` 编译** |

### 具体修改的文件

- `node/bin/node.gz` — 替换为 x86_64 Node.js v24.10.0
- `node/bin/store-helper.gz` — 使用 Go `GOARCH=amd64` 重新编译
- `Pastel.xcodeproj/project.pbxproj` — Debug / Release 的 `ARCHS` 从 `arm64` 改为 `x86_64`
- `Scripts/BuildStoreHelper.sh` — `GOARCH=arm64` → `GOARCH=amd64`
- `Scripts/BundleNodeRuntime.sh` — SAPSigner 默认架构从 `arm64` → `x86_64`
- `Scripts/BuildAdHocDMG.sh` — 注释更新（Apple silicon → hardened runtime）
- `Pastel/PastelApp.swift` — 修复 Xcode 26.6 下的 Swift 6 类型推断错误，移除当前 SDK 不支持的 `.visibilityPriority(.high)` API
- `StoreHelper/README.md` — arm64 → x86_64

### 未改动的部分

- SwiftUI 界面代码、业务逻辑、网络请求、下载引擎均未修改
- NodeProject (JavaScript) 业务代码未修改
- StoreHelper (Go) 源码未修改，仅重新编译
- Sparkle 自动更新框架为 universal 二进制，无需改动

<br>

## 搜索、选择、下载

选择 App Store 地区，搜索 App，再从版本记录中选取需要的版本。Pastel 会根据 Apple 账户所在地区匹配商店，让搜索与下载始终保持一致。

<p align="center">
  <img width="820" alt="Pastel App 版本详情" src="https://github.com/user-attachments/assets/1f260ee8-d97c-445e-990d-e5f3bb6aa903">
</p>

<br>

## 查看更多历史版本

Pastel 汇集 Timbrd、Agzy、Bilin 和 Apple 提供的版本记录。版本来源用于查找可用版本；IPA 始终由 Apple 提供。已知 App ID 与版本 ID 时，也可以直接输入并下载。

如果账户尚未获取某个免费 App，Pastel 会在你确认后先将它加入账户，再继续下载。付费 App 需要由该账户事先购买。

<p align="center">
  <img width="560" alt="Pastel 版本来源选择" src="https://github.com/user-attachments/assets/f382ef02-e764-4763-b3eb-02f48ba0a0fe">
</p>

<br>

## 管理已下载的 App

下载资料库按 App 整理 IPA，并显示版本、地区、Apple 账户和更新状态。你可以在"访达"中显示文件，也可以使用共享菜单或隔空投送将文件发送至 iPhone 或 iPad。

<p align="center">
  <img width="900" alt="Pastel 下载资料库" src="https://github.com/user-attachments/assets/3b50e39c-84bc-4c65-b2f4-c55bab9bdabb">
</p>

<br>

## 专为 Mac 打造

Pastel 使用 SwiftUI 构建，采用 Apple 的最新 Liquid Glass 设计语言，并提供简体中文、繁体中文、日语、韩语和泰语界面。网络访问遵循 macOS 代理设置，并支持 HTTP、HTTPS、SOCKS5、`ALL_PROXY` 和 `NO_PROXY`。

<p align="center">
  <img width="820" alt="Pastel 多语言界面" src="https://github.com/user-attachments/assets/058f2acc-55aa-422b-a48e-fc7a0820be9a">
</p>

<br>

## 开始使用

1. 从源码构建（见下方）。
2. 将 Pastel 拖移到"应用程序"文件夹，然后打开 App。
3. 前往"设置"→"Apple 账户"，添加账户并按提示完成双重认证。
4. 选择 App 与版本，然后开始下载。

<p align="center">
  <img width="640" alt="Pastel Apple 账户设置" src="https://github.com/user-attachments/assets/b40db4c9-3e27-4bd4-9f1e-82ed0ecc29a0">
</p>

Apple 账户密码与会话数据使用 AES-GCM 加密保存，主密钥由 macOS Keychain 保护。Pastel 使用 Apple App Store 认证与下载服务，并复用已加密保存的认证状态，减少重复登录与等待。

<br>

## 从源码构建

```bash
cd NodeProject
npm install
cd ..
open Pastel.xcodeproj
```

使用 Xcode 构建并运行 `Pastel` 方案，无需 Apple 开发者账户。需要生成 DMG 时，运行 `sh Scripts/BuildAdHocDMG.sh`；修改下载辅助组件后，先按 [StoreHelper 构建说明](StoreHelper/README.md) 更新随附的二进制文件。

> **注意**：构建 Store Helper 需要 Go 工具链（`GOARCH=amd64`）。构建 Node 运行时需替换 `node/bin/node.gz` 为 x86_64 版 Node.js。

<details>
  <summary>开源项目与致谢</summary>
  <br>
  Apple 登录与下载流程参考 <a href="https://github.com/majd/ipatool">majd/ipatool</a>、<a href="https://github.com/beer-psi/ipatool.ts">beer-psi/ipatool.ts</a>、<a href="https://github.com/SideStore/SideStore">SideStore</a> 与 <a href="https://github.com/Lakr233/Asspp">Lakr233/Asspp</a>。多语言翻译由 Claude 协助完成。
</details>

<br>

<div align="center">
  <p><a href="https://github.com/EEliberto/Pastel-macOS/issues">报告问题（原版）</a>&nbsp;&nbsp;·&nbsp;&nbsp;<a href="LICENSE">Apache License 2.0</a></p>
  <sub>Pastel 与 Apple Inc. 无隶属关系。App Store、Mac、iPhone、iPad 与隔空投送是 Apple Inc. 的商标。</sub>
</div>

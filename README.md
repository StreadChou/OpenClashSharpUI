<div align="center">

<img src="https://raw.githubusercontent.com/vernesong/OpenClash/dev/img/logo.png" alt="OpenClash" width="160" />

# OpenClash SharpUI

基于 OpenClash 的 UI 精简与交互改造项目

[![License](https://img.shields.io/github/license/StreadChou/OpenClashSharpUI?style=flat-square&color=brightgreen)](LICENSE)
[![Release](https://img.shields.io/github/v/release/StreadChou/OpenClashSharpUI?style=flat-square)](https://github.com/StreadChou/OpenClashSharpUI/releases)
[![Upstream](https://img.shields.io/badge/Upstream-OpenClash-blue?style=flat-square)](https://github.com/vernesong/OpenClash)

</div>

## 项目定位

**OpenClash SharpUI 是 [vernesong/OpenClash](https://github.com/vernesong/OpenClash) 的非官方 fork。** 上游是运行于 OpenWrt 的 LuCI 插件，负责管理 Mihomo（Clash Meta）内核。本项目计划对其 UI 进行大规模精简和重构，重点改善信息组织、操作流程与视觉体验，同时保持主要功能、配置格式和运行逻辑，并持续合并上游更新。

本项目的终极目标，是希望 OpenClash 作者更加重视 UI 和日常使用体验。我们希望通过实际改造展示更简洁的界面与交互方案，并为上游提供可参考的设计和实现。

**如果后续 OpenClash 完成 UI 改进，维护者确认可以回归上游，本项目将彻底关闭：停止开发、维护、版本发布和问题受理，并引导用户回到原始项目。** 届时会提供回归上游的说明。本项目不以长期维持独立分支为目标。

这里描述的是项目方向与维护约定，不代表所有 UI 改造已经完成。SharpUI 的问题请提交到[本仓库 Issues](https://github.com/StreadChou/OpenClashSharpUI/issues)；确认是上游问题后，再提供可在原版复现的信息。

## 目录

- [项目定位](#项目定位)
- [改造原则](#改造原则)
- [上游基线](#上游基线)
- [版本规则](#版本规则)
- [下载与安装](#下载与安装)
- [使用手册与依赖](#使用手册与依赖)
- [源码构建](#源码构建)
- [开发与上游同步](#开发与上游同步)
- [致谢](#致谢)
- [支持上游作者](#支持上游作者)
- [许可证](#许可证)

## 改造原则

- 优先精简概览、配置与订阅、日志与诊断等日常操作，减少默认展示的复杂选项。
- 复用原有操作接口与业务能力，保持配置、校验、保存和应用行为；未展示的高级配置应完整保留。
- 新 UI 尽量使用独立目录、资源命名空间和接口适配层，保留原界面或高级入口，逐步迁移。
- 控制与上游的差异，避免无关格式化、文件重排以及重复维护后端逻辑，便于持续合并。
- 保留上游署名、许可证与致谢，欢迎将有价值的改进反馈给上游。

## 上游基线

| 项目 | 当前记录 |
| --- | --- |
| 上游仓库 | [vernesong/OpenClash](https://github.com/vernesong/OpenClash) |
| 上游版本 / 标签 | `0.48.002` / `v0.48.002` |
| 上游提交 | [`e553e82e01c697aefbe1555261a503f425e00ccd`](https://github.com/vernesong/OpenClash/commit/e553e82e01c697aefbe1555261a503f425e00ccd) |
| 本项目仓库 | [StreadChou/OpenClashSharpUI](https://github.com/StreadChou/OpenClashSharpUI) |

这是当前代码所记录的同步基线，不表示它始终是上游最新版本，也不表示本项目已经发布了对应安装包。每次合并新的上游版本时更新此表。

## 版本规则

本项目的对外版本采用 **上游版本 + SharpUI 修订号**，明确展示与原始项目的对应关系。

| 场景 | 本项目显示版本 | GitHub 发布标签 |
| --- | --- | --- |
| 同步上游 `0.48.002`，形成对应的 SharpUI 基线发布 | `0.48.002` | `sharpui-v0.48.002` |
| 在此基础上首次独立修复 BUG 或发布 UI 改进 | `0.48.002-001` | `sharpui-v0.48.002-001` |
| 在同一上游基线上再次独立发布 | `0.48.002-002` | `sharpui-v0.48.002-002` |
| 合并上游 `0.48.003`，保留 SharpUI 改造并形成新的基线发布 | `0.48.003` | `sharpui-v0.48.003` |
| 新基线上的首次独立修订 | `0.48.003-001` | `sharpui-v0.48.003-001` |

- 无后缀表示该上游版本对应的 SharpUI 同步发布，包含已有的 UI 改造，不代表与原版源码完全一致。
- 后缀从 `001` 开始，按实际发布递增，至少保留三位；超过 `999` 后继续使用 `1000`。普通开发提交不自动增加版本。
- 切换到新的上游版本后，修订号重新开始；若继续维护旧基线，则继续使用其已有修订序列。已发布版本和标签不覆盖、不复用。
- 每个 Release 应注明上游版本、完整提交 SHA、本项目修订内容、验证结果及已知限制。

**GitHub 可以使用这种标签和 Release 名称。** 我们使用 `sharpui-v` 标签前缀区分 fork 发布，避免与已继承的上游 `v0.48.002` 等标签混淆；界面和 Release 名称中的版本仍为 `0.48.002-001`。标签与发布的关系参见 [GitHub 官方说明](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)。

这是本项目的版本约定，**不是严格 SemVer**：`002` 和 `001` 带有前导零，且本项目将 `-001` 定义为正式修订，不能套用 SemVer 的预发布排序规则。版本比较应分别按十进制解析上游各数字段及修订号，缺省修订号视为 `0`：

```text
0.48.002 < 0.48.002-001 < 0.48.002-002 < 0.48.003
```

稳定或预发布状态按实际发布性质设置，不由 `-001` 后缀决定。发布时显式指定是否设为 Latest，不依赖 GitHub 的自动语义版本排序；旧基线的维护版本不替代较新基线的稳定版本成为 Latest。有关严格 SemVer 的限制，参见 [SemVer 规范](https://semver.org/lang/zh-CN/)。

### 安装包版本与发布状态

GitHub 标签、显示版本和安装包元数据是不同层次。IPK/APK 的版本格式、升级排序、下载文件名，以及 OpenClash 内置更新器都需要单独适配与验证；不能仅将 `PKG_VERSION` 改为 `0.48.002-001` 就认为发布链路已经兼容。

**当前仓库仍保留上游的打包与插件更新逻辑，尚未完成 SharpUI 修订号和更新来源的完整适配。** 上述规则是后续发布约定，实际安装包以本项目 Releases 为准。原插件更新来源仍指向上游，直接使用原插件更新功能可能覆盖 SharpUI 改动。

后续打包将分别维护上游版本与 fork 修订，按 SDK 支持的包版本规则映射，并在 Release 中注明对应关系。每次 UI 发布也需要更新静态资源缓存标识，确保用户能够加载修订后的界面。

## 下载与安装

请以[本项目 Releases](https://github.com/StreadChou/OpenClashSharpUI/releases) 中实际发布的资产及说明为准。若尚无 SharpUI 安装包，请使用本仓库源码构建；上游安装包不会包含本项目的 UI 改造。

发布安装包后，在 OpenWrt 的 **系统 → 软件包** 中选择与系统包管理器匹配的文件进行安装：

| 文件类型 | 对应系统 |
| --- | --- |
| `*.ipk` | 使用 opkg 的系统 |
| `*.apk` | 使用 apk 的系统 |

升级、从原版迁入和回归上游前，请备份配置并阅读对应版本的迁移说明。不要把上游下载脚本或原插件更新通道当作 SharpUI 的发布来源。

## 使用手册与依赖

代理配置与功能说明可参考[上游使用手册](https://github.com/vernesong/OpenClash/blob/dev/.github/skills/openclash-user-guide/SKILL.md)。该文档随上游变化，UI 位置和选项可能与当前 SharpUI 不同。

运行依赖继续沿用所同步上游版本的要求，包括 LuCI、luci-compat、dnsmasq-full、bash、curl、Ruby/YAML 及相应网络组件；iptables / nftables、opkg / apk 的具体依赖按目标 OpenWrt 环境选择，以[包 Makefile](luci-app-openclash/Makefile)和上游对应版本说明为准。

## 源码构建

获取本 fork 的源码：

```sh
git clone --branch master https://github.com/StreadChou/OpenClashSharpUI.git
cd OpenClashSharpUI
```

准备与目标 OpenWrt 版本和平台匹配的 SDK，将本仓库的 `luci-app-openclash` 目录复制到 SDK 的 `package/luci-app-openclash`。准备所需依赖及 `po2lmo` 等构建工具后，在 **SDK 根目录**执行：

```sh
make defconfig
make package/luci-app-openclash/compile V=s
```

构建产物位于 SDK 的 `bin/packages/` 下，具体路径和文件名取决于 SDK。现有构建步骤也可参考[构建工作流](.github/workflows/compile_new_ipk.yml)，但其中发布分支、版本读取与更新来源仍需按本项目规则适配，不能直接据此认定 SharpUI 发布已经就绪。

## 开发与上游同步

Agent 和贡献者请先阅读仓库根目录的 [AGENTS.md](AGENTS.md)，其中包含 UI 改造边界、行为保持原则、验证要求、上游合并流程以及版本和发布约定。

同步时以明确的上游稳定标签与提交 SHA 为目标，在独立分支合并，逐项核对接口、配置和运行行为；保留 SharpUI 的界面与适配层，更新上游基线及同步说明。上游标签使用独立命名空间，本项目发布使用 `sharpui-v*`，不移动已有标签。

## 致谢

首先感谢 [vernesong](https://github.com/vernesong) 和 [OpenClash](https://github.com/vernesong/OpenClash) 的所有贡献者。本项目的代理管理与运行能力来自上游，SharpUI 的工作聚焦于 UI 与使用体验。

以下保留上游项目的致谢：

| 类别 | 项目 | 作者 / 组织 | 说明 |
|------|------|-------------|------|
| 内核 | [Mihomo](https://github.com/MetaCubeX/mihomo) | [MetaCubeX](https://github.com/MetaCubeX) | 代理内核（Clash Meta） |
| 代码基础 | [Luci For Clash](https://github.com/frainzy1477/luci-app-clash) | [frainzy1477](https://github.com/frainzy1477) | 上游 OpenClash 的代码基础 |
| 控制面板 | [metacubexd](https://github.com/MetaCubeX/metacubexd) | [MetaCubeX](https://github.com/MetaCubeX) | 默认内置面板 |
| 控制面板 | [zashboard](https://github.com/Zephyruso/zashboard) | [Zephyruso](https://github.com/Zephyruso) | 内置面板 |
| 控制面板 | [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta) | [MetaCubeX](https://github.com/MetaCubeX) | 内置面板，基于 [yacd](https://github.com/haishanh/yacd) |
| 控制面板 | [Razord-meta](https://github.com/MetaCubeX/Razord-meta) | [MetaCubeX](https://github.com/MetaCubeX) | 内置面板（Dashboard） |
| 订阅转换 | [subconverter](https://github.com/tindy2013/subconverter) | [tindy2013](https://github.com/tindy2013) | 在线订阅转换 |
| GEO / IP 数据 | [v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) | [Loyalsoldier](https://github.com/Loyalsoldier) | GeoIP / GeoSite 数据 |
| GEO / IP 数据 | [meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat) | [MetaCubeX](https://github.com/MetaCubeX) | GeoSite 规则数据 |
| GEO / IP 数据 | [mmdb_china_ip_list](https://github.com/alecthw/mmdb_china_ip_list) | [alecthw](https://github.com/alecthw) | 中国 IP 列表 MMDB |
| GEO / IP 数据 | [china-operator-ip](https://github.com/gaoyifan/china-operator-ip) | [gaoyifan](https://github.com/gaoyifan) | 运营商 IP 列表 |
| 规则模板 | [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) | [ACL4SSR](https://github.com/ACL4SSR) | 订阅转换规则模板 |
| 规则模板 | [Custom_OpenClash_Rules](https://github.com/Aethersailor/Custom_OpenClash_Rules) | [Aethersailor](https://github.com/Aethersailor) | 订阅转换规则模板 |
| 流媒体检测 | [RegionRestrictionCheck](https://github.com/lmc999/RegionRestrictionCheck) | [lmc999](https://github.com/lmc999) | 流媒体解锁检测 |
| IP 检查 | [ip.skk.moe](https://ip.skk.moe/) | [SukkaW](https://ip.skk.moe/) | 公网 IP 检测 |
| 编辑器 | [CodeMirror](https://codemirror.net/) | [Marijn Haverbeke](https://github.com/marijnh) | 内置编辑器 |

## 支持上游作者

以下为上游 OpenClash 作者提供的支持方式，收款方为上游作者。

* PayPal

<p align="left">
    <a href='https://ko-fi.com/H2H41G5LS' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
</p>

* USDT-BSC

<p align="left">
    <img width="300" src="https://github.com/vernesong/OpenClash/raw/master/img/USDT-Wallet.png">
</p>

## 许可证

本项目沿用上游的 [MIT License](LICENSE)，保留原作者版权声明。

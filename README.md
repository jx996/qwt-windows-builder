# qwt-windows-builder

自动化编译 [QWT（Qt Widgets for Technical Applications）](https://sourceforge.net/projects/qwt/files/qwt/) 的 GitHub Actions 工程。

在 Windows 平台（GitHub 官方 `windows-latest` runner = Windows Server 2025 + **Visual Studio 2026**）上，
使用 Qt + MSVC x64 编译 QWT 动态链接库（**Release + Debug**），打包为 **7z** 并自动发布到 GitHub Release。

## 使用方法

1. 打开仓库 **Actions** → 左侧选择 **Build QWT (Windows / Qt / MSVC x64)**
2. 点击 **Run workflow**，按需填写参数（全部有默认值，可直接运行）：

| 参数 | 说明 | 默认值 |
| --- | --- | --- |
| `qwt_version` | QWT 版本号；填 `latest` 自动识别 SourceForge 最新版本 | `6.3.0` |
| `qt_version` | Qt 版本号 | `6.8.3` |
| `qt_arch` | Qt 编译套件（aqt 架构名） | `win64_msvc2022_64` |
| `build_examples` | 是否同时编译 examples / playground / tests | `false` |

3. 运行完成后，在仓库 **Releases** 页面下载编译产物：
   `qwt-<QWT版本>-qt<Qt版本>-vs2026-x64-release-debug.7z`

推送代码到 `main` 分支也会自动触发一次默认参数构建（作为冒烟测试）。

## 编译产物结构

```
qwt-6.3.0-qt6.8.3-vs2026-x64-release-debug.7z
└── qwt-6.3.0-qt6.8.3-vs2026-x64-release-debug/
    ├── COPYING             # Qwt License 1.0 官方全文（随产物分发）
    ├── include/            # 全部 QWT 头文件
    ├── lib/                # 导入库（qwt.lib / qwtd.lib）
    ├── bin/                # 运行时 DLL + PDB（qwt.dll / qwtd.dll）
    └── plugins/designer/   # Qt Designer 插件
```

> Qt 6.8.x 官方提供的 MSVC 套件为 `win64_msvc2022_64`（ABI 兼容 VS2026 工具链，直接可用）。

## 环境自检与自动安装

工作流在编译前会逐项检测，缺失时自动安装：

- **MSVC x64 工具链**（vswhere 检测，缺失时经 Chocolatey 安装 VS2026 C++ workload）
- **7-Zip**（缺失时经 Chocolatey 安装）
- **Qt**（`jurplel/install-qt-action` 带 GitHub Actions 缓存，未命中时经 aqtinstall 自动安装）

## 命名规范

统一采用「全小写 + 连字符分隔 + 版本号保留小数点」的语义化命名：

```
qwt-<QWT_VERSION>-qt<QT_VERSION>-vs2026-x64-release-debug.7z
```

| 组成部分 | 说明 | 示例 |
| --- | --- | --- |
| `qwt` | 库名（小写） | `qwt` |
| `6.3.0` | QWT 版本 | `6.3.0` |
| `qt6.8.3` | 编译所用的 Qt 版本 | `qt6.8.3` |
| `vs2026` | 工具链（Visual Studio 2026 / MSVC） | `vs2026` |
| `x64` | 目标架构 | `x64` |
| `release-debug` | 包内含 Release 与 Debug 两种配置 | `release-debug` |

据此，同一个版本会得到：

- Release tag / 名称：`qwt-6.3.0-qt6.8.3-vs2026-x64`
- 资产文件：`qwt-6.3.0-qt6.8.3-vs2026-x64-release-debug.7z`
- 压缩包内根目录：`qwt-6.3.0-qt6.8.3-vs2026-x64-release-debug/`

同一 tag 重复构建会覆盖上传同名资产。

## 许可

本仓库自身内容（CI 工作流、文档等）采用 **MIT License**，见 [LICENSE](LICENSE)。

编译产物属于 QWT 的二进制分发，遵循 **The Qwt License, Version 1.0**
（GNU LGPL 附例外条款），官方全文：<https://qwt.sourceforge.io/qwtlicense.html>。
工作流在打包时会把 QWT 官方 `COPYING` 一并放入压缩包根目录，随产物分发。
本仓库不包含、不分发 QWT 源码——源码在每次构建时从 SourceForge 官方地址实时下载。

各依赖的许可与合规要点详见 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。

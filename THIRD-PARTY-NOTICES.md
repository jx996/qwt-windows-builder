# 第三方许可声明 / Third-Party Notices

本仓库自身内容（CI 工作流、README 等构建脚本与文档）采用 [MIT License](LICENSE)。

本仓库**不包含也不分发** QWT 源码：QWT 源码在每次构建时从 SourceForge 官方地址
（`https://sourceforge.net/projects/qwt/files/qwt/`）实时下载。

## QWT

编译产物（`qwt.dll` / `qwtd.dll`、导入库、头文件、Designer 插件等）属于 QWT 的二进制分发，
遵循 QWT 项目自身的许可协议。

| 项目 | 说明 |
| --- | --- |
| 名称 | Qwt — Qt Widgets for Technical Applications |
| 版权 | Copyright (C) 1997 Josef Wilgen；Copyright (C) 2002 Uwe Rathmann |
| 许可 | **The Qwt License, Version 1.0**（GNU LGPL 基础上附例外条款） |
| 许可全文 | <https://qwt.sourceforge.io/qwtlicense.html> |
| 上游源码 | <https://sourceforge.net/projects/qwt/files/qwt/> |

### 关于 Qwt License 1.0 的要点

Qwt License 1.0 以 GNU Lesser General Public License（LGPL）为基础，并附加以下例外：

1. 继承自 Qwt 控件的子类**不构成衍生作品**；
2. 将应用程序/控件以**静态方式**链接到 Qwt 库**不构成衍生作品**，无需提供应用源码、
   也无需强制使用共享版 Qwt 库；
3. 其他条款详见许可全文。

由于例外条款放宽了部分 LGPL 义务，使用本仓库产物构建商业闭源程序通常是可行的；
如需严格合规判断，请以官方许可全文为准，或咨询法务。

### 产物内的许可材料

每次构建时，工作流会把 QWT 源码包中的官方 `COPYING`（Qwt License 1.0 全文）
一并放入压缩包根目录，随产物一同分发，以满足许可协议对分发许可文本的要求：

```
qwt-<QWT版本>-qt<Qt版本>-vs2026-x64-release-debug/
├── COPYING               # Qwt License 1.0 官方全文（随产物分发）
├── include/
├── lib/
├── bin/
└── plugins/designer/
```

## 其他构建期依赖

| 依赖 | 许可 | 说明 |
| --- | --- | --- |
| Qt 6.x | LGPLv3 / GPLv3 / 商业许可 | 由 aqtinstall 在构建环境安装，未打包进产物（运行时需自行安装 Qt 或遵守 Qt 许可分发 Qt 运行库） |
| Visual Studio 2026 / MSVC | Microsoft 许可 | 仅作为构建工具链使用 |
| GitHub Actions 相关 Action | 各自许可 | `actions/checkout`、`jurplel/install-qt-action`、`ilammy/msvc-dev-cmd`、`actions/upload-artifact` |

# 桌面编辑器依赖

- PySide6-Essentials 6.10.2 / Shiboken6 6.10.2：Qt for Python。安装包元数据列出 LGPL-3.0-only OR GPL-2.0-only OR GPL-3.0-only。项目与源码：https://code.qt.io/pyside/pyside-setup.git/ 。许可证说明：https://doc.qt.io/qtforpython-6/licenses.html 。
- Qt 动态库：随 PySide6-Essentials 分发，保留为独立动态库。
- PyInstaller：用于构建独立程序，许可及 bootloader 例外见 https://pyinstaller.org/en/stable/license.html 。

本包不含游戏 EXE、游戏引擎、原版 compile.dll 或游戏全局背景、角色等美术素材。空白模板中的 introduction、nand_gate、not_gate、foundry 与 sandbox 关卡文件按用户要求复制自本机 Turing Complete 2.1.334 原版 campaign，版权属于游戏原作者；原始文件校验记录见 `src/tclevel/data/templates/native_bootstrap/manifest.json` 和 `src/tclevel/data/templates/native_tools/manifest.json`。生成工程时的少量配置调整见工具模板文档。Save Monger 的格式参考和独立适配说明见下文。
# Native circuit format

`src/tclevel/codec.py` independently adapts the native format described by
Stuffe/save_monger, commit `2ef0e5b3b76915e1b0a23a88fcbd885371cdb68e`
(v2.1.326), including common.nim and version readers/writer. Upstream declares
the main source CC0: https://github.com/Stuffe/save_monger/tree/2ef0e5b3b76915e1b0a23a88fcbd885371cdb68e
The raw Snappy codec is an independent Python implementation; SuperSnappy is
not bundled or linked. No game executables, engine libraries or compile.dll are distributed. Bundled campaign data and its level image resources remain the property of the original game authors.

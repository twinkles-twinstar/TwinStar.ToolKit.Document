# 安装

- [平台支持](#平台支持)

- [模块组成](#模块组成)

- [必要安装](#必要安装)

- [外部程序](#外部程序)

- [Windows 资源管理器拓展](#Windows-资源管理器拓展)

- [多语言支持](#多语言支持)

- [对宿主终端的要求](#对宿主终端的要求)

## 平台支持

* 操作系统：`Windows 7+` 、`Linux ?` 、`Macintosh 13.1~` 、`Android 9~` 、`iPhone 16.2~` 。

* 处理器架构：`x86 32|64` 、`arm 32|64` 。

> 对于 `Windows` 、`Linux` 、`Macintosh` ，只提供 `x86_64` 分发；对于 `Android` 、`iPhone` ，只提供 `arm_64` 分发；如有面向其他平台的需要，需自行克隆并编译本项目。
> 
> @ `Android` `iPhone` \
> 由于目前仅提供基于终端的交互程序，因此需要 **ROOT | Jailbreak** 环境。

## 模块组成

* `Core`
	
	核心库，负责内部功能的实现。

* `Shell`
	
	外壳程序，提供基于终端的命令行界面。

* `Script`
	
	脚本，控制工具的运行流程。

* `Windows Explorer Extension`
	
	Windows 资源管理器拓展，将工具集成至 Windows Explorer 右键菜单中。

## 必要安装

`Core` 、`Shell` 、`Script` 是运行所需的 **必要模块** ，按照以下步骤进行安装。

1. 创建 **主目录**
	
	在存储空间中创建一个空目录，它用于存放工具运行所需的所有文件，称为主目录。
	
	如果你想卸载本工具，只需删除主目录，工具不会有任何数据残留。
	
	> 主目录的位置可以随意，但要确保用户有对该目录及其内容的 **读写执行权限** 。

2. 安装 `Core`
	
	查看本项目的 [Core 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Core) ，下载适用于你设备的分发。
	
	之后，将该文件移动到主目录内，并重命名为 `core` 。
	
	> @ `Android` \
	> 由于 **Android 7** 引入的 **链接器命名空间** 机制，程序无法加载 `+ <system-library>` 与 `+ <private-library>` 之外的动态库，对于单纯的命令行程序而言，后者是不存在的；因此，须将 `- core` 重命名为 `- TwinStar.ToolKit.core` ，并移动到 `+ <system-library>` 中。\
	> `+ <system-library>` 在 32 位系统中为  `+ /system/lib` ，在 64 位系统中为 `+ /system/lib64` 。

3. 安装 `Shell`
	
	查看本项目的 [Shell 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Shell) ，下载适用于你设备的分发。
	
	之后，将该文件移动到主目录内，并重命名为 `shell` 。
	
	> 可执行程序的运行需要文件具有可执行权限，**Windows** 上一般不需额外操作，其他系统的用户可能要在终端中执行 `> chmod +x shell` 为 `- shell` 添加可执行权限。
	> 
	> @ `Windows` \
	> 请在终端中执行 `> mklink shell.exe shell` 以创建指向 `- shell` 的软链接 `- shell.exe` ；也可以直接重命名 `- shell` 为 `- shell.exe` 。
	> 
	> @ `Android` \
	> 在采用了 FUSE 方案的系统中，`+ /storage/emulated/<id>` 实际上是 `+ /data/media/<id>` 的映射，但 FUSE 的限制使用户无法通过 `+ /storage/emulated/<id>` 进行文件的权限修改与执行，必须通过真实路径 `+ /data/media/<id>` 进行访问。

4. 安装 `Script`
	
	查看本项目的 [Script 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Script) ，下载适用于你设备的分发。
	
	之后，将该文件移动到主目录内，创建 `script` 目录，并将压缩包的所有内容解压至其中。

5. 安装 **启动脚本**
	
	查看本项目的 [Miscellaneous 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Miscellaneous) ，下载适用于你设备的分发。
	
	之后，将该文件移动到主目录内，并重命名为 `launch` ，但保留原有扩展名。

6. 检查 **主目录结构**
	
	如果操作无误，主目录内的组织结构将如下所示：
	
	```
	+ <home>
	  - core
	  - shell
	  + script
	      - main.js
	      - ...
	  - launch.[sh|cmd]
	```

7. 安装 **C++ 共享库**
	
	> **此步骤只需要 `Android` 用户执行。**
	
	出于安全性与减小体积的考虑，程序使用的 C++ 运行时 为 c++_shared 。因此，用户还需要将对应处理器架构的 `libc++_shared.so` 文件复制到系统库目录中。
	
	> 该文件可从 Android NDK 工具链中提取，也可在 [Miscellaneous 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/Miscellaneous) 中找到。

8. 签名 **程序文件**
	
	> **此步骤只需要 `iPhone` 用户执行。**
	
	在 iPhone 上运行可执行文件需要通过签名验证，而分发的程序文件（ **core | shell** ）未经过签名，用户需要自行对其进行签名，才能在 iPhone 中正常运行。
	
	> 可以在 **Macintosh** 上使用 **codesign** 工具进行签名。

9. 测试 **启动脚本**
	
	最后一步是测试启动脚本能否成功运行。
	
	Windows 用户，请双击 `- launch.cmd` 以启动该脚本。
	
	其他系统的用户，请在终端中执行 `> sh launch.sh` 。
	
	如果操作无误，你可以在终端中看到工具的运行界面。
	
	> 请不要直接启动 shell 程序，因为它需要用户传入一些必需参数，而启动脚本会自动添加启动参数。
	> 
	> @ `Windows` \
	> 若提示 **缺失动态库（DLL）** ，请安装 [Visual C++ 运行时组件](https://visualstudio.microsoft.com/zh-hans/downloads/#microsoft-visual-c-redistributable-for-visual-studio-2022) 。

## 外部程序

工具的某些功能需要调用外部程序，用户需自行下载并安装这些程序，并放置于主目录下的 `external` 目录内。

* [ffmpeg](https://ffmpeg.org/download.html)
	
	用于 **WEM 音频解码** 。
	
	下载完成后，将文件 `ffmpeg[.exe]` 重命名为 `ffmpeg` 并放置于 `+ <home>/external/ffmpeg` 目录下。

* [ww2ogg](https://github.com/hcs64/ww2ogg/releases/tag/0.24)
	
	用于 **WEM 音频解码** 。
	
	下载完成后，将文件 `ww2ogg[.exe]` 重命名为 `ww2ogg` 并放置于 `+ <home>/external/ww2ogg` 目录下。
	
	再将文件 `packed_codebooks_aoTuV_603.bin` 放置于该目录下。
	
	> 该项目的作者只分发了适用于 Windows 的可执行程序，其他系统的用户须自行下载源码构建。

## Windows 资源管理器拓展

对于 Windows 系统，还可向资源管理器的右键菜单植入功能选项，只需几步鼠标点击即可完成大部分文件处理操作，这需要用户额外安装一个拓展程序，以下的安装步骤适用于 `Windows 10+` 。

1. 查看本项目的 [WindowsExplorerExtension 分发](https://github.com/twinkles-twinstar/TwinStar.ToolKit.Document/releases/tag/WindowsExplorerExtension) ，下载适用于你设备的分发。

2. 右键查看应用包文件 `.msix` 的属性，切换到 ⌈ 数字签名 ⌋ 页，选择列表中第一项，再点击 ⌈ 详细信息 ⌋ ，在弹出的窗口中依次选择 ⌈ 查看证书 ⌋ - ⌈ 安装证书 ⌋ - ⌈ 本地计算机 ⌋ - ⌈ 将所有证书都放入下列存储 ⌋ - ⌈ 受信任人 ⌋ ，完成证书的安装。

3. 双击以安装应用包文件 `.msix` 。

4. 现在，你可以在任意文件或目录的右键菜单内看到 `⌈ TwinStar ToolKit ⌋` 选项，它为大多数功能提供了快捷入口。如果没有看到该选项，请尝试重启资源管理器 `explorer.exe` ，或重启计算机。

5. 安装成功后，进入系统环境变量设置，添加环境变量：变量名为 `TwinStar.ToolKit.WindowsExplorerExtension.launch_file` ，变量值为主目录内启动脚本的完整路径（不要使用引号包围），例如 `C:\TwinStar.ToolKit\launch.cmd` 。

6. 现在，你可以通过拓展程序来快速启动本工具；卸载拓展程序后，这些选项将不再出现。

> 拓展程序在新、旧式右键菜单上的表现有所区别：\
> 在新式右键菜单（即 Windows 11 式）中，所有功能选项将合并为一个二级菜单；\
> 在旧式右键菜单（即 Windows 10 式）中，各个功能选项都单独显示为一项一级菜单，这将导致右键菜单的面积大大增加；\
> 因此，Windows 10 用户请衡量利弊以决定是否安装。
> 
> *如此设计是因为新式右键菜单不允许单个右键拓展提供二级菜单，需为拓展程序绑定多个右键拓展以达到二级菜单效果，而旧式右键菜单中单个右键拓展只能提供至多 16 项选项。*

## 多语言支持

工具提供多语言支持，目前支持中文与英文。

默认情况下，工具将使用中文进行交互，如果需要切换为其他语言，应修改配置文件，步骤如下：

1. 进入工具的主目录，找到 `- <home>/script/Entry/Entry.json` 文件，使用文本编辑器打开。

2. 文本中的第二行为 `"language": "Chinese"` ，表示目前所使用的交互语言为中文，若需切换为英文，请将 `Chinese` 改为 `English` 。

> 多语言文本通过 `- <home>/script/Language/<Language-ID>.json` 定义，如果需要修正文本错误，或增加对其他语言的支持，请修改该文件；方便的话，也请提交 PR 为本项目做贡献。

## 对宿主终端的要求

`Shell` 提供了基于终端的命令行界面，但需要宿主终端支持以下特性：

1. UTF-8 输入/输出：必需，若不支持，将导致程序无法正常进行输入输出。

2. 虚拟终端控制序列：可选，若不支持，将导致程序无法对不同类型的文本修饰以不同的颜色。

3. 完备的字体：可选，若不支持，一些字符（如汉字、emoji ）将无法正常显示。

有些操作系统的默认终端不提供（或默认关闭）这些支持，用户可以安装第三方终端并在其中运行本程序，可以参照以下列表：

* Windows 7 ~ 8
	
	自带 cmd 不提供支持，推荐使用 [cmder](https://cmder.app/) 。

* Windows 10
	
	自带 cmd 默认关闭支持，推荐使用 [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) 。

* Windows 11
	
	自带 cmd 默认关闭支持，自带 Windows Terminal 提供支持，请将其设置为系统的默认终端。

* Macintosh 12.0.1+
	
	自带终端提供支持。

* Linux - Ubuntu 20.04+
	
	自带终端提供支持。

* Android
	
	推荐使用 [Termux](https://termux.dev/en/) 。
	
	> 当然，你也可以通过 ADB 来使用桌面系统的终端。

> @ `Windows` \
> 双击以运行启动脚本时，将在系统默认终端中运行程序，若想更改为在指定第三方终端中运行，请修改 `launch.cmd` ，但切换终端将导致一次 cmd 窗口闪烁，除非你使用 Windows 11 并将 Windows Terminal 设为系统默认终端。
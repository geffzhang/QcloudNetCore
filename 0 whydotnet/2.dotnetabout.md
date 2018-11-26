# .NET Core入门

可在 Windows、Linux 和 macOS 上安装 .NET Core。 你可在最喜欢的文本编辑器中编写代码并生成跨平台的库和应用程序。

## 创建应用程序

首先，在计算机上下载并安装 [.NET Core SDK](https://www.microsoft.com/net/download/)。

然后，打开某一终端，如 PowerShell、命令提示符或 Bash。 键入以下 `dotnet` 命令以创建并运行 C# 应用程序。

```console
dotnet new console --output sample1
dotnet run --project sample1
```

您应看到以下输出：

```console
Hello World!
```

祝贺你！ 现已创建了一个简单的 .NET Core 应用程序。 此外，还可以使用 Visual Studio Code、Visual Studio 2017或 Visual Studio for Mac（仅限 macOS）来创建 .NET Core 应用程序。

## 工具箱

![工具箱架构](resource/toolarch.png)

这些工具的分层非常简单。 在底部，我们将 共享 SDK 组件作为基础。共享 SDK 组件是一组负责编译代码、发布代码、打包 NuGet 包等操作的目标和关联任务。 其他所有高级工具（如 Visual Studio 或 Visual Studio Code）依靠 共享 SDK 组件来生成项目、还原依赖项以及完成其他操作。 现在所有工具集使用共享 SDK 组件及其目标，包括 CLI。

### CLI 命令

.NET Core 命令行接口 (CLI) 工具是用于开发 .NET 应用程序的新型跨平台工具链。 CLI 是更高级别的工具（如集成开发环境 (IDE)、编辑器和生成协调程序）可以驻留的基础。 CLI 核心命令如下：

* `new`
* `restore`
* `run` 
* `build`
* `publish`
* `test`
* `pack` 

这些命令的作用（新建项目、生成项目、发布项目、打包项目等等）。 可以使用 `dotnet <command> --help` 查看相应命令的帮助屏幕，也可以参阅此网站上的文档来熟悉所有更改。 
CLI 采用可使你为项目指定其他工具的扩展性模型。

## 命令结构

CLI 命令结构包含[驱动程序（“dotnet”）](#driver)、[命令（或“谓词”）](#command-verb)，或可能的命令[参数](#arguments)和[选项](#options)。 在大部分 CLI 操作中可看到此模式，例如创建新控制台应用并从命令行运行该应用，因为从名为 *my_app* 的目录中执行时，显示以下命令：

```console
dotnet new console
dotnet build --output /build_output
dotnet /build_output/my_app.dll
```

---

### <a name="driver"></a>驱动程序

驱动程序名为 [dotnet](dotnet.md)，并具有两项职责，即运行[依赖于框架的应用](../deploying/index.md)或执行命令。 唯一一次在不使用命令的情况下使用 `dotnet` 是在将其用于启动应用程序时。

若要运行依赖于框架的应用，请在驱动程序后指定应用，例如，`dotnet /path/to/my_app.dll`。 从应用的 DLL 驻留的文件夹执行命令时，只需执行 `dotnet my_app.dll` 即可。

为驱动程序提供命令时，`dotnet.exe` 启动 CLI 命令执行过程。 首先，驱动程序确定要使用的 SDK 版本。 如果在命令选项中未指定版本，则驱动程序使用可用的最新版本。 若要指定某个版本，而不是最新安装的版本，请使用 `--fx-version <VERSION>` 选项（请参阅 [dotnet 命令](dotnet.md) 引用）。 确定 SDK 版本后，驱动程序执行命令。

### <a name="command-verb"></a>命令（“谓词”）

命令（或“谓词”）仅仅是执行操作的命令。 例如，`dotnet build` 生成代码。 `dotnet publish` 发布代码。 使用 `dotnet {verb}` 约定将命令作为控制台应用程序实现。

### <a name="arguments"></a>自变量

在命令行上传递的参数是被调用的命令的参数。 例如，执行 `dotnet publish my_app.csproj` 时，`my_app.csproj` 参数指示要发布的项目，并被传递到 `publish` 命令。

### <a name="options"></a>选项

在命令行上传递的选项是被调用的命令的选项。 例如，执行 `dotnet publish --output /build_output` 时，`--output` 选项及其值被传递到 `publish` 命令。

## <a name="see-also"></a>请参阅

* [dotnet/CLI GitHub 存储库](https://github.com/dotnet/cli/)  
* [.NET Core 安装指南](https://aka.ms/dotnetcoregs)  
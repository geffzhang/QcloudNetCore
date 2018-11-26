# <a name="net-standard"></a>.NET Standard

[.NET Standard](https://github.com/dotnet/standard) 是一套正式的 .NET API 规范，有望在所有 .NET 实现中推出。 推出 .NET Standard 的背后动机是要提高 .NET 生态系统中的一致性。 [ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) 持续为 .NET 实现行为建立统一性，但适用于 .NET 库实现的 .NET 基类库 (BCL) 没有类似的规范。

.NET Standard 是一种正式规范，它定义了一组版本化 API。这些 API 必须可用于符合相应 Standard 版本要求的 .NET 实现。 .NET Standard 面向库开发者。 定目标到 .NET Standard 版本的库可用于任意 .NET Framework、.NET Core 或支持 Standard 版本的 Xamarin 实现。

.NET Standard 的最新版本是 2.0。 .NET Core 2.1 SDK 及已安装 .NET Core 工作负载的 Visual Studio 2017 版本 15.8。


.NET Standard 可实现以下重要情境：

- 为要实现的所有 .NET 实现定义一组统一的、与工作负荷无关的 BCL API。
- 使开发人员能够通过同一组 API 生成可在各种 .NET 实现中使用的可移植库。
- 减少甚至消除由于 .NET API 方面的原因而对共享源代码进行的条件性编译（仅适用于 OS API）。

各种 .NET 实现以特定版本的 .NET Standard 为目标。 每个 .NET 实现版本都会公布它所支持的最高 .NET Standard 版本，这种声明意味着它也支持以前的版本。 例如，.NET Framework 4.6 实现 .NET Standard 1.3。也就是说，它会公开在 .NET Standard 版本 1.0 到 1.3 中定义的所有 API。 同样，.NET Framework 4.6.1 实现 .NET Standard 1.4，而 .NET Core 1.0 则实现 .NET Standard 1.6。

## <a name="net-implementation-support"></a>.NET 实现支持

下表列出了支持每个 .NET Standard 版本的最低平台版本。
| .NET Standard              | [1.0] | [1.1]  | [1.2] | [1.3] | [1.4] | [1.5]      | [1.6]      | [2.0]      |
|----------------------------|-------|--------|-------|-------|-------|------------|------------|------------|
| .NET Core                  | 1.0   | 1.0    | 1.0   | 1.0   | 1.0   | 1.0        | 1.0        | 2.0        |
| .NET Framework <sup>1</sup>| 4.5   | 4.5    | 4.5.1 | 4.6   | 4.6.1 | 4.6.1      | 4.6.1      | 4.6.1      |
| Mono                       | 4.6   | 4.6    | 4.6   | 4.6   | 4.6   | 4.6        | 4.6        | 5.4        |
| Xamarin.iOS                | 10.0  | 10.0   | 10.0  | 10.0  | 10.0  | 10.0       | 10.0       | 10.14      |
| Xamarin.Mac                | 3.0   | 3.0    | 3.0   | 3.0   | 3.0   | 3.0        | 3.0        | 3.8        |
| Xamarin.Android            | 7.0   | 7.0    | 7.0   | 7.0   | 7.0   | 7.0        | 7.0        | 8.0        |
| 通用 Windows 平台 | 10.0  | 10.0   | 10.0  | 10.0  | 10.0  | 10.0.16299 | 10.0.16299 | 10.0.16299 |
| Windows                    | 8.0   | 8.0    | 8.1   |       |       |            |            |            |
| Windows Phone              | 8.1   | 8.1    | 8.1   |       |       |            |            |            |
| Windows Phone Silverlight  | 8.0   |        |       |       |       |            |            |            |
| Unity                      | 2018.1| 2018.1 | 2018.1| 2018.1| 2018.1| 2018.1     | 2018.1     | 2018.1     |

<sup>1 针对 .NET framework 列出的版本适用于 .NET Core SDK 2.0 和更高版本的工具。旧版本对 .NET Standard 1.5 及更高版本使用了不同映射。如果无法升级到 Visual Studio 2017，可[下载适用于 Visual Studio 2015 的 .NET Core 工具](https://github.com/dotnet/core/blob/master/release-notes/download-archive.md)。</sup>

- 列表示 .NET Standard 版本。 每个标题单元格都是一个文档链接，其中介绍了相应版本的 .NET Standard 中新增了哪些 API。
- 行表示不同的 .NET 实现。
- 各单元格中的版本号指示要定向到此 .NET Standard 版本所需的最低实现版本。
- 有关交互式表的信息，请参阅 [.NET Standard 版本](http://immo.landwerth.net/netstandard-versions/#)。

[1.0]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.0.md
[1.1]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.1.md
[1.2]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.2.md
[1.3]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.3.md
[1.4]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.4.md
[1.5]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.5.md
[1.6]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.6.md
[2.0]: https://github.com/dotnet/standard/blob/master/docs/versions/netstandard2.0.md

若要查找可以定位的 .NET Standard 最高版本，请按照以下步骤操作：

1. 查找要运行的 .NET 实现所在的行。
2. 在这一行中从右向左查找可以定位的 .NET Standard 版本所在的列。
3. 列标题指明了目标平台支持的 .NET Standard 版本（也支持所有更低的 .NET Standard 版本）。
4. 对要定位的每个平台重复执行此过程。 如果有多个目标平台，应选择它们都支持的最高版本。 例如，如果要在 .NET Framework 4.5 和 .NET Core 1.0 上运行，可以使用的最高 .NET Standard 版本是 .NET Standard 1.1。

### <a name="which-net-standard-version-to-target"></a>要定位哪个 .NET Standard 版本

选择 .NET Standard 版本时，应权衡以下因素：

- 版本越高，可使用的 API 就越多。
- 版本越低，可实现它的平台就越多。

一般来说，建议尽可能定位*最低*版本 .NET Standard。 因此，在找到可以定位的最高版本 .NET Standard 后，请按照以下步骤操作：

1. 定位前一更低版本的 .NET Standard，然后生成项目。
2. 如果成功生成项目，请重复执行第 1 步。 否则，重新定位到后一较高版本，这就是应该使用的版本。

但是，定位更低版本的 .NET Standard 会引入许多支持依赖项。 如果项目定位 .NET Standard 1.x，我们建议还定位 .NET Standard 2.0。 这简化了在 .NET Standard 2.0 兼容框架上运行的库的用户的依赖项关系图，并减少了下载所需的包数。

### <a name="net-standard-versioning-rules"></a>.NET Standard 版本控制规则

版本控制规则主要有两个：

- 累加性：.NET Standard 版本在逻辑上形成同心圆。也就是说，较高的版本包含较低版本的所有 API。 版本之间没有重大更改。
- 不可变：一旦发布，.NET Standard 版本就会冻结起来。 新 API 首先会在特定的 .NET 实现（如 .NET Core）中可用。 如果 .NET Standard 评审委员会认为新 API 应可用于所有 .NET 实现，则会将它们添加到新的 .NET Standard 版本中。

## <a name="specification"></a>规范

.NET Standard 规范是一组标准化的 API。 此规范由 .NET 实现者（具体而言，由 Microsoft（包括 .NET Framework、NET Core 和 Mono）和 Unity）进行维护。 在通过 [GitHub](https://github.com/dotnet/standard) 建立新 .NET Standard 版本的过程中，采用公众反馈流程。

### <a name="official-artifacts"></a>正式项目

正式规范是一组用于定义标准中包含的 API 的 .cs 文件。 [dotnet/standard 存储库](https://github.com/dotnet/standard)中的 [Ref 目录](https://github.com/dotnet/standard/tree/master/netstandard/ref)定义了 .NET Standard API。

[NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library) 元包（[源代码](https://github.com/dotnet/standard/blob/master/netstandard/pkg/NETStandard.Library.dependencies.props)）描述用于部分定义一个或多个 .NET Standard 版本的库集。

给定的组件（如 `System.Runtime`）描述：

- .NET Standard 的一部分（即其范围）。
- .NET Standard 在此范围内的多个版本。

标准库提供派生项目以方便读取，并实现某些开发人员方案（例如，使用编译器）。

- [Markdown 中的 API 列表](https://github.com/dotnet/standard/tree/master/docs/versions)
- 引用程序集，以 NuGet 包的形式分发，由 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) 元包引用。

### <a name="package-representation"></a>包表示形式

.NET Standard 引用程序集的主要分发载体是NuGet 包。 实现会以适用于每个 .NET 实现的各种方式提供。

NuGet 包面向一个或多个[框架](frameworks.md)。 .NET Standard 包定位“.NET Standard”框架。 可以使用 `netstandard` 精简 TFM（例如 `netstandard1.4`）来设定 .NET Standard 框架作为目标。 如果构建的库将用于在多个运行时上运行，就应将此框架作为目标。 对于最广泛的 API 集，将 `netstandard2.0` 设定为目标，因为 .NET Standard 2.0 的可用 API 数量比 .NET Standard 1.6 的两倍还多。

[`NETStandard.Library`](https://www.nuget.org/packages/NETStandard.Library/) 元包引用定义 .NET Standard 的一整套 NuGet 包。  要指定 `netstandard` 作为目标，最常见的方法是引用此元包。 它描述并提供了对大约 40 个 .NET 库及定义 .Net Standard 的相关 API 的访问权限。 可以引用以 `netstandard` 为目标的其他包来使用其他 API。

### <a name="versioning"></a>版本管理

规范并不是单一的，而是一组版本不断线性递增的 API。 该标准的第一个版本建立了一组基准 API。 后续版本将添加 API，并继承以前的版本定义的 API。 在从标准中移除 API 方面，并没有成文的规定。

.NET Standard 并不特定于任何一种 .NET 实现，也不与其中任一运行时的版本控制方案匹配。

添加到任何实现（例如 .NET Framework、.NET Core 和 Mono）的 API 可被视为适合添加到规范中的候选项，尤其是本质上非常重要的 API。 [.NET Standard 的新版本](https://github.com/dotnet/standard/blob/master/docs/versions.md)根据 .NET 实现版本进行创建，以便可以定位 .NET Standard PCL 中的新 API。

.NET Standard 版本控制对于库的使用至关重要。 在 .NET Standard 版本既定的情况下，可以使用定位相同或更低版本的库。 下面的做法介绍了使用 .NET Standard PCL（专用于 .NET Standard 定位）的工作流。

- 选择要用于 PCL 的 .NET Standard 版本。
- 使用依赖相同或更低 .NET Standard 版本的库。
- 如果发现依赖更高 .NET Standard 版本的库，要么需要采用相同的版本，要么不要使用此库。

## <a name="targeting-net-standard"></a>定位 .NET Standard

可以结合使用 `netstandard` 框架和 NETStandard.Library 元包来构建.NET Standard 库。

## <a name="net-framework-compatibility-mode"></a>.NET Framework 兼容性模式

从 .NET Standard 2.0 开始，引入了 .NET Framework 兼容性模式。 此兼容性模式允许 .NET Standard 项目引用 .NET Framework 库，就像其针对 .NET Standard 编译一样。 引用 .NET Framework 库并不适用于所有项目，，例如使用 Windows Presentation Foundation (WPF) API 的库。

有关详细信息，请参阅 [.NET Framework兼容性模式](https://docs.microsoft.com/zh-cn/dotnet/core/porting/third-party-deps#net-framework-compatibility-mode)。

## <a name="net-standard-libraries-and-visual-studio"></a>.NET Standard 库和 Visual Studio

要在 Visual Studio 中生成 .NET Standard 库，请确保 Windows 上已安装 [Visual Studio 2017 版本 15.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) 或更高版本，或 macOS 上已安装 [Visual Studio for Mac 版本 7.1](https://visualstudio.microsoft.com/vs/visual-studio-mac/) 或更高版本。

如果项目中只需使用 .NET Standard 2.0 库，也可在 Visual Studio 2015 中执行此操作。 但是需要安装 NuGet client 3.6 或更高版本。 可从 [NuGet 下载](https://www.nuget.org/downloads)页面下载适用于 Visual Studio 2015 的 NuGet 客户端。

### <a name="tooling-support-for-net-standard-libraries"></a>.NET Standard 库的工具支持

随着 .NET Core 2.0 和 .NET Standard 2.0 发布，Visual Studio 2017 和 [.NET Core 命令行接口 (CLI)](../../core/tools/index.md) 均包含创建 .NET Standard 库所需的工具支持。

如果安装含 .NET Core 跨平台开发工作负荷的 Visual Studio，可以使用项目模板创建 .NET Standard 2.0 库项目，如下图所示：

![添加新的 .NET Standard 库项目](./resource/std-project-cs.png)

如果使用的是 .NET Core CLI，可以运行下面的 [dotnet new](../../core/tools/dotnet-new.md) 命令，以创建定目标到 .NET Standard 2.0 的类库项目：

```
dotnet new classlib
```

## <a name="see-also"></a>请参阅
 
- [.NET Standard 简介](https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/)
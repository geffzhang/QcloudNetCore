# <a name="building-docker-images-for-net-core-applications"></a>为 .NET Core 应用程序生成 Docker 映像

 首先，我们探讨 Microsoft 维护和提供的各种不同的 Docker 映像，及其使用情况。 然后讲解了如何生成和 Docker 化 ASP.NET Core 应用。


## <a name="docker-image-optimizations"></a>Docker 映像优化

为开发人员生成 Docker 映像时，侧重于以下三种主要方案：

* 用于开发 .NET Core 应用的映像
* 用于生成 .NET Core 应用的映像
* 用于运行 .NET Core 应用的映像

为什么是三个映像？
因为在开发、生成和运行容器化应用程序时，具有不同的优先级。

* **开发：** 优先级注重循环访问更改的速度以及调试更改的能力。 与更改代码并且快速查看相比，映像的大小是否不是那么重要？

* **生成：** 此映像包含编译应用所需的所有内容，其中包括编译器和任何其他用于优化二进制文件的依赖项。  可使用生成映像创建置于生产映像中的资产。 生成映像用于持续集成或用于生成环境中。 此方法允许生成代理在生成映像实例中编译和生成应用程序（包括所有必需的依赖项）。 生成代理只需要了解如何运行此 Docker 映像即可。

* **生产：** 部署和启动映像的速度可以有多快？ 此映像很小，因此从 Docker 注册表到 Docker 主机的网络性能得到了优化。 已准备运行内容，以此实现从 Docker 运行到处理结果的最快时间。 Docker 模型中不需要动态代码编译。 放置在此映像中的内容将限制为运行应用程序所需的二进制文件和内容。

    例如，`dotnet publish` 输出包含：

    * 已编译的二进制文件
    * .js 和 .css 文件


在生产映像中包括 `dotnet publish` 命令输出的原因是使生产映像保持最小大小。

某些 .NET Core 映像共享不同标记之间的层，因此下载最新标记是一个相对轻量的过程。 如果计算机上已有较早版本，此体系结构会降低所需的磁盘空间。

当多个应用程序在同一计算机上使用公共映像时，在公共映像之间共享内存。 映像必须相同才可共享。

## <a name="docker-image-variations"></a>Docker 映像变体

为了实现上述目标，我们在 [`microsoft/dotnet`](https://hub.docker.com/r/microsoft/dotnet/) 下提供了映像变体。

* `microsoft/dotnet:<version>-sdk`(`microsoft/dotnet:2.1-sdk`) 此映像包含带有 .NET Core 和命令行工具 (CLI) 的 .NET Core SDK。 此映像将映射到**开发方案**。 可使用此映像进行本地开发、调试和单元测试。 此映像还可用于**生成**方案。 使用 `microsoft/dotnet:sdk` 始终都提供最新版本。

> [!TIP]
> 如果不确定自己的需求，需使用 `microsoft/dotnet:<version>-sdk` 映像。 作为“实际”映像，它旨在用作一次性容器（装载源代码并启动容器以启动应用）以及生成其他映像的基础映像。

* `microsoft/dotnet:<version>-runtime`：此映像包含 .NET Core（运行时和库），并且针对在生产环境中运行 .NET Core 应用进行了优化。

## <a name="alternative-images"></a>备用映像

除了开发、生成和生产的优化方案外，我们还提供了其他映像：

* `microsoft/dotnet:<version>-runtime-deps`：runtime-deps 映像包括具有 .NET Core 所需的所有本机依赖项的操作系统。 此映像适用于独立应用程序

每个变体的最新版本：

* `microsoft/dotnet` 或 `microsoft/dotnet:latest`（SDK 映像的别名）
* `microsoft/dotnet:sdk`
* `microsoft/dotnet:runtime`
* `microsoft/dotnet:runtime-deps`

## <a name="samples-to-explore"></a>要了解的示例

* [此 ASP.NET Core Docker 示例](https://github.com/dotnet/dotnet-docker/tree/master/samples/aspnetapp)演示了生成 ASP.NET Core 应用程序的 Docker 映像以用于生产的最佳实践模式。 此示例适用于 Linux 和 Windows 容器。

* 此 .NET Core Docker 示例演示了[生成 .NET Core 应用程序的 Docker 映像以用于生产](https://github.com/dotnet/dotnet-docker/tree/master/samples/dotnetapp)的最佳实践模式。

## <a name="forward-the-request-scheme-and-original-ip-address"></a>转发请求方案和原始 IP 地址

代理服务器、负载均衡器和其他网络设备通常会在请求到达容器化应用之前隐藏有关请求的信息：

* 当通过 HTTP 代理 HTTPS 请求时，原方案 (HTTPS) 将丢失，并且必须在标头中转接。
* 由于应用收到来自代理的请求，而不是 Internet 或公司网络上请求的真实源，因此原始客户端 IP 地址也必须在标头中转接。

此信息在请求处理中可能很重要，例如在重定向、身份验证、链接生成、策略评估和客户端地理位置中。

要将方案和原始 IP 地址转发到 ASP.NET Core 容器化应用，请使用 Forwarded Headers 中间件。 有关详细信息，请参阅[配置 ASP.NET Core 以使用代理服务器和负载均衡器](https://docs.microsoft.com/zh-cn/aspnet/core/host-and-deploy/proxy-load-balancer?view=aspnetcore-2.1)。

## <a name="your-first-aspnet-core-docker-app"></a>第一个 ASP.NET Core Docker 应用

本教程对要 Docker 化的应用使用 ASP.NET Core Docker 示例应用程序。 此 ASP.NET Core Docker 示例应用程序演示了生成 ASP.NET Core 应用程序的 Docker 映像以用于生产的最佳做法模式。 此示例适用于 Linux 和 Windows 容器。

该示例 Dockerfile 基于 ASP.NET Core 运行时 Docker 基础映像创建 ASP.NET Core 应用程序 Docker 映像。

它使用 [Docker 多阶段生成功能](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)完成以下操作：

* 基于较大的 ASP.NET Core 生成 Docker 基础映像在容器中生成示例 
* 基于较小的 ASP.NET Core Docker 运行时基础映像将最终生成结果复制到 Docker 映像

> [!NOTE]
> 生成映像包含生成应用程序所需的工具，而运行时映像不包括这些工具。

### <a name="prerequisites"></a>系统必备

要进行生成并运行，请安装以下各项：

#### <a name="net-core-21-sdk"></a>.NET Core 2.1 SDK

* 安装 [.NET Core SDK 2.1](https://www.microsoft.com/net/core)。

* 安装常用的代码编辑器（如果尚未安装）。

> [!TIP]
> 需要安装代码编辑器？ 试用 [Visual Studio](https://visualstudio.com/downloads)！

#### <a name="installing-docker-client"></a>安装 Docker 客户端

安装 [Docker 18.03](https://docs.docker.com/release-notes/docker-ce/) 或更高版本的 Docker 客户端。

可在以下位置安装 Docker 客户端：

* Linux 分布

   * [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)

   * [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)

   * [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)

   * [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

* [macOS](https://docs.docker.com/docker-for-mac/install/)

* [Windows](https://docs.docker.com/docker-for-windows/install/)。

#### <a name="installing-git-for-sample-repository"></a>为示例存储库安装 Git

* 如果要克隆存储库，请安装 [git](https://git-scm.com/download)。

### <a name="getting-the-sample-application"></a>获取示例应用程序

获取该示例的最简单方法是使用以下指令克隆包含 git 的 [.NET Core Docker 存储库](https://github.com/dotnet/dotnet-docker)： 

```console
git clone https://github.com/dotnet/dotnet-docker
```

也可以从 .NET Core Docker 存储库中下载 zip 格式的存储库（其大小较小）。

### <a name="run-the-aspnet-app-locally"></a>在本地运行 ASP.NET 应用

对于引用点，在容器化应用程序之前，请先在本地运行应用程序。

可使用以下命令，通过 .NET Core 2.1 SDK 在本地生成和运行应用程序（这些指令假定使用存储库的根目录）：

```console
cd dotnet-docker
cd samples
cd aspnetapp // solution scope where the dockerfile is located
cd aspnetapp // project scope

dotnet run
```

应用程序启动后，在 Web 浏览器中访问 **http://localhost:5000**。

### <a name="build-and-run-the-sample-with-docker-for-linux-containers"></a>使用 Docker for Linux 容器生成和运行示例

可使用以下命令，通过 Linux 容器在 Docker 中生成和运行示例（这些指令假定使用存储库的根目录）：

```console
cd dotnet-docker
cd samples
cd aspnetapp // solution scope where the dockerfile is located

docker build -t aspnetapp .
docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
```

> [!NOTE]
> `docker run` 的“-p”参数将本地计算机上的端口 5000 映射到容器中的端口 80（该端口映射形式为 `host:container`）。 有关详细信息，请参阅命令行参数中的 [docker run](https://docs.docker.com/engine/reference/commandline/exec/) 引用。

应用程序启动后，在 Web 浏览器中访问 **http://localhost:5000**。

### <a name="build-and-run-the-sample-with-docker-for-windows-containers"></a>使用 Docker for Windows 容器生成和运行示例

可使用以下命令，通过 Windows 容器在 Docker 中生成和运行示例（这些指令假定使用存储库的根目录）：

```console
cd dotnet-docker
cd samples
cd aspnetapp // solution scope where the dockerfile is located

docker build -t aspnetapp .
docker run -it --rm --name aspnetcore_sample aspnetapp
```

> [!IMPORTANT]
> 使用 Windows 容器时，必须直接在浏览器中转到容器 IP 地址（而不是 `http://localhost`）。 可通过以下步骤获取容器的 IP 地址：

* 打开另一个命令提示符。
* 运行 `docker ps`，查看正在运行的容器。 其中应包含“Aspnetcore_sample”。
* 运行 `docker exec aspnetcore_sample ipconfig`。
* 将容器 IP 地址复制并粘贴到浏览器中（例如，172.29.245.43）。

> [!NOTE]
> Docker exec 支持通过名称或哈希标识容器。 示例中使用了名称 (aspnetcore_sample)。


> [!NOTE]
> Docker exec 在正在运行的容器中运行新命令。 有关详细信息，请参阅命令行参数中的 [docker exec 引用](https://docs.docker.com/engine/reference/commandline/exec/)。

可使用 dotnet publish 命令生成已准备好在本地部署到生产环境的应用程序。

```console
dotnet publish -c Release -o published
```

> [!NOTE]
> -c Release 参数用于在发布模式（默认为调试模式）下生成应用程序。 有关详细信息，请参阅命令行参数中的 dotnet run 引用。

可使用以下命令在 Windows 中运行应用程序。

```console
dotnet published\aspnetapp.dll
```

可使用以下命令在 Linux 或 macOS 中运行应用程序。

```bash
dotnet published/aspnetapp.dll
```

### <a name="docker-images-used-in-this-sample"></a>此示例中使用的 Docker 映像

此示例的 dockerfile 中使用了以下 Docker 映像。

* `microsoft/dotnet:2.1-sdk`
* `microsoft/dotnet:2.1-aspnetcore-runtime`

祝贺你！ 你刚才已：
> [!div class="checklist"]
> * 了解 Microsoft .NET Core Docker 映像
> * 获取要 Docker 化的 ASP.NET Core 示例应用
> * 在本地运行 ASP.NET 示例应用
> * 使用 Docker for Linux 容器生成和运行示例
> * 使用 Docker for Windows 容器生成和运行示例

> [!NOTE]
> 如果你没有 腾讯云账号，请[立即注册](https://cloud.tencent.com/act/free?from=10107 )获取一个包含 7 款热门产品和 42 款长期免费云产品服务的任意组合。
# <a name="introduction-to-net-and-docker"></a>.NET 和 Docker 简介

本文提供了如何在 Docker 上使用 .NET 的简介和概念背景。

## Docker：打包应用程序以在任何位置部署和运行

Docker 是一个开放平台，使开发人员和管理员可以在称为[容器](https://www.docker.com/what-container)的松散隔离的环境中构建[映像](https://docs.docker.com/glossary/?term=image)、交付和运行分布式应用程序。 此方法可以在开发、QA 和生产环境之间进行高效的应用程序生命周期管理。
 
[Docker 平台](https://docs.docker.com/engine/docker-overview/#the-docker-platform)使用 [Docker 引擎](https://docs.docker.com/engine/docker-overview/#docker-engine)快速构建应用程序，并将其打包为使用以 [Dockerfile](https://docs.docker.com/glossary/?term=Dockerfile) 格式编写的文件创建的 [Docker 映像](https://docs.docker.com/glossary/?term=image)，然后在[分层容器](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/#container-and-layers)中部署和运行。

你可以创建自己的[分层映像](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/#images-and-layers)作为 dockerfiles，或使用[注册表](https://docs.docker.com/glossary/?term=registry)中的现有映像，例如 [Docker 中心](https://docs.docker.com/glossary/?term=Docker%20Hub)。

使用 Docker 时，开发人员会创建一个应用或服务，并将它及其依赖项打包到一个容器映像中。 映像是应用或服务及其配置和依赖项的静态表示形式。

若要运行应用或服务，应用的映像会实例化，以创建一个在 Docker 主机上运行的容器。 最初，会在开发环境或 PC 中测试容器。

开发人员应将映像存储在注册表中，该注册表充当映像库，并且在部署到生产业务流程协调程序时需要用到该注册表。 Docker 通过 Docker Hub维护一个公共注册表；其他供应商为不同映像集合提供注册表，包括 腾讯云容器注册表。 或者，企业可本地拥有一个专用注册表，用于其 Docker 映像。
![docker基本概念和术语](./resource/dockerbasic.png)

通过将映射存储到注册表中，可存储静态和不可变的应用程序，包括其在框架级别的所有依赖项。 然后可将这些映像部署到多种环境中，并进行版本控制，从而提供一致的部署单元
无论是托管在本地还是托管在云中，在下列情况下都建议使用私有映像注册表：
 - 由于保密性，映像不能被公开分享。
 - 希望映像和所选部署环境之间的网络延迟保持在最低水平。 例如，如果生产环境是腾讯 云，为实现最低的网络延迟，需要将映像存储在 [腾讯云 容器注册表](https://cloud.tencent.com/document/product/457/9118)中。 同样，如果生产环境是在本地，则需要在同一本地网络中使用本地的 Docker 信任的注册表。


### <a name="getting-net-docker-images"></a>获取 .NET Docker 映像

由 Microsoft 创建和优化官方 .NET Docker 映像。 这些映像在 Docker 中心的 Microsoft 存储库中公开提供。 每个存储库可以包含多个映像，具体取决于 .NET 和 OS 版本。 大多数映像存储库提供广泛的标记，以帮助选择特定的框架版本和 OS（Linux 发行版或 Windows 版本）。

## <a name="scenario-based-guidance"></a>基于方案的指南

Microsoft 对 .NET 存储库的打算是要有细化和集中存储库，表示特定方案或工作负载。

`microsoft/aspnetcore` 映像针对 Docker 上的 ASP.NET Core 应用进行了优化，因此容器可以更快启动。

.NET Core 映像 (`microsoft/dotnet`) 适用于基于 .NET Core 的控制台应用。 例如，批处理、Azure WebJobs 和其他控制台方案应该使用优化的 .NET Core 映像。

使用 Docker 和 .NET 应用程序最显而易见的水平方案是用于生产部署和托管。 事实证明，生产只是一种方案，其他方案同样有用。 这些方案不是特定于 .NET，但应该适用于大多数开发人员平台。

* **低摩擦安装** — 你可以尝试使用 .NET，无需本地安装。 只下载 Docker 映像，其中包含 .NET。

* **在容器中开发**你可以在一致的环境中开发，使开发和生产环境类似（可避免一些问题，例如开发人员计算机上的全局状态）。 通过 Visual Studio Tools for Docker，你甚至可以直接从 Visual Studio 启动容器。

* **容器中测试** — 你可以在容器中测试，减少由于环境配置不当或上次测试遗留的其他更改而导致的故障。

* **在容器中生成** — 你可以在容器中生成代码，消除为多个环境正确配置共享生成计算机的需要，而是转向“BYOC”（自带容器）方法。

* **在所有环境中部署** — 可以通过你的所有环境部署映像。 这种方法减少了配置差异导致的故障，通常通过外部配置（例如，注入的环境变量）改变映像行为。


### <a name="common-docker-development-scenarios"></a>常见的 Docker 开发方案

#### <a name="net-core"></a>.NET Core

**.NET Core 资源**

 选择适合你感兴趣方案的 .NET Core 示例。 所有示例说明都介绍了如何从 Windows、Linux 或 macOS 主机定位 Windows 或 Linux Docker 映像。

这些示例使用 .NET Core 2.0。 它们会在适用时使用 Docker [多阶段生成](https://github.com/dotnet/announcements/issues/18)和[多拱形标记](https://github.com/dotnet/announcements/issues/14)。

* [DockerHub 上的 .NET Core 映像](https://hub.docker.com/r/microsoft/dotnet/)

* [使 .NET Core 应用程序 Docker 化](https://docs.docker.com/engine/examples/dotnetcore/)

* 此 .NET Core Docker 示例演示如何[在 .NET Core 开发过程中使用 Docker](https://github.com/dotnet/dotnet-docker-samples/tree/master/dotnetapp-dev)。 此示例适用于 Linux 和 Windows 容器。

* 此 .NET Core Docker 示例演示了[生成 .NET Core 应用程序的 Docker 映像以用于生产](https://github.com/dotnet/dotnet-docker-samples/tree/master/dotnetapp-prod)的最佳实践模式。 此示例适用于 Linux 和 Windows 容器。

* 此 [.NET Core Docker 示例](https://github.com/dotnet/dotnet-docker-samples/tree/master/dotnetapp-selfcontained)演示了生成[独立式 .NET Core 应用程序](../deploying/index.md)的 Docker 映像的最佳实践模式。 用于最小的生产容器，而不会受益于[在容器之间共享基础映像](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/)。 但是，可以共享较低的 Docker 层。

#### <a name="aspnet-core"></a>ASP.NET Core

* [此 ASP.NET Core Docker 示例](https://github.com/dotnet/dotnet-docker-samples/tree/master/aspnetapp)演示了生成 ASP.NET Core 应用程序的 Docker 映像以用于生产的最佳实践模式。 此示例适用于 Linux 和 Windows 容器。

* [DockerHub 上的 ASP.NET Core 映像](https://hub.docker.com/r/microsoft/aspnetcore-build/)

* [GitHub 上的 ASP.NET Core 映像](https://github.com/aspnet/aspnet-docker)


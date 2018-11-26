# 腾讯云开发者工具套件（ SDK ）3.0

腾讯云 API 全新升级 3.0 ，该版本进行了性能优化且全地域部署、支持就近和按地域接入、访问时延下降显著，接口描述更加详细、错误码描述更加全面、SDK增加接口级注释，让您更加方便快捷的使用腾讯云产品。

腾讯云开发者工具套件（ SDK ）3.0 ，SDK 3.0 是云 API 3.0 平台的配套工具。后续所有的云服务产品都会接入进来。新版 SDK 实现了统一化，具有各个语言版本的 SDK 使用方法相同，接口调用方式相同，统一的错误码和返回包格式这些优点。

为方便 .NET 开发者调试和接入腾讯云产品 API ，这里向您介绍适用于 .NET 的腾讯云开发工具包，开源地址是 https://github.com/TencentCloud/tencentcloud-sdk-dotnet ，对应的Nuget 包：https://www.nuget.org/packages/TencentCloudSDK/ 

![腾讯云开发者套件3.0 Sdk](./resource/tencentcloudsdk.png)
微软发布了.NET 代码库指南https://docs.microsoft.com/zh-cn/dotnet/standard/library-guidance/ ，指导你为.NET创建高质量代码库。该指南包含我们已确定的适用于大多数公共.NET库的 最佳实践。希望帮助.NET开发人员构建具有以下方面的优秀库：
- 包容性：优秀的.NET库致力于支持众多平台和应用程序。
- 稳定性：优秀的.NET 系统在具有众多库的应用程序中运行的 .NET 生态系统中共存。
- 设计为可改进：.NET 库要随着时间的推移进行改进和演变，同时支持现有用户。
- 可调试：.NET库要使用最新的工具，为用户打造卓越的调试体验。
- 受信任：.NE 库通过安全最佳做法发布到 NuGet，备受开发人员的信赖。

TencentCloudSDK遵循.NET 代码库指南，结合国内实践设定支持.NETStandard 2.0 和.NETFramewrok 4.5 作为主要的运行平台。以便更轻松地构建.NET库，包括跨平台支持，.NET Standard以及与NuGet的紧密集成。

我们后面的动手实验中将使用腾讯云开发者工具套件（SDK） 3.0 使用腾讯云的产品。
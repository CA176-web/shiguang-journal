# 第三方组件与许可证

本项目名称与图标独立设计，没有打包 Diarium 程序、图标或私有代码。Diarium 名称仅用于说明功能参考与数据迁移来源。

| 组件 | 固定版本 | 许可证 / 来源 |
| --- | --- | --- |
| .NET Windows Desktop Runtime | 10.0.14，由 SDK 10.0.403 发布 | MIT 及其第三方通知，见 licenses 文件夹 |
| Microsoft.Data.Sqlite / Core | 10.0.14 | MIT，Microsoft Corporation |
| SQLitePCLRaw bundle / core / provider / native library | 2.1.14 | Apache-2.0，SourceGear LLC；其中 SQLite 引擎为 Public Domain |
| NAudio 及其依赖模块 | 2.4.0 | MIT，Mark Heath 及贡献者 |
| PDFsharp-WPF | 6.2.4（精确固定） | MIT，empira Software GmbH；见 `licenses/PDFsharp-6.2.4-MIT.txt` |
| Microsoft.Extensions.Logging.Abstractions / DependencyInjection.Abstractions | 8.0.3 / 8.0.2 | MIT，Microsoft Corporation |

本安装分享包附带许可文本。发布包含运行时自身通知以及 `licenses` 下的 MIT、Apache 许可文本。SQLite 官方版权说明：[sqlite.org/copyright.html](https://www.sqlite.org/copyright.html)。

NAudio 项目：[github.com/naudio/NAudio](https://github.com/naudio/NAudio)。SQLitePCLRaw 项目：[github.com/ericsink/SQLitePCL.raw](https://github.com/ericsink/SQLitePCL.raw)。Microsoft.Data.Sqlite 属于 EF Core：[github.com/dotnet/efcore](https://github.com/dotnet/efcore)。

## PDF 导出

PDF 引擎固定使用 [PDFsharp-WPF 6.2.4](https://www.nuget.org/packages/PDFsharp-WPF/6.2.4)，该版本的 NuGet 元数据及[对应版本许可证](https://github.com/empira/PDFsharp/blob/v6.2.4/LICENSE)均为 MIT。6.5.0 及之后的许可已经变化，升级依赖前需重新核对，不能直接取消版本约束。

PDF 在本机生成，文字保留 Unicode 映射和内嵌字体子集。中文字体来自运行电脑安装的 Windows 字体，经 WPF 字体 API 处理；安装包不分发微软雅黑等 Windows 字体文件。PDF 内嵌可预览照片，原始照片及其他附件同时保存在配套文件夹。

自动测试项目另用 [PdfPig 0.1.16](https://www.nuget.org/packages/PdfPig/0.1.16)（Apache-2.0）独立解析文本、页数、图片及链接；该测试依赖不随程序安装包分发。

## MIT 许可

Copyright (c) Microsoft Corporation. All rights reserved.

Copyright (c) Mark Heath and NAudio contributors.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


## OpenStreetMap（可选在线地图）

在线地图数据 © OpenStreetMap contributors，适用 [Open Database License](https://www.openstreetmap.org/copyright)。栅格图块由 `tile.openstreetmap.org` 按需提供，遵守 [Tile Usage Policy](https://operations.osmfoundation.org/policies/tiles/)。程序未捆绑离线图块包；加载地图前由用户主动启用，界面持续显示来源链接。网络服务可用性不由本软件保证。

## Open-Meteo（可选天气）

天气预报及历史模型数据来自 [Open-Meteo](https://open-meteo.com/)，按 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 署名展示。界面包含来源链接。此版本使用其免费非商业 API，使用范围和请求限额以 [服务条款](https://open-meteo.com/en/terms) 为准。免费服务不提供可用性保证。天气仅在用户启用后按日记坐标请求，不上传正文、标题、照片或联系人。

接口说明：[天气预报](https://open-meteo.com/en/docs)、[历史天气](https://open-meteo.com/en/docs/historical-weather-api)。本机 ICS 解析参考 [RFC 5545](https://www.rfc-editor.org/rfc/rfc5545.html)，独立 WebDAV 同步参考 [RFC 4918](https://www.rfc-editor.org/rfc/rfc4918.html) 与 [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html)。

## Google Drive（可选云同步）

0.5.0 通过 HTTPS 调用 Google 的 [桌面 OAuth 接口](https://developers.google.com/identity/protocols/oauth2/native-app) 和 [Drive API v3 应用数据接口](https://developers.google.com/workspace/drive/api/guides/appdata)，没有新增 Google SDK 依赖或打包第三方客户端凭据。用户需注册并导入自己的桌面客户端配置，再主动通过 Google 浏览器页面授权。

Google Drive 名称用于标识所连接的服务，不表示 Google 为本程序提供认证或背书。网络服务、账号权限和配额由 Google 管理；拾光独立加密同步格式与原版 Diarium 的云文件不互通。

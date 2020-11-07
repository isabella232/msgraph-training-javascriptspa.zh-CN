---
ms.openlocfilehash: bbb1dd429dca4e67735c5fdb2b2280f729115eb2
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822787"
---
<!-- markdownlint-disable MD002 MD041 -->

本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 JavaScript 单页面应用。

> [!TIP]
> 如果您只想下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-javascriptspa)。

## <a name="prerequisites"></a>必备条件

在开始本教程之前，你将需要访问 HTTP 服务器以承载示例文件。 这可以是开发计算机上的测试服务器，也可以是远程服务器上的服务器。 本教程包含使用 Node.js 包在开发计算机上运行简单测试服务器的说明。 如果您计划使用此选项，则您的开发计算机上应安装有 [Node.js](https://nodejs.org) 。 如果您没有 Node.js，请访问 "下载选项" 的上一个链接。

您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。

> [!NOTE]
> 本教程是使用节点版本12.16.1 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。

## <a name="feedback"></a>反馈

请在 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-javascriptspa)中提供有关本教程的任何反馈。

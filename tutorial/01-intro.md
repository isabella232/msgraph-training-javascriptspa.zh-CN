---
ms.openlocfilehash: bbb1dd429dca4e67735c5fdb2b2280f729115eb2
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822787"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9fa0-101">本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 JavaScript 单页面应用。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-101">This tutorial teaches you how to build a JavaScript single-page app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="b9fa0-102">如果您只想下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-javascriptspa)。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-javascriptspa).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9fa0-103">必备条件</span><span class="sxs-lookup"><span data-stu-id="b9fa0-103">Prerequisites</span></span>

<span data-ttu-id="b9fa0-104">在开始本教程之前，你将需要访问 HTTP 服务器以承载示例文件。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-104">Before you start this tutorial, you will need access to an HTTP server to host the sample files.</span></span> <span data-ttu-id="b9fa0-105">这可以是开发计算机上的测试服务器，也可以是远程服务器上的服务器。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-105">This could be a test server on your development machine, or a remote server.</span></span> <span data-ttu-id="b9fa0-106">本教程包含使用 Node.js 包在开发计算机上运行简单测试服务器的说明。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-106">The tutorial includes instructions to use a Node.js package to run a simple test server on your development machine.</span></span> <span data-ttu-id="b9fa0-107">如果您计划使用此选项，则您的开发计算机上应安装有 [Node.js](https://nodejs.org) 。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-107">If you plan to use this option, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="b9fa0-108">如果您没有 Node.js，请访问 "下载选项" 的上一个链接。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-108">If you do not have Node.js, visit the previous link for download options.</span></span>

<span data-ttu-id="b9fa0-109">您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-109">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="b9fa0-110">如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="b9fa0-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="b9fa0-111">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="b9fa0-112">你可以 [注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) 以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="b9fa0-113">本教程是使用节点版本12.16.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-113">This tutorial was written with Node version 12.16.1.</span></span> <span data-ttu-id="b9fa0-114">本指南中的步骤可能适用于其他版本，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-114">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="b9fa0-115">反馈</span><span class="sxs-lookup"><span data-stu-id="b9fa0-115">Feedback</span></span>

<span data-ttu-id="b9fa0-116">请在 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-javascriptspa)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="b9fa0-116">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-javascriptspa).</span></span>

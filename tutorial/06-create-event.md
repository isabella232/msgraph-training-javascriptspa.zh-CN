---
ms.openlocfilehash: 177f436812050ab4e9bf6c86bb5229b30f0f885b
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822808"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="32f53-101">在本节中，您将添加在用户日历上创建事件的功能。</span><span class="sxs-lookup"><span data-stu-id="32f53-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-a-new-event-form"></a><span data-ttu-id="32f53-102">创建新的事件表单</span><span class="sxs-lookup"><span data-stu-id="32f53-102">Create a new event form</span></span>

1. <span data-ttu-id="32f53-103">将以下函数添加到 **ui.js** ，以呈现新事件的窗体。</span><span class="sxs-lookup"><span data-stu-id="32f53-103">Add the following function to **ui.js** to render a form for the new event.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="showNewEventFormSnippet":::

## <a name="create-the-event"></a><span data-ttu-id="32f53-104">创建事件</span><span class="sxs-lookup"><span data-stu-id="32f53-104">Create the event</span></span>

1. <span data-ttu-id="32f53-105">将以下函数添加到 **graph.js** ，以从表单中读取值并创建新事件。</span><span class="sxs-lookup"><span data-stu-id="32f53-105">Add the following function to **graph.js** to read the values from the form and create a new event.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="createEventSnippet":::

1. <span data-ttu-id="32f53-106">保存所做的更改并刷新应用程序。</span><span class="sxs-lookup"><span data-stu-id="32f53-106">Save your changes and refresh the app.</span></span> <span data-ttu-id="32f53-107">单击 " **日历** " 导航项，然后单击 " **创建事件** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="32f53-107">Click the **Calendar** nav item, then click the **Create event** button.</span></span> <span data-ttu-id="32f53-108">填写值，然后单击 " **创建** "。</span><span class="sxs-lookup"><span data-stu-id="32f53-108">Fill in the values and click **Create**.</span></span> <span data-ttu-id="32f53-109">创建新事件后，应用程序将返回到日历视图。</span><span class="sxs-lookup"><span data-stu-id="32f53-109">The app returns to the calendar view once the new event is created.</span></span>

    ![新事件表单的屏幕截图](images/create-event-01.png)

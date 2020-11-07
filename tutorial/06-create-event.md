---
ms.openlocfilehash: 177f436812050ab4e9bf6c86bb5229b30f0f885b
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822808"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将添加在用户日历上创建事件的功能。

## <a name="create-a-new-event-form"></a>创建新的事件表单

1. 将以下函数添加到 **ui.js** ，以呈现新事件的窗体。

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="showNewEventFormSnippet":::

## <a name="create-the-event"></a>创建事件

1. 将以下函数添加到 **graph.js** ，以从表单中读取值并创建新事件。

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="createEventSnippet":::

1. 保存所做的更改并刷新应用程序。 单击 " **日历** " 导航项，然后单击 " **创建事件** " 按钮。 填写值，然后单击 " **创建** "。 创建新事件后，应用程序将返回到日历视图。

    ![新事件表单的屏幕截图](images/create-event-01.png)

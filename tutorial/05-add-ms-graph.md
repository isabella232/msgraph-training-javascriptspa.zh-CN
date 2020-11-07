---
ms.openlocfilehash: 1f2ff0e013bd1b104cd5b353ba2da542382657ab
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822805"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将把 Microsoft Graph 合并到应用程序中。 对于此应用程序，您将使用 [Microsoft Graph JavaScript 客户端库](https://github.com/microsoftgraph/msgraph-sdk-javascript) 来调用 microsoft graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

在本节中，您将使用 Microsoft Graph 客户端库获取用户的日历事件。

1. 在名为 **timezones.js** 的项目的根目录中创建一个新文件，并添加以下代码。

    :::code language="javascript" source="../demo/graph-tutorial/timezones.js" id="zoneMappingsSnippet":::

    此代码将 Windows 时区标识符映射到 IANA 时区标识符，以便与 moment.js 兼容。

1. 将以下函数添加到 **graph.js** 。

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="getEventsSnippet":::

    考虑此代码执行的操作。

    - 将调用的 URL 为 `/me/calendarview` 。
    - `header`方法添加 `Prefer` 指定用户的首选时区的标头。
    - 该 `query` 方法将添加日历视图的开始和结束时间。
    - 此 `select` 方法将为每个事件返回的字段限制为只是视图实际使用的字段。
    - `orderby`方法按开始时间对结果进行排序，最早的事件最先开始。
    - 该 `top` 方法请求响应中的最高为50个事件。

1. 打开 **ui.js** 并添加以下函数。

    ```javascript
    function showCalendar(events) {
      // TEMPORARY
      // Render the results as JSON
      var alert = createElement('div', 'alert alert-success');

      var pre = createElement('pre', 'alert-pre border bg-light p-2');
      alert.appendChild(pre);

      var code = createElement('code', 'text-break',
        JSON.stringify(events, null, 2));
      pre.appendChild(code);

      mainContainer.innerHTML = '';
      mainContainer.appendChild(alert);
    }
    ```

1. 更新 `switch` `updatePage` 要在视图时调用的函数中的语句 `showCalendar` `Views.calendar` 。

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="updatePageSnippet" highlight="18-20":::

1. 保存所做的更改并刷新应用程序。 登录并单击导航栏中的 " **日历** " 链接。 如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。

## <a name="display-the-results"></a>显示结果

在本节中，您将更新 `showCalendar` 函数，以对用户更友好的方式显示事件。

1. 将现有的 `showCalendar` 函数替换为以下内容。

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="showCalendarSnippet":::

    这将遍历事件集合并为每个事件添加一个表行。

1. 保存更改并刷新应用。 单击 " **日历** " 链接，应用现在应呈现当前星期的事件表。

    ![事件表的屏幕截图](./images/calendar-list.png)

---
ms.openlocfilehash: 1f2ff0e013bd1b104cd5b353ba2da542382657ab
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822805"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d6548-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="d6548-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="d6548-102">对于此应用程序，您将使用 [Microsoft Graph JavaScript 客户端库](https://github.com/microsoftgraph/msgraph-sdk-javascript) 来调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="d6548-102">For this application, you will use the [Microsoft Graph JavaScript Client Library](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d6548-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="d6548-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="d6548-104">在本节中，您将使用 Microsoft Graph 客户端库获取用户的日历事件。</span><span class="sxs-lookup"><span data-stu-id="d6548-104">In this section, you'll use the Microsoft Graph client library to get calendar events for the user.</span></span>

1. <span data-ttu-id="d6548-105">在名为 **timezones.js** 的项目的根目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="d6548-105">Create a new file in the root of the project named **timezones.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/timezones.js" id="zoneMappingsSnippet":::

    <span data-ttu-id="d6548-106">此代码将 Windows 时区标识符映射到 IANA 时区标识符，以便与 moment.js 兼容。</span><span class="sxs-lookup"><span data-stu-id="d6548-106">This code maps Windows time zone identifiers to IANA time zone identifiers for compatibility with moment.js.</span></span>

1. <span data-ttu-id="d6548-107">将以下函数添加到 **graph.js** 。</span><span class="sxs-lookup"><span data-stu-id="d6548-107">Add the following function to **graph.js**.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="getEventsSnippet":::

    <span data-ttu-id="d6548-108">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="d6548-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="d6548-109">将调用的 URL 为 `/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="d6548-109">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="d6548-110">`header`方法添加 `Prefer` 指定用户的首选时区的标头。</span><span class="sxs-lookup"><span data-stu-id="d6548-110">The `header` method adds a `Prefer` header specifying the user's preferred time zone.</span></span>
    - <span data-ttu-id="d6548-111">该 `query` 方法将添加日历视图的开始和结束时间。</span><span class="sxs-lookup"><span data-stu-id="d6548-111">The `query` method adds the start and end times for the calendar view.</span></span>
    - <span data-ttu-id="d6548-112">此 `select` 方法将为每个事件返回的字段限制为只是视图实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="d6548-112">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="d6548-113">`orderby`方法按开始时间对结果进行排序，最早的事件最先开始。</span><span class="sxs-lookup"><span data-stu-id="d6548-113">The `orderby` method sorts the results by the start time, with the earliest event being first.</span></span>
    - <span data-ttu-id="d6548-114">该 `top` 方法请求响应中的最高为50个事件。</span><span class="sxs-lookup"><span data-stu-id="d6548-114">The `top` method requests up to 50 events in the response.</span></span>

1. <span data-ttu-id="d6548-115">打开 **ui.js** 并添加以下函数。</span><span class="sxs-lookup"><span data-stu-id="d6548-115">Open **ui.js** and add the following function.</span></span>

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

1. <span data-ttu-id="d6548-116">更新 `switch` `updatePage` 要在视图时调用的函数中的语句 `showCalendar` `Views.calendar` 。</span><span class="sxs-lookup"><span data-stu-id="d6548-116">Update the `switch` statement in the `updatePage` function to call `showCalendar` when the view is `Views.calendar`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="updatePageSnippet" highlight="18-20":::

1. <span data-ttu-id="d6548-117">保存所做的更改并刷新应用程序。</span><span class="sxs-lookup"><span data-stu-id="d6548-117">Save your changes and refresh the app.</span></span> <span data-ttu-id="d6548-118">登录并单击导航栏中的 " **日历** " 链接。</span><span class="sxs-lookup"><span data-stu-id="d6548-118">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="d6548-119">如果一切正常，应在用户的日历上看到一个 JSON 转储的事件。</span><span class="sxs-lookup"><span data-stu-id="d6548-119">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="d6548-120">显示结果</span><span class="sxs-lookup"><span data-stu-id="d6548-120">Display the results</span></span>

<span data-ttu-id="d6548-121">在本节中，您将更新 `showCalendar` 函数，以对用户更友好的方式显示事件。</span><span class="sxs-lookup"><span data-stu-id="d6548-121">In this section you will update the `showCalendar` function to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="d6548-122">将现有的 `showCalendar` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="d6548-122">Replace the existing `showCalendar` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/ui.js" id="showCalendarSnippet":::

    <span data-ttu-id="d6548-123">这将遍历事件集合并为每个事件添加一个表行。</span><span class="sxs-lookup"><span data-stu-id="d6548-123">This loops through the collection of events and adds a table row for each one.</span></span>

1. <span data-ttu-id="d6548-124">保存更改并刷新应用。</span><span class="sxs-lookup"><span data-stu-id="d6548-124">Save the changes and refresh the app.</span></span> <span data-ttu-id="d6548-125">单击 " **日历** " 链接，应用现在应呈现当前星期的事件表。</span><span class="sxs-lookup"><span data-stu-id="d6548-125">Click on the **Calendar** link and the app should now render a table of events for the current week.</span></span>

    ![事件表的屏幕截图](./images/calendar-list.png)

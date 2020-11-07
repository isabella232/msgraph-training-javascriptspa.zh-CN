---
ms.openlocfilehash: 0ef683278142776d6895b8655dd16e3635b90fa4
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822804"
---
<!-- markdownlint-disable MD002 MD041 -->

首先，为项目创建一个空目录。 这可以在 HTTP 服务器上，也可以在开发计算机上的目录中。 如果是在开发计算机上，则需要将其复制到服务器以进行测试，或在开发计算机上运行 HTTP 服务器。 如果您没有其中的任何一个，则下一节将提供相关说明。

## <a name="start-a-local-web-server-optional"></a>启动本地 web 服务器 (可选) 

> [!NOTE]
> 本节中的步骤需要 [Node.js](https://nodejs.org)。

在本节中，您将使用 [http-服务器](https://www.npmjs.com/package/http-server) 从命令行运行一个简单的 http 服务器。

1. 在为项目创建的目录中打开命令行界面 (CLI) 。
1. 运行以下命令以启动该目录中的 web 服务器。

    ```Shell
    npx http-server -c-1
    ```

1. 打开浏览器并浏览到 `http://localhost:8080` 。

您应该会看到 **/页面的索引** 。 这将确认 HTTP 服务器是否正在运行。

![由 http-服务器提供服务的索引页的屏幕截图。](images/run-web-server.png)

## <a name="design-the-app"></a>设计应用程序

在本节中，您将创建应用程序的基本 UI 布局。

1. 在名为 **index.html** 的项目的根目录中创建一个新文件，并添加以下代码。

    :::code language="html" source="../demo/graph-tutorial/index.html" id="indexSnippet":::

    这将定义应用程序的基本布局，包括导航栏。 此外，它还会添加以下内容：

    - [引导数据库](https://getbootstrap.com/) 及其支持的 JavaScript
    - [FontAwesome](https://fontawesome.com/)
    - [Moment.js](https://momentjs.com/)
    - [适用于 JavaScript 的 Microsoft 身份验证库 ( # A0) 2。0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)
    - [Microsoft Graph JavaScript 客户端库](https://github.com/microsoftgraph/msgraph-sdk-javascript)

    > [!TIP]
    > 页面包含一个 favicon， (`<link rel="shortcut icon" href="g-raph.png">`) 。 您可以删除此行，也可以从 [GitHub](https://github.com/microsoftgraph/g-raph)下载 **g-raph.png** 文件。

1. 创建一个名为 " **style** " 的新文件，并添加以下代码。

    :::code language="css" source="../demo/graph-tutorial/style.css":::

1. 创建一个名为 **auth.js** 的新文件，并添加以下代码。

    ```javascript
    function signIn() {
      // TEMPORARY
      updatePage({name: 'Megan Bowen', userName: 'meganb@contoso.com'});
    }

    function signOut() {
      // TEMPORARY
      updatePage();
    }
    ```

1. 创建一个名为 **ui.js** 的新文件，并添加以下代码。

    ```javascript
    // Select DOM elements to work with
    const authenticatedNav = document.getElementById('authenticated-nav');
    const accountNav = document.getElementById('account-nav');
    const mainContainer = document.getElementById('main-container');

    const Views = { error: 1, home: 2, calendar: 3 };

    function createElement(type, className, text) {
      var element = document.createElement(type);
      element.className = className;

      if (text) {
        var textNode = document.createTextNode(text);
        element.appendChild(textNode);
      }

      return element;
    }

    function showAuthenticatedNav(user, view) {
      authenticatedNav.innerHTML = '';

      if (user) {
        // Add Calendar link
        var calendarNav = createElement('li', 'nav-item');

        var calendarLink = createElement('button',
          `btn btn-link nav-link${view === Views.calendar ? ' active' : '' }`,
          'Calendar');
        calendarLink.setAttribute('onclick', 'getEvents();');
        calendarNav.appendChild(calendarLink);

        authenticatedNav.appendChild(calendarNav);
      }
    }

    function showAccountNav(user) {
      accountNav.innerHTML = '';

      if (user) {
        // Show the "signed-in" nav
        accountNav.className = 'nav-item dropdown';

        var dropdown = createElement('a', 'nav-link dropdown-toggle');
        dropdown.setAttribute('data-toggle', 'dropdown');
        dropdown.setAttribute('role', 'button');
        accountNav.appendChild(dropdown);

        var userIcon = createElement('i',
          'far fa-user-circle fa-lg rounded-circle align-self-center');
        userIcon.style.width = '32px';
        dropdown.appendChild(userIcon);

        var menu = createElement('div', 'dropdown-menu dropdown-menu-right');
        dropdown.appendChild(menu);

        var userName = createElement('h5', 'dropdown-item-text mb-0', user.displayName);
        menu.appendChild(userName);

        var userEmail = createElement('p', 'dropdown-item-text text-muted mb-0', user.mail || user.userPrincipalName);
        menu.appendChild(userEmail);

        var divider = createElement('div', 'dropdown-divider');
        menu.appendChild(divider);

        var signOutButton = createElement('button', 'dropdown-item', 'Sign out');
        signOutButton.setAttribute('onclick', 'signOut();');
        menu.appendChild(signOutButton);
      } else {
        // Show a "sign in" button
        accountNav.className = 'nav-item';

        var signInButton = createElement('button', 'btn btn-link nav-link', 'Sign in');
        signInButton.setAttribute('onclick', 'signIn();');
        accountNav.appendChild(signInButton);
      }
    }

    function showWelcomeMessage(user) {
      // Create jumbotron
      var jumbotron = createElement('div', 'jumbotron');

      var heading = createElement('h1', null, 'JavaScript SPA Graph Tutorial');
      jumbotron.appendChild(heading);

      var lead = createElement('p', 'lead',
        'This sample app shows how to use the Microsoft Graph API to access' +
        ' a user\'s data from JavaScript.');
      jumbotron.appendChild(lead);

      if (user) {
        // Welcome the user by name
        var welcomeMessage = createElement('h4', null, `Welcome ${user.displayName}!`);
        jumbotron.appendChild(welcomeMessage);

        var callToAction = createElement('p', null,
          'Use the navigation bar at the top of the page to get started.');
        jumbotron.appendChild(callToAction);
      } else {
        // Show a sign in button in the jumbotron
        var signInButton = createElement('button', 'btn btn-primary btn-large',
          'Click here to sign in');
        signInButton.setAttribute('onclick', 'signIn();')
        jumbotron.appendChild(signInButton);
      }

      mainContainer.innerHTML = '';
      mainContainer.appendChild(jumbotron);
    }

    function showError(error) {
      var alert = createElement('div', 'alert alert-danger');

      var message = createElement('p', 'mb-3', error.message);
      alert.appendChild(message);

      if (error.debug)
      {
        var pre = createElement('pre', 'alert-pre border bg-light p-2');
        alert.appendChild(pre);

        var code = createElement('code', 'text-break text-wrap',
          JSON.stringify(error.debug, null, 2));
        pre.appendChild(code);
      }

      mainContainer.innerHTML = '';
      mainContainer.appendChild(alert);
    }

    function updatePage(view, data) {
      if (!view) {
        view = Views.home;
      }

      const user = JSON.parse(sessionStorage.getItem('graphUser'));

      showAccountNav(user);
      showAuthenticatedNav(user, view);

      switch (view) {
        case Views.error:
          showError(data);
          break;
        case Views.home:
          showWelcomeMessage(user);
          break;
        case Views.calendar:
          break;
      }
    }

    updatePage(Views.home);
    ```

1. 保存所有更改并刷新页面。 现在，应用程序看起来应非常不同。

    ![重新设计的主页的屏幕截图](images/app-layout.png)

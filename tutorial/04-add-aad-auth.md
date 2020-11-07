---
ms.openlocfilehash: 77f74be505d72c6c0fd55d5f2650e32d03d63348
ms.sourcegitcommit: 18785a430bc1885ccac1ebbe1f3eba634c58bea8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822791"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b64a4-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="b64a4-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="b64a4-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="b64a4-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="b64a4-103">在此步骤中，将 [Microsoft 身份验证库](https://github.com/AzureAD/microsoft-authentication-library-for-js) 库集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="b64a4-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

1. <span data-ttu-id="b64a4-104">在名为 **config.js** 的根目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="b64a4-104">Create a new file in the root directory named **config.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/config.example.js" id="msalConfigSnippet":::

    <span data-ttu-id="b64a4-105">将替换 `YOUR_APP_ID_HERE` 为应用程序注册门户中的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="b64a4-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b64a4-106">如果您使用的是源代码管理（如 git），现在可以从源代码管理中排除 **config.js** 文件，以避免无意中泄漏您的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="b64a4-106">If you're using source control such as git, now would be a good time to exclude the **config.js** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="b64a4-107">打开 **auth.js** 并将以下代码添加到文件的开头。</span><span class="sxs-lookup"><span data-stu-id="b64a4-107">Open **auth.js** and add the following code to the beginning of the file.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/auth.js" id="authInitSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="b64a4-108">实施登录</span><span class="sxs-lookup"><span data-stu-id="b64a4-108">Implement sign-in</span></span>

<span data-ttu-id="b64a4-109">在本节中，您将实现 `signIn` 和 `signOut` 函数。</span><span class="sxs-lookup"><span data-stu-id="b64a4-109">In this section you'll implement the `signIn` and `signOut` functions.</span></span>

1. <span data-ttu-id="b64a4-110">将现有的 `signIn` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b64a4-110">Replace the existing `signIn` function with the following.</span></span>

    ```javascript
    async function signIn() {
      // Login
      try {
        // Use MSAL to login
        const authResult = await msalClient.loginPopup(msalRequest);
        console.log('id_token acquired at: ' + new Date().toString());
        // Save the account username, needed for token acquisition
        sessionStorage.setItem('msalAccount', authResult.account.username);
        // TEMPORARY
        updatePage(Views.error, {
          message: 'Login successful',
          debug: `Token: ${authResult.accessToken}`
        });
      } catch (error) {
        console.log(error);
        updatePage(Views.error, {
          message: 'Error logging in',
          debug: error
        });
      }
    }
    ```

    <span data-ttu-id="b64a4-111">在成功登录后，此临时代码将显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-111">This temporary code will display the access token after a successful login.</span></span>

1. <span data-ttu-id="b64a4-112">将现有的 `signOut` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b64a4-112">Replace the existing `signOut` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/auth.js" id="signOutSnippet":::

1. <span data-ttu-id="b64a4-113">保存更改并刷新页面。</span><span class="sxs-lookup"><span data-stu-id="b64a4-113">Save your changes and refresh the page.</span></span> <span data-ttu-id="b64a4-114">登录后，您应该会看到一个显示访问令牌的错误框。</span><span class="sxs-lookup"><span data-stu-id="b64a4-114">After you sign in, you should see an error box that shows the access token.</span></span>

## <a name="get-the-users-profile"></a><span data-ttu-id="b64a4-115">获取用户的配置文件</span><span class="sxs-lookup"><span data-stu-id="b64a4-115">Get the user's profile</span></span>

<span data-ttu-id="b64a4-116">在本节中，您将改进 `signIn` 函数，以使用访问令牌从 Microsoft Graph 获取用户的配置文件。</span><span class="sxs-lookup"><span data-stu-id="b64a4-116">In this section you'll improve the `signIn` function to use the access token to get the user's profile from Microsoft Graph.</span></span>

1. <span data-ttu-id="b64a4-117">在 **auth.js** 中添加以下函数以检索用户的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-117">Add the following function in **auth.js** to retrieve the user's access token.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/auth.js" id="getTokenSnippet":::

1. <span data-ttu-id="b64a4-118">在名为 **graph.js** 的项目的根目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="b64a4-118">Create a new file in the root of the project named **graph.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="graphInitSnippet":::

    <span data-ttu-id="b64a4-119">此代码创建auth.js中包装方法的授权提供程序 `getToken` ，并使用此提供程序初始化 Graph 客户端。 \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="b64a4-119">This code creates an authorization provider that wraps the `getToken` method in **auth.js** , and initializes the Graph client with this provider.</span></span>

1. <span data-ttu-id="b64a4-120">在 **graph.js** 中添加以下函数，以获取用户的配置文件。</span><span class="sxs-lookup"><span data-stu-id="b64a4-120">Add the following function in **graph.js** to get the user's profile.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="getUserSnippet":::

1. <span data-ttu-id="b64a4-121">将现有的 `signIn` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="b64a4-121">Replace the existing `signIn` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/auth.js" id="signInSnippet":::

1. <span data-ttu-id="b64a4-122">保存更改并刷新页面。</span><span class="sxs-lookup"><span data-stu-id="b64a4-122">Save your changes and refresh the page.</span></span> <span data-ttu-id="b64a4-123">登录后，应返回到主页，但 UI 应更改，以指示您已登录。</span><span class="sxs-lookup"><span data-stu-id="b64a4-123">After you sign in, you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![登录后主页的屏幕截图](./images/user-signed-in.png)

1. <span data-ttu-id="b64a4-125">单击右上角的用户头像以访问 " **注销** " 链接。</span><span class="sxs-lookup"><span data-stu-id="b64a4-125">Click the user avatar in the top right corner to access the **Sign out** link.</span></span> <span data-ttu-id="b64a4-126">单击 " **注销** " 重置会话并返回到主页。</span><span class="sxs-lookup"><span data-stu-id="b64a4-126">Clicking **Sign out** resets the session and returns you to the home page.</span></span>

    ![带有 "注销" 链接的下拉菜单的屏幕截图](./images/sign-out-button.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="b64a4-128">存储和刷新令牌</span><span class="sxs-lookup"><span data-stu-id="b64a4-128">Storing and refreshing tokens</span></span>

<span data-ttu-id="b64a4-129">此时，您的应用程序具有访问令牌，该令牌是在 `Authorization` API 调用的标头中发送的。</span><span class="sxs-lookup"><span data-stu-id="b64a4-129">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="b64a4-130">这是允许应用代表用户访问 Microsoft Graph 的令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-130">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="b64a4-131">但是，此令牌的生存期较短。</span><span class="sxs-lookup"><span data-stu-id="b64a4-131">However, this token is short-lived.</span></span> <span data-ttu-id="b64a4-132">令牌在发出后会过期一小时。</span><span class="sxs-lookup"><span data-stu-id="b64a4-132">The token expires an hour after it is issued.</span></span> <span data-ttu-id="b64a4-133">这就是刷新令牌变得有用的地方。</span><span class="sxs-lookup"><span data-stu-id="b64a4-133">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="b64a4-134">刷新令牌允许应用在不要求用户重新登录的情况下请求新的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-134">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="b64a4-135">由于应用程序使用的是 MSAL 库，因此您无需实现任何令牌存储或刷新逻辑。</span><span class="sxs-lookup"><span data-stu-id="b64a4-135">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="b64a4-136">MSAL 在浏览器会话中缓存令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-136">MSAL caches the token in the browser session.</span></span> <span data-ttu-id="b64a4-137">该 `acquireTokenSilent` 方法首先检查缓存的标记，如果它未过期，它将返回。</span><span class="sxs-lookup"><span data-stu-id="b64a4-137">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="b64a4-138">如果它已过期，则使用缓存的刷新令牌获取新的。</span><span class="sxs-lookup"><span data-stu-id="b64a4-138">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="b64a4-139">Graph 客户端对象在 `getToken` 每次请求时调用 **auth.js** 的方法，以确保它具有最新的令牌。</span><span class="sxs-lookup"><span data-stu-id="b64a4-139">The Graph client object calls the `getToken` method in **auth.js** on every request, ensuring that it has an up-to-date token.</span></span>
